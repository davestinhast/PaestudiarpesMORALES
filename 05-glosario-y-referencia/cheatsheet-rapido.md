# Cheat Sheet Referencia Rápida

Esto es lo que necesitas consultar rápido en el examen o en clase.

---

## Métodos HTTP

| Método | Acción | Tiene body? | Ejemplo |
|--------|--------|-------------|---------|
| GET | Leer | No | `GET /api/usuarios` |
| POST | Crear | Sí (JSON) | `POST /api/usuarios` |
| PUT | Actualizar todo | Sí (JSON) | `PUT /api/usuarios/5` |
| PATCH | Actualizar parte | Sí (JSON) | `PATCH /api/usuarios/5` |
| DELETE | Eliminar | No | `DELETE /api/usuarios/5` |

---

## Códigos de estado más importantes

| Código | Qué significa | Cuándo ocurre |
|--------|--------------|---------------|
| 200 | OK | Petición exitosa |
| 201 | Created | Recurso creado (POST exitoso) |
| 204 | No Content | Éxito sin datos que devolver |
| 400 | Bad Request | El cliente mandó datos incorrectos |
| 401 | Unauthorized | No estás autenticado |
| 403 | Forbidden | No tienes permiso |
| 404 | Not Found | El recurso no existe |
| 405 | Method Not Allowed | Método HTTP incorrecto |
| 409 | Conflict | Conflicto (ej: email duplicado) |
| 500 | Internal Server Error | Error en el código del servidor |

---

## Headers esenciales

```
// En las peticiones (Postman o código)
Content-Type: application/json Cuando mandas JSON en el body
Authorization: Bearer [token] Cuando necesitas autenticarte

// En las respuestas (tu PHP)
header('Content-Type: application/json');
header('Access-Control-Allow-Origin: *');
```

---

## PHP Snippets más usados

```php
// Leer método HTTP
$method = $_SERVER['REQUEST_METHOD'];

// Leer query parameter (?id=5)
$id = $_GET['id'] ?? null;

// Leer body JSON
$datos = json_decode(file_get_contents('php://input'), true);

// Enviar respuesta JSON
http_response_code(200);
echo json_encode(["status" => "success", "datos" => $resultado]);

// Conectar a MySQL con PDO
$pdo = new PDO("mysql:host=localhost;dbname=mi_bd", "root", "");

// SELECT todos
$stmt = $pdo->query("SELECT * FROM tabla");
$resultados = $stmt->fetchAll(PDO::FETCH_ASSOC);

// SELECT uno por ID (seguro)
$stmt = $pdo->prepare("SELECT * FROM tabla WHERE id = ?");
$stmt->execute([$id]);
$registro = $stmt->fetch(PDO::FETCH_ASSOC);

// INSERT
$stmt = $pdo->prepare("INSERT INTO tabla (col1, col2) VALUES (?, ?)");
$stmt->execute([$valor1, $valor2]);
$nuevoId = $pdo->lastInsertId();

// UPDATE
$stmt = $pdo->prepare("UPDATE tabla SET col1 = ?, col2 = ? WHERE id = ?");
$stmt->execute([$valor1, $valor2, $id]);

// DELETE
$stmt = $pdo->prepare("DELETE FROM tabla WHERE id = ?");
$stmt->execute([$id]);
```

---

## SQL Comandos esenciales

```sql
-- Base de datos
CREATE DATABASE nombre;
USE nombre;

-- Tablas
CREATE TABLE usuarios (
 id INT AUTO_INCREMENT PRIMARY KEY,
 nombre VARCHAR(100) NOT NULL
);
DESCRIBE usuarios;

-- Datos
SELECT * FROM usuarios;
SELECT * FROM usuarios WHERE id = 5;
SELECT * FROM usuarios WHERE nombre LIKE '%Carlos%';
SELECT * FROM usuarios ORDER BY nombre ASC LIMIT 10;

INSERT INTO usuarios (nombre) VALUES ('Ana');
UPDATE usuarios SET nombre = 'Ana García' WHERE id = 1;
DELETE FROM usuarios WHERE id = 1;

-- Útiles
SELECT COUNT(*) FROM usuarios;
SELECT MAX(id) FROM usuarios;
```

---

## Estructura de URL de API

```
http://localhost:8080/api/v1/usuarios/42?activo=true
|___| |___________| |_| |_| |______| |_| |________|
 1 2 3 4 5 6 7

1. Protocolo
2. Host:Puerto
3. Prefijo de API
4. Versión (opcional)
5. Recurso (nombre en plural)
6. ID del recurso específico
7. Query parameters
```

---

## Postman Pasos rápidos para POST

1. Método **POST**
2. URL `http://localhost:8080/api/recurso`
3. **Headers** `Content-Type: application/json`
4. **Body** **raw** **JSON**
5. Escribir el JSON
6. **Send**

---

## XAMPP Arrancar el servidor

1. Abrir XAMPP Control Panel
2. Click **Start** en Apache
3. Click **Start** en MySQL
4. Abrir navegador `http://localhost` o `http://localhost:8080`
5. phpMyAdmin `http://localhost/phpmyadmin`

---

## Errores frecuentes y solución

| Error | Causa más probable | Solución |
|-------|-------------------|----------|
| `Could not get response` en Postman | Apache no corre | Start Apache en XAMPP |
| 404 | URL equivocada o archivo no existe | Revisar la ruta |
| 500 | Error en PHP | Ver `xampp/apache/logs/error.log` |
| JSON null en PHP | Falta `Content-Type: application/json` | Agregarlo en Headers de Postman |
| `Access-Control-Allow-Origin` | CORS no configurado | Agregar headers en PHP |
| Puerto ocupado | Otro proceso usa el 80/8080 | Cambiar puerto en httpd.conf |
