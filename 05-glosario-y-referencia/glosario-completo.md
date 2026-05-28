# Glosario Completo De la A a la Z

Todo el vocabulario que necesitas para tus cursos de Programación de Servicios Web y Desarrollo de Aplicaciones Web.

---

## A

**Apache** 
El servidor web que viene con XAMPP. Es el programa que "escucha" en el puerto 80 u 8080 y responde cuando alguien accede a `localhost`. Sin Apache corriendo, no hay servidor web.

**API (Application Programming Interface)** 
Interfaz que permite que dos aplicaciones se comuniquen. En el contexto web, es un conjunto de URLs (endpoints) que un servidor expone para que las aplicaciones puedan pedir y enviar datos.

**Array** 
Lista de valores. En JSON: `["manzana", "pera", "uva"]`. En PHP: `["manzana", "pera", "uva"]`.

**Autenticación** 
El proceso de verificar *quién eres*. Cuando inicias sesión con tu usuario y contraseña, eso es autenticación.

**Autorización** 
El proceso de verificar *qué puedes hacer*. Después de autenticarte, el sistema decide si tienes permiso para realizar cierta acción.

---

## B

**Backend** 
La parte del software que corre en el servidor y no es visible para el usuario. Maneja la lógica, la base de datos y responde las peticiones de la API.

**Base de datos** 
Sistema que almacena datos de forma organizada. MySQL es el que usas con XAMPP. Los datos se guardan en tablas (como hojas de Excel).

**Body (cuerpo)** 
El contenido principal de una petición HTTP. En POST y PUT, aquí van los datos que quieres enviar al servidor. Normalmente en formato JSON.

**Boolean** 
Tipo de dato que solo puede ser `true` (verdadero) o `false` (falso).

---

## C

**CORS (Cross-Origin Resource Sharing)** 
Mecanismo de seguridad del navegador. Si tu página web en `localhost:3000` intenta llamar a una API en `localhost:8080`, el navegador lo bloquea por defecto. Para permitirlo, el servidor debe enviar el header `Access-Control-Allow-Origin: *`.

**CRUD** 
Las 4 operaciones básicas de cualquier sistema de datos:
- **C**reate POST
- **R**ead GET
- **U**pdate PUT/PATCH
- **D**elete DELETE

**Cliente** 
Quien hace la petición. Puede ser un navegador, Postman, una app móvil, etc.

---

## D

**DELETE** 
Método HTTP para eliminar un recurso. `DELETE /api/usuarios/5` elimina el usuario con ID 5.

**DSN (Data Source Name)** 
Cadena de conexión que le dice a PDO cómo conectarse a la base de datos. Ejemplo: `"mysql:host=localhost;dbname=mi_bd;charset=utf8"`.

---

## E

**Endpoint** 
Una URL específica de tu API que realiza una función concreta. Cada endpoint acepta ciertos métodos HTTP.

Ejemplos:
- `GET /api/usuarios` Endpoint para obtener usuarios
- `POST /api/productos` Endpoint para crear productos

**Estado (Status)** 
El resultado de una petición HTTP, representado por un código numérico (200, 404, 500...).

---

## F

**fetch()** 
Función de JavaScript para hacer peticiones HTTP desde el navegador (similar a lo que hace Postman, pero desde código).

**fetchAll()** 
Método de PDO que devuelve todos los resultados de una consulta como un array.

**fetch()** 
Método de PDO que devuelve solo el primer resultado de una consulta.

**Frontend** 
La parte visible del software. Lo que el usuario ve en su navegador o app. Se comunica con el backend a través de la API.

---

## G

**GET** 
Método HTTP para obtener datos. No tiene body. Los parámetros van en la URL.

---

## H

**Header (Cabecera)** 
Metadatos que viajan junto con una petición o respuesta HTTP. Dan contexto sobre los datos, como su formato, idioma, o información de autenticación.

Headers comunes:
- `Content-Type: application/json` Los datos son JSON
- `Authorization: Bearer token123` Token de autenticación

**HTTP (HyperText Transfer Protocol)** 
El protocolo de comunicación base de la web. Define cómo los clientes hacen peticiones y cómo los servidores responden.

**HTTPS** 
HTTP con cifrado SSL/TLS. Los datos viajan encriptados. En producción siempre deberías usar HTTPS.

**htdocs** 
La carpeta raíz de Apache en XAMPP. Todo lo que pones aquí es accesible desde `http://localhost`.

---

## I

**ID** 
Identificador único de un registro en la base de datos. Normalmente es un número que se incrementa automáticamente (AUTO_INCREMENT en MySQL).

**IP (Internet Protocol)** 
Protocolo de direccionamiento de redes. Cada dispositivo en una red tiene una IP. `127.0.0.1` siempre apunta a tu propia máquina (igual que `localhost`).

---

## J

**JSON (JavaScript Object Notation)** 
Formato de texto para intercambiar datos. Es el formato estándar de las APIs REST modernas. Fácil de leer por humanos y máquinas.

**JWT (JSON Web Token)** 
Tipo de token usado para autenticación en APIs. Tiene 3 partes separadas por puntos: `header.payload.signature`. Se usa como `Authorization: Bearer [JWT]`.

**json_encode()** 
Función de PHP que convierte un array o objeto PHP a una cadena JSON.

**json_decode()** 
Función de PHP que convierte una cadena JSON a un array o objeto PHP.

---

## L

**localhost** 
Nombre de dominio especial que siempre apunta a tu propia computadora. Es equivalente a la IP `127.0.0.1`.

**LOG** 
Archivo de registro donde Apache y otros servicios anotan lo que sucede: peticiones recibidas, errores, advertencias.

---

## M

**MySQL** 
Sistema de gestión de bases de datos relacional. Es el que viene incluido en XAMPP. Los datos se organizan en tablas con filas y columnas.

**Método HTTP** 
El "verbo" que indica qué acción quieres hacer: GET (leer), POST (crear), PUT (actualizar completo), PATCH (actualizar parcial), DELETE (eliminar).

---

## N

**NULL** 
Valor especial que representa "sin valor" o "vacío" en bases de datos y JSON. Es diferente de 0 o de una cadena vacía "".

---

## P

**PATCH** 
Método HTTP para actualizar parcialmente un recurso. Solo mandas los campos que quieres cambiar.

**PDO (PHP Data Objects)** 
Clase de PHP para conectarse a bases de datos de forma segura y portable. Permite usar `prepare()` para evitar inyección SQL.

**PHP** 
Lenguaje de programación de lado servidor. XAMPP lo incluye. Se usa para crear la lógica de las APIs y páginas web dinámicas.

**phpMyAdmin** 
Interfaz web para administrar MySQL visualmente. Disponible en `http://localhost/phpmyadmin` con XAMPP.

**Postman** 
Aplicación para probar APIs. Permite hacer cualquier tipo de petición HTTP, guardarlas, y organizarlas en colecciones.

**POST** 
Método HTTP para crear un nuevo recurso. Los datos van en el body de la petición.

**Puerto** 
Número que identifica un canal de comunicación específico en un servidor. Apache usa el 80 (HTTP) o 8080 (alternativo). MySQL usa el 3306.

**PUT** 
Método HTTP para actualizar completamente un recurso. Debes mandar todos los campos, no solo los que cambiaron.

---

## Q

**Query Parameter** 
Parámetro que va en la URL después del símbolo `?`. Se usan para filtrar, ordenar o paginar.
Ejemplo: `http://localhost/api/usuarios?activo=1&orden=nombre`

---

## R

**Request (Petición)** 
El mensaje que envía el cliente al servidor. Tiene un método, una URL, headers y opcionalmente un body.

**Response (Respuesta)** 
El mensaje que devuelve el servidor al cliente. Tiene un código de estado, headers y un body.

**REST (Representational State Transfer)** 
Estilo de arquitectura para diseñar APIs web. Usa HTTP, trabaja con recursos, y usa los métodos HTTP para las operaciones CRUD.

**RESTful** 
Adjetivo para una API que sigue los principios REST correctamente.

**Recurso** 
Cualquier "cosa" con la que trabaja tu API: un usuario, un producto, un pedido, etc. Cada recurso tiene su propia URL.

---

## S

**Servidor** 
Programa o computadora que recibe peticiones y devuelve respuestas. En tu caso, Apache es el servidor web y MySQL es el servidor de base de datos.

**SQL (Structured Query Language)** 
Lenguaje para hacer consultas a bases de datos relacionales. Se usa con MySQL.

**Status Code** 
Código numérico de 3 dígitos que indica el resultado de una petición HTTP. 2xx = éxito, 4xx = error del cliente, 5xx = error del servidor.

---

## T

**Token** 
Cadena de texto que funciona como una "llave" de acceso temporal. Se usa en autenticación para no tener que mandar usuario y contraseña en cada petición.

---

## U

**URL (Uniform Resource Locator)** 
La dirección de un recurso en internet (o en tu red local). Ejemplo: `http://localhost:8080/api/usuarios/5`

---

## V

**Variable de entorno** 
Variable de configuración que vive fuera del código. Se usa para datos sensibles como contraseñas o URLs que cambian entre entornos (desarrollo/producción).

---

## X

**XAMPP** 
Paquete que instala Apache, MySQL, PHP y Perl en tu computadora para tener un servidor web local.

**XML (eXtensible Markup Language)** 
Formato de texto para datos, similar a HTML. Es el formato que usa SOAP. Más verboso que JSON y menos usado en APIs modernas.

---

## Tabla rápida de referencia

| Concepto | Descripción corta |
|----------|-------------------|
| API | Puente de comunicación entre aplicaciones |
| REST | Estilo de diseño de APIs usando HTTP |
| HTTP | Protocolo de comunicación web |
| GET | Obtener datos |
| POST | Crear datos |
| PUT | Actualizar completamente |
| PATCH | Actualizar parcialmente |
| DELETE | Eliminar |
| JSON | Formato de datos para APIs |
| localhost | Tu propia computadora como servidor |
| Puerto 8080 | Puerto alternativo para el servidor web |
| XAMPP | Servidor local: Apache + MySQL + PHP |
| htdocs | Carpeta raíz del servidor Apache |
| Postman | Herramienta para probar APIs |
| PDO | Clase PHP para base de datos segura |
| CRUD | Create, Read, Update, Delete |
| Header | Metadatos de la petición/respuesta |
| Body | Contenido principal de la petición |
| Status 200 | Éxito |
| Status 201 | Creado exitosamente |
| Status 400 | Error del cliente (datos incorrectos) |
| Status 404 | No encontrado |
| Status 500 | Error del servidor |
