# Ejemplo Práctico Completo API de Usuarios

Este es un ejemplo funcional completo de una API REST en PHP con XAMPP. Puedes copiarlo y probarlo directamente.

## Estructura de archivos

```
htdocs/
 api-usuarios/
 config/
 database.php Conexión a MySQL
 api/
 usuarios.php La API
 README.txt Notas
```

---

## Paso 1 Base de datos (ejecutar en phpMyAdmin)

```sql
-- Crear la base de datos
CREATE DATABASE IF NOT EXISTS api_ejemplo CHARACTER SET utf8 COLLATE utf8_general_ci;

-- Usar la base de datos
USE api_ejemplo;

-- Crear la tabla de usuarios
CREATE TABLE IF NOT EXISTS usuarios (
 id INT AUTO_INCREMENT PRIMARY KEY,
 nombre VARCHAR(100) NOT NULL,
 apellido VARCHAR(100) NOT NULL,
 email VARCHAR(150) NOT NULL UNIQUE,
 edad INT DEFAULT NULL,
 activo TINYINT(1) DEFAULT 1,
 creado_en TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
 actualizado_en TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Insertar datos de ejemplo
INSERT INTO usuarios (nombre, apellido, email, edad) VALUES
('Carlos', 'Ramírez', 'carlos@gmail.com', 28),
('Sofía', 'Herández', 'sofia@gmail.com', 24),
('Miguel', 'Torres', 'miguel@hotmail.com', 31),
('Valentina','López', 'vale@gmail.com', 22),
('Andrés', 'Jiménez', 'andres@gmail.com', 35);
```

---

## Paso 2 Archivo de conexión

**`config/database.php`**

```php
<?php

// Configuración de la base de datos
define('DB_HOST', 'localhost');
define('DB_NAME', 'api_ejemplo');
define('DB_USER', 'root');
define('DB_PASS', '');
define('DB_CHARSET', 'utf8');

// Función para obtener la conexión
function getConexion() {
 static $pdo = null; // Guardar la conexión para no crear una nueva cada vez

 if ($pdo === null) {
 try {
 $dsn = "mysql:host=" . DB_HOST . ";dbname=" . DB_NAME . ";charset=" . DB_CHARSET;

 $pdo = new PDO($dsn, DB_USER, DB_PASS, [
 PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
 PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
 PDO::ATTR_EMULATE_PREPARES => false,
 ]);

 } catch(PDOException $e) {
 responder([
 "status" => "error",
 "mensaje" => "Error de conexión a la base de datos"
 ], 500);
 }
 }

 return $pdo;
}

// Función helper para enviar respuestas
function responder($datos, $codigo = 200) {
 http_response_code($codigo);
 echo json_encode($datos, JSON_UNESCAPED_UNICODE | JSON_PRETTY_PRINT);
 exit();
}
```

---

## Paso 3 La API completa

**`api/usuarios.php`**

```php
<?php

// =============================================
// HEADERS Siempre van primero
// =============================================
header('Content-Type: application/json; charset=utf-8');
header('Access-Control-Allow-Origin: *');
header('Access-Control-Allow-Methods: GET, POST, PUT, PATCH, DELETE, OPTIONS');
header('Access-Control-Allow-Headers: Content-Type, Authorization');

// Responder a peticiones OPTIONS (necesario para CORS)
if ($_SERVER['REQUEST_METHOD'] === 'OPTIONS') {
 http_response_code(204); // No Content
 exit();
}

// =============================================
// DEPENDENCIAS
// =============================================
require_once '../config/database.php';

// =============================================
// OBTENER INFORMACIÓN DE LA PETICIÓN
// =============================================
$pdo = getConexion();
$method = $_SERVER['REQUEST_METHOD']; // GET, POST, PUT, DELETE...
$id = isset($_GET['id']) ? (int) $_GET['id'] : null;

// =============================================
// ENRUTAMIENTO Según el método, ejecutar función
// =============================================
switch ($method) {
 case 'GET':
 obtenerUsuarios($pdo, $id);
 break;

 case 'POST':
 crearUsuario($pdo);
 break;

 case 'PUT':
 actualizarUsuarioCompleto($pdo, $id);
 break;

 case 'PATCH':
 actualizarUsuarioParcial($pdo, $id);
 break;

 case 'DELETE':
 eliminarUsuario($pdo, $id);
 break;

 default:
 responder(["error" => "Método HTTP no permitido"], 405);
}


// =============================================
// GET Obtener uno o todos los usuarios
// =============================================
function obtenerUsuarios($pdo, $id) {

 if ($id !== null) {
 // Buscar usuario específico por ID
 $stmt = $pdo->prepare("SELECT * FROM usuarios WHERE id = ?");
 $stmt->execute([$id]);
 $usuario = $stmt->fetch();

 if (!$usuario) {
 responder([
 "status" => "error",
 "mensaje" => "No existe un usuario con el ID $id"
 ], 404);
 }

 responder([
 "status" => "success",
 "datos" => $usuario
 ], 200);

 } else {
 // Devolver todos los usuarios
 // Soporte para filtro por nombre: ?nombre=Carlos
 if (isset($_GET['nombre'])) {
 $busqueda = "%" . $_GET['nombre'] . "%";
 $stmt = $pdo->prepare(
 "SELECT * FROM usuarios WHERE nombre LIKE ? OR apellido LIKE ? ORDER BY nombre ASC"
 );
 $stmt->execute([$busqueda, $busqueda]);
 } else {
 $stmt = $pdo->query("SELECT * FROM usuarios ORDER BY id ASC");
 }

 $usuarios = $stmt->fetchAll();

 responder([
 "status" => "success",
 "total" => count($usuarios),
 "datos" => $usuarios
 ], 200);
 }
}


// =============================================
// POST Crear usuario nuevo
// =============================================
function crearUsuario($pdo) {

 // Leer y decodificar el body JSON
 $body = file_get_contents('php://input');
 $datos = json_decode($body, true);

 // Verificar que el JSON sea válido
 if (json_last_error() !== JSON_ERROR_NONE) {
 responder(["error" => "El body no es JSON válido"], 400);
 }

 // Validar campos obligatorios
 $camposRequeridos = ['nombre', 'apellido', 'email'];
 $errores = [];

 foreach ($camposRequeridos as $campo) {
 if (!isset($datos[$campo]) || trim($datos[$campo]) === '') {
 $errores[] = "El campo '$campo' es obligatorio";
 }
 }

 if (!empty($errores)) {
 responder(["errores" => $errores], 400);
 }

 // Validar formato de email
 if (!filter_var($datos['email'], FILTER_VALIDATE_EMAIL)) {
 responder(["error" => "El email no tiene un formato válido"], 400);
 }

 // Verificar si el email ya existe
 $check = $pdo->prepare("SELECT id FROM usuarios WHERE email = ?");
 $check->execute([$datos['email']]);

 if ($check->fetch()) {
 responder(["error" => "Ya existe un usuario con ese email"], 409); // 409 Conflict
 }

 // Insertar el nuevo usuario
 $stmt = $pdo->prepare(
 "INSERT INTO usuarios (nombre, apellido, email, edad) VALUES (?, ?, ?, ?)"
 );

 $stmt->execute([
 trim($datos['nombre']),
 trim($datos['apellido']),
 trim($datos['email']),
 $datos['edad'] ?? null
 ]);

 $nuevoId = (int) $pdo->lastInsertId();

 // Obtener el usuario recién creado para devolverlo
 $stmt2 = $pdo->prepare("SELECT * FROM usuarios WHERE id = ?");
 $stmt2->execute([$nuevoId]);
 $nuevoUsuario = $stmt2->fetch();

 responder([
 "status" => "success",
 "mensaje" => "Usuario creado exitosamente",
 "datos" => $nuevoUsuario
 ], 201); // 201 Created
}


// =============================================
// PUT Actualizar usuario completamente
// =============================================
function actualizarUsuarioCompleto($pdo, $id) {

 if (!$id) {
 responder(["error" => "Se requiere el ID del usuario (?id=X)"], 400);
 }

 // Verificar que el usuario existe
 $check = $pdo->prepare("SELECT id FROM usuarios WHERE id = ?");
 $check->execute([$id]);

 if (!$check->fetch()) {
 responder(["error" => "No existe un usuario con el ID $id"], 404);
 }

 // Leer body
 $datos = json_decode(file_get_contents('php://input'), true);

 // Validar campos obligatorios (PUT = todos los campos)
 $camposRequeridos = ['nombre', 'apellido', 'email'];
 $errores = [];

 foreach ($camposRequeridos as $campo) {
 if (!isset($datos[$campo]) || trim($datos[$campo]) === '') {
 $errores[] = "El campo '$campo' es obligatorio en PUT";
 }
 }

 if (!empty($errores)) {
 responder(["errores" => $errores], 400);
 }

 // Verificar que el email no esté usado por OTRO usuario
 $checkEmail = $pdo->prepare("SELECT id FROM usuarios WHERE email = ? AND id != ?");
 $checkEmail->execute([$datos['email'], $id]);

 if ($checkEmail->fetch()) {
 responder(["error" => "Ese email ya está en uso por otro usuario"], 409);
 }

 // Actualizar
 $stmt = $pdo->prepare(
 "UPDATE usuarios SET nombre = ?, apellido = ?, email = ?, edad = ?, activo = ? WHERE id = ?"
 );

 $stmt->execute([
 trim($datos['nombre']),
 trim($datos['apellido']),
 trim($datos['email']),
 $datos['edad'] ?? null,
 isset($datos['activo']) ? (int) $datos['activo'] : 1,
 $id
 ]);

 // Devolver el usuario actualizado
 $stmt2 = $pdo->prepare("SELECT * FROM usuarios WHERE id = ?");
 $stmt2->execute([$id]);

 responder([
 "status" => "success",
 "mensaje" => "Usuario actualizado completamente",
 "datos" => $stmt2->fetch()
 ], 200);
}


// =============================================
// PATCH Actualizar usuario parcialmente
// =============================================
function actualizarUsuarioParcial($pdo, $id) {

 if (!$id) {
 responder(["error" => "Se requiere el ID del usuario (?id=X)"], 400);
 }

 // Verificar que existe
 $check = $pdo->prepare("SELECT * FROM usuarios WHERE id = ?");
 $check->execute([$id]);
 $usuarioActual = $check->fetch();

 if (!$usuarioActual) {
 responder(["error" => "No existe un usuario con el ID $id"], 404);
 }

 // Leer body
 $datos = json_decode(file_get_contents('php://input'), true);

 if (empty($datos)) {
 responder(["error" => "Debes enviar al menos un campo para actualizar"], 400);
 }

 // Construir query dinámicamente (solo los campos que llegaron)
 $camposPermitidos = ['nombre', 'apellido', 'email', 'edad', 'activo'];
 $setClauses = [];
 $valores = [];

 foreach ($camposPermitidos as $campo) {
 if (array_key_exists($campo, $datos)) {
 $setClauses[] = "$campo = ?";
 $valores[] = $datos[$campo];
 }
 }

 if (empty($setClauses)) {
 responder(["error" => "Ningún campo válido para actualizar"], 400);
 }

 $valores[] = $id; // El ID va al final para el WHERE
 $sql = "UPDATE usuarios SET " . implode(', ', $setClauses) . " WHERE id = ?";

 $stmt = $pdo->prepare($sql);
 $stmt->execute($valores);

 // Devolver el usuario actualizado
 $stmt2 = $pdo->prepare("SELECT * FROM usuarios WHERE id = ?");
 $stmt2->execute([$id]);

 responder([
 "status" => "success",
 "mensaje" => "Usuario actualizado parcialmente",
 "datos" => $stmt2->fetch()
 ], 200);
}


// =============================================
// DELETE Eliminar usuario
// =============================================
function eliminarUsuario($pdo, $id) {

 if (!$id) {
 responder(["error" => "Se requiere el ID del usuario (?id=X)"], 400);
 }

 // Verificar que existe
 $check = $pdo->prepare("SELECT id, nombre FROM usuarios WHERE id = ?");
 $check->execute([$id]);
 $usuario = $check->fetch();

 if (!$usuario) {
 responder(["error" => "No existe un usuario con el ID $id"], 404);
 }

 // Eliminar
 $stmt = $pdo->prepare("DELETE FROM usuarios WHERE id = ?");
 $stmt->execute([$id]);

 responder([
 "status" => "success",
 "mensaje" => "Usuario '{$usuario['nombre']}' eliminado exitosamente"
 ], 200);
}
```

---

## Paso 4 Probar en Postman

### 1. Obtener todos los usuarios
```
GET http://localhost/api-usuarios/api/usuarios.php
```
Respuesta esperada:
```json
{
 "status": "success",
 "total": 5,
 "datos": [...]
}
```

### 2. Obtener usuario por ID
```
GET http://localhost/api-usuarios/api/usuarios.php?id=1
```

### 3. Buscar por nombre
```
GET http://localhost/api-usuarios/api/usuarios.php?nombre=carlos
```

### 4. Crear usuario
```
POST http://localhost/api-usuarios/api/usuarios.php
Headers: Content-Type: application/json
Body:
{
 "nombre": "Pedro",
 "apellido": "Sánchez",
 "email": "pedro@gmail.com",
 "edad": 29
}
```

### 5. Actualizar completamente (PUT)
```
PUT http://localhost/api-usuarios/api/usuarios.php?id=1
Headers: Content-Type: application/json
Body:
{
 "nombre": "Carlos Alberto",
 "apellido": "Ramírez Vega",
 "email": "carlos.nuevo@gmail.com",
 "edad": 29,
 "activo": 1
}
```

### 6. Actualizar parcialmente (PATCH)
```
PATCH http://localhost/api-usuarios/api/usuarios.php?id=1
Headers: Content-Type: application/json
Body:
{
 "email": "carlos.actualizado@gmail.com"
}
```

### 7. Eliminar usuario
```
DELETE http://localhost/api-usuarios/api/usuarios.php?id=5
```

---

## Respuestas de error esperadas

### Usuario no encontrado
```json
{
 "status": "error",
 "mensaje": "No existe un usuario con el ID 99"
}
```
*Código: 404*

### Campos faltantes al crear
```json
{
 "errores": [
 "El campo 'nombre' es obligatorio",
 "El campo 'email' es obligatorio"
 ]
}
```
*Código: 400*

### Email duplicado
```json
{
 "error": "Ya existe un usuario con ese email"
}
```
*Código: 409*
