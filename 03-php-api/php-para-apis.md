# PHP para APIs Todo lo que necesitas saber

## ¿Por qué PHP para APIs?

PHP es un lenguaje de servidor que:
- **Ya viene incluido en XAMPP.**
- Es fácil de aprender.
- Tiene funciones nativas para manejar JSON.
- Tiene PDO para conectarse a MySQL de forma segura.
- Corre en Apache directamente.

---

## Estructura básica de un archivo PHP de API

```php
<?php
// 1. Configurar headers (siempre primero)
header('Content-Type: application/json');
header('Access-Control-Allow-Origin: *');

// 2. Conectar a la base de datos
require_once 'config/database.php';

// 3. Leer el método HTTP
$metodo = $_SERVER['REQUEST_METHOD'];

// 4. Tomar decisiones según el método
if ($metodo === 'GET') {
 // hacer algo
} elseif ($metodo === 'POST') {
 // hacer otra cosa
}
```

---

## Variables superglobales de PHP que usarás

| Variable | ¿Para qué? | Ejemplo |
|----------|------------|---------|
| `$_SERVER['REQUEST_METHOD']` | Saber qué método HTTP llegó | `"GET"`, `"POST"`, etc. |
| `$_GET['nombre']` | Leer query parameters de la URL | `?nombre=Carlos` |
| `$_POST['nombre']` | Leer datos de formulario HTML normal | |
| `file_get_contents('php://input')` | Leer el body de la petición (JSON) | |
| `$_SERVER['REQUEST_URI']` | La URL completa de la petición | `/api/usuarios?id=5` |

---

## Trabajar con JSON en PHP

### Convertir PHP JSON (para responder)

```php
$datos = [
 "status" => "success",
 "usuarios" => [
 ["id" => 1, "nombre" => "Ana"],
 ["id" => 2, "nombre" => "Luis"]
 ]
];

echo json_encode($datos);
// Salida: {"status":"success","usuarios":[{"id":1,"nombre":"Ana"},{"id":2,"nombre":"Luis"}]}

// Con formato bonito (para depurar):
echo json_encode($datos, JSON_PRETTY_PRINT);
```

### Convertir JSON PHP (para recibir del cliente)

```php
// Leer el body crudo de la petición
$body = file_get_contents('php://input');

// Convertir JSON a array de PHP (true = array asociativo)
$datos = json_decode($body, true);

// Ahora puedes usar $datos como un array normal
echo $datos['nombre']; // "Ana"
echo $datos['email']; // "ana@gmail.com"
```

### Verificar si el JSON es válido

```php
$body = file_get_contents('php://input');
$datos = json_decode($body, true);

if (json_last_error() !== JSON_ERROR_NONE) {
 http_response_code(400);
 echo json_encode(["error" => "JSON inválido"]);
 exit();
}
```

---

## PDO Conexión segura a MySQL

**PDO (PHP Data Objects)** es la forma moderna y segura de conectarse a bases de datos en PHP.

### Conexión

```php
<?php
// Datos de conexión
$host = "localhost";
$dbname = "mi_base_de_datos";
$user = "root";
$password = "";

try {
 // Crear la conexión
 $pdo = new PDO(
 "mysql:host=$host;dbname=$dbname;charset=utf8",
 $user,
 $password
 );

 // Modo de errores: que lance excepciones
 $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);

 // Que los resultados sean arrays asociativos por defecto
 $pdo->setAttribute(PDO::ATTR_DEFAULT_FETCH_MODE, PDO::FETCH_ASSOC);

} catch(PDOException $e) {
 http_response_code(500);
 echo json_encode(["error" => "Error de conexión: " . $e->getMessage()]);
 exit();
}
```

### Consultas con PDO

#### SELECT Obtener datos

```php
// Obtener todos los registros
$stmt = $pdo->query("SELECT * FROM usuarios");
$usuarios = $stmt->fetchAll(); // Array con todos los resultados

// Obtener un solo registro
$stmt = $pdo->query("SELECT * FROM usuarios WHERE id = 1");
$usuario = $stmt->fetch(); // Un solo array

// Con parámetros (¡SIEMPRE usa prepare cuando hay variables!)
$id = 5;
$stmt = $pdo->prepare("SELECT * FROM usuarios WHERE id = ?");
$stmt->execute([$id]);
$usuario = $stmt->fetch();
```

#### ¿Por qué `prepare`? Seguridad

**Sin prepare (PELIGROSO SQL Injection):**
```php
$id = $_GET['id']; // Un atacante podría mandar: 1 OR 1=1 -- 
$stmt = $pdo->query("SELECT * FROM usuarios WHERE id = $id");
// La consulta quedaría: SELECT * FROM usuarios WHERE id = 1 OR 1=1 --
// ¡Esto devuelve TODOS los usuarios!
```

**Con prepare (SEGURO):**
```php
$id = $_GET['id'];
$stmt = $pdo->prepare("SELECT * FROM usuarios WHERE id = ?");
$stmt->execute([$id]);
// PDO escapa automáticamente el valor, sin importar lo que manden
```

#### INSERT Crear

```php
$stmt = $pdo->prepare(
 "INSERT INTO usuarios (nombre, email, edad) VALUES (?, ?, ?)"
);
$stmt->execute([$nombre, $email, $edad]);

// Obtener el ID del registro recién insertado
$nuevoId = $pdo->lastInsertId();
```

#### UPDATE Actualizar

```php
$stmt = $pdo->prepare(
 "UPDATE usuarios SET nombre = ?, email = ? WHERE id = ?"
);
$stmt->execute([$nombre, $email, $id]);

// Saber cuántas filas se actualizaron
$filasAfectadas = $stmt->rowCount();
```

#### DELETE Eliminar

```php
$stmt = $pdo->prepare("DELETE FROM usuarios WHERE id = ?");
$stmt->execute([$id]);

$filasEliminadas = $stmt->rowCount(); // 0 si no existía, 1 si se eliminó
```

---

## Funciones útiles de PHP para APIs

### http_response_code()

Establece el código de estado HTTP de la respuesta.

```php
http_response_code(200); // OK
http_response_code(201); // Created
http_response_code(400); // Bad Request
http_response_code(404); // Not Found
http_response_code(500); // Internal Server Error
```

**Siempre llámala ANTES de hacer `echo`.**

### isset() Verificar si existe una variable

```php
// Verificar si un query parameter existe
if (isset($_GET['id'])) {
 $id = $_GET['id'];
} else {
 // No mandaron id
}

// Forma corta con el operador ??
$id = $_GET['id'] ?? null; // Si no existe, $id = null
```

### empty() Verificar si está vacío

```php
$nombre = "";

if (empty($nombre)) {
 echo "El nombre no puede estar vacío";
}
```

### Validar que un campo exista y no esté vacío

```php
$datos = json_decode(file_get_contents('php://input'), true);

$errores = [];

if (!isset($datos['nombre']) || empty($datos['nombre'])) {
 $errores[] = "El nombre es obligatorio";
}

if (!isset($datos['email']) || empty($datos['email'])) {
 $errores[] = "El email es obligatorio";
}

if (!empty($errores)) {
 http_response_code(400);
 echo json_encode(["errores" => $errores]);
 exit();
}
```

---

## Estructura completa de una API PHP

```php
<?php

// ====================================================
// CONFIGURACIÓN DE HEADERS
// ====================================================
header('Content-Type: application/json; charset=utf-8');
header('Access-Control-Allow-Origin: *');
header('Access-Control-Allow-Methods: GET, POST, PUT, DELETE, OPTIONS');
header('Access-Control-Allow-Headers: Content-Type');

// Manejar peticiones OPTIONS (pre-flight de CORS)
if ($_SERVER['REQUEST_METHOD'] === 'OPTIONS') {
 http_response_code(200);
 exit();
}

// ====================================================
// CONEXIÓN A LA BASE DE DATOS
// ====================================================
require_once '../config/database.php';

// ====================================================
// OBTENER MÉTODO Y PARÁMETROS
// ====================================================
$method = $_SERVER['REQUEST_METHOD'];
$id = isset($_GET['id']) ? (int)$_GET['id'] : null;

// ====================================================
// ENRUTAMIENTO
// ====================================================
switch ($method) {
 case 'GET':
 handleGet($pdo, $id);
 break;
 case 'POST':
 handlePost($pdo);
 break;
 case 'PUT':
 handlePut($pdo, $id);
 break;
 case 'PATCH':
 handlePatch($pdo, $id);
 break;
 case 'DELETE':
 handleDelete($pdo, $id);
 break;
 default:
 http_response_code(405);
 echo json_encode(["error" => "Método no permitido"]);
 break;
}

// ====================================================
// FUNCIONES
// ====================================================

function handleGet($pdo, $id) {
 // lógica de GET
}

function handlePost($pdo) {
 // lógica de POST
}

function handlePut($pdo, $id) {
 // lógica de PUT
}

function handlePatch($pdo, $id) {
 // lógica de PATCH
}

function handleDelete($pdo, $id) {
 // lógica de DELETE
}
```

---

## Función helper para responder

En vez de repetir `http_response_code` y `json_encode` por todos lados, crea una función:

```php
function responder($datos, $codigo = 200) {
 http_response_code($codigo);
 echo json_encode($datos, JSON_UNESCAPED_UNICODE);
 exit();
}

// Uso:
responder(["status" => "success", "datos" => $usuarios], 200);
responder(["error" => "No encontrado"], 404);
responder(["error" => "Error del servidor"], 500);
```

`JSON_UNESCAPED_UNICODE` hace que los caracteres como `ñ`, `é`, `á` se muestren correctamente en el JSON en vez de como `ñ`, `é`, etc.
