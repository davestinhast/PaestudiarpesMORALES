# GUÍA COMPLETA DE APIs PARA ESTUDIAR Y NO MORIR EN EL INTENTO

> **Cursos:** Programación de Servicios Web · Desarrollo de Aplicaciones Web 
> **Herramientas usadas:** XAMPP · Postman · Localhost:8080 
> **Nivel:** Desde cero. Léxico simple. Sin complicaciones.

---

## ÍNDICE GENERAL

1. [¿Qué es una API?](#1--qué-es-una-api)
2. [¿Qué es HTTP?](#2--qué-es-http)
3. [Métodos HTTP (verbos)](#3--métodos-http-los-verbos-de-la-api)
4. [XAMPP y la carpeta htdocs](#4--xampp-y-la-carpeta-htdocs)
5. [Localhost y puertos](#5--localhost-y-puertos)
6. [¿Qué es Postman?](#6--qué-es-postman-y-para-qué-sirve)
7. [JSON el idioma de las APIs](#7--json--el-idioma-de-las-apis)
8. [Códigos de estado HTTP](#8--códigos-de-estado-http)
9. [Estructura de una URL de API](#9--estructura-de-una-url-de-api)
10. [Headers y Body](#10--headers-y-body)
11. [Parámetros en las peticiones](#11--parámetros-en-las-peticiones)
12. [Ejemplo completo paso a paso](#12--ejemplo-completo-paso-a-paso)
13. [Comandos útiles](#13--comandos-útiles)
14. [Errores comunes y cómo resolverlos](#14--errores-comunes-y-cómo-resolverlos)
15. [Glosario de palabras importantes](#15--glosario-de-palabras-importantes)

---

## 1. ¿Qué es una API?

**API** son las siglas de **Application Programming Interface** (Interfaz de Programación de Aplicaciones).

En palabras simples: **una API es como un mesero en un restaurante**.

Imagina esto:
- **Tú** eres el cliente.
- **La cocina** es el servidor (donde están los datos y la lógica).
- **El mesero** es la API.

Tú no puedes entrar directamente a la cocina. No sabes cómo funciona. Solo le dices al mesero lo que quieres, el mesero va a la cocina, trae lo que pediste, y te lo entrega.

Así funciona una API: **tú haces una petición, la API la procesa, y te devuelve una respuesta.**

### ¿Para qué sirve en la vida real?

- Cuando entras a Spotify y le das play a una canción tu app le manda una petición a la API de Spotify.
- Cuando inicias sesión con Google en cualquier página esa página usa la API de Google.
- Cuando un mapa te muestra tu ubicación usa la API de Google Maps.
- Cuando un ecommerce procesa tu pago usa la API de PayPal o Stripe.

### Tipos de APIs

| Tipo | ¿Qué es? |
|------|----------|
| **REST** | El tipo más común hoy en día. Usa HTTP. Devuelve datos en formato JSON o XML. |
| **SOAP** | Más antiguo. Usa XML. Más difícil de usar. Ya casi no se usa. |
| **GraphQL** | Más moderno que REST. Tú decides exactamente qué datos quieres. |
| **WebSocket** | Para comunicación en tiempo real (chats, juegos online). |

> En tus cursos lo más probable es que trabajes con **REST**. Es el estándar moderno.

---

## 2. ¿Qué es HTTP?

**HTTP** son las siglas de **HyperText Transfer Protocol** (Protocolo de Transferencia de HiperTexto).

Es básicamente **el idioma que usan los navegadores y los servidores para comunicarse entre sí.**

### HTTP vs HTTPS

| | Significado | Diferencia |
|---|---|---|
| **HTTP** | HyperText Transfer Protocol | Sin cifrado. Lo que envías se puede leer. |
| **HTTPS** | HTTP Secure | Con cifrado. Lo que envías está protegido. |

En desarrollo local (XAMPP, localhost) usamos **HTTP** sin problema porque nadie más está viendo el tráfico.

### ¿Cómo funciona una petición HTTP?

```
CLIENTE (tú / Postman / app)
 |
 |--- Manda una PETICIÓN (request) --->
 |
 SERVIDOR (XAMPP, Node.js, etc.)
 |
 |<-- Responde con una RESPUESTA (response) ---
```

Cada petición tiene:
- Un **método** (qué quieres hacer)
- Una **URL** (a dónde vas)
- **Headers** (información extra)
- Un **body** (lo que mandas, opcional)

---

## 3. Métodos HTTP (los "verbos" de la API)

Los métodos HTTP le dicen al servidor **qué acción quieres hacer**. Son como verbos en español.

| Método | ¿Qué hace? | Ejemplo de uso |
|--------|------------|----------------|
| **GET** | **Obtener** información | "Dame la lista de usuarios" |
| **POST** | **Crear** algo nuevo | "Crea un usuario nuevo" |
| **PUT** | **Actualizar** completamente algo | "Reemplaza todos los datos de este usuario" |
| **PATCH** | **Actualizar** solo una parte | "Cambia solo el email de este usuario" |
| **DELETE** | **Eliminar** algo | "Borra este usuario" |

### Explicación con ejemplos de la vida real

#### GET Pedir información
```
GET /api/usuarios
```
Esto le dice al servidor: *"Dame todos los usuarios que tienes."*

Es como buscar en Google solo estás **pidiendo** información, no cambiando nada.

---

#### POST Crear algo nuevo
```
POST /api/usuarios
Body: { "nombre": "Juan", "email": "juan@gmail.com" }
```
Esto le dice al servidor: *"Crea un usuario nuevo con estos datos."*

Es como llenar un formulario de registro estás **enviando** datos para crear algo.

---

#### PUT Reemplazar completamente
```
PUT /api/usuarios/5
Body: { "nombre": "Juan Carlos", "email": "juancarlos@gmail.com" }
```
Esto le dice al servidor: *"El usuario número 5, reemplázalo por completo con esto."*

Si antes tenía un campo `telefono` y no lo mandas en el PUT, ese campo desaparece.

---

#### PATCH Actualizar solo una parte
```
PATCH /api/usuarios/5
Body: { "email": "nuevoemail@gmail.com" }
```
Esto le dice al servidor: *"Solo cambia el email del usuario 5, deja todo lo demás igual."*

---

#### DELETE Eliminar
```
DELETE /api/usuarios/5
```
Esto le dice al servidor: *"Borra el usuario número 5."*

---

### Regla para recordarlos: **CRUD**

| Letra | Operación | Método HTTP |
|-------|-----------|-------------|
| **C** | Create (Crear) | POST |
| **R** | Read (Leer) | GET |
| **U** | Update (Actualizar) | PUT / PATCH |
| **D** | Delete (Eliminar) | DELETE |

**CRUD** es el concepto base de cualquier sistema de datos. Si tu API hace estas 4 cosas, ya tienes todo lo necesario.

---

## 4. XAMPP y la carpeta htdocs

### ¿Qué es XAMPP?

**XAMPP** es un programa que instala en tu computadora todo lo necesario para tener un servidor web funcionando **de forma local** (sin internet, solo en tu máquina).

El nombre XAMPP viene de:

| Letra | Significa | ¿Qué es? |
|-------|-----------|----------|
| **X** | Cross-platform | Funciona en Windows, Mac y Linux |
| **A** | Apache | El servidor web (el que escucha las peticiones) |
| **M** | MariaDB / MySQL | La base de datos (donde se guardan los datos) |
| **P** | PHP | Lenguaje de programación para el servidor |
| **P** | Perl | Otro lenguaje de programación (menos usado) |

### ¿Para qué sirve cada parte?

**Apache:** Es el servidor. Cuando tú abres el navegador y escribes `localhost`, Apache es quien responde. Piensa en él como el edificio donde vive tu aplicación.

**MySQL/MariaDB:** Es la base de datos. Aquí guardas usuarios, productos, pedidos, lo que sea. Es como una hoja de Excel gigante pero mucho más poderosa.

**PHP:** Es el lenguaje que hace la lógica del servidor. Recibe la petición, consulta la base de datos, y devuelve la respuesta.

---

### ¿Qué es htdocs?

`htdocs` es **la carpeta raíz de Apache**. Todo lo que pongas ahí, Apache lo puede servir.

**Ubicación típica:**
```
Windows: C:\xampp\htdocs\
Mac/Linux: /opt/lampp/htdocs/
```

Si creas un archivo en:
```
C:\xampp\htdocs\mi-proyecto\index.php
```

Entonces puedes verlo en el navegador en:
```
http://localhost/mi-proyecto/index.php
```

### Estructura de una API en htdocs

Así se veía probablemente tu proyecto en htdocs:

```
htdocs/
 mi-api/
 index.php Punto de entrada principal
 config/
 database.php Configuración de la base de datos
 models/
 Usuario.php Lógica de los datos
 controllers/
 UsuarioController.php Lógica de negocio
 api/
 usuarios.php Endpoint de la API
```

### Pasos para arrancar XAMPP

1. Abrir el **Panel de Control de XAMPP**.
2. Hacer clic en **Start** en Apache.
3. Hacer clic en **Start** en MySQL (si usas base de datos).
4. Cuando ambos estén en verde con su puerto visible, está funcionando.
5. Abrir el navegador y escribir `http://localhost` debería aparecer la página de inicio de XAMPP.

---

## 5. Localhost y puertos

### ¿Qué es localhost?

**localhost** es simplemente **tu propia computadora**, vista como un servidor.

Cuando escribes `localhost` en el navegador, le estás diciendo: *"Conectame al servidor que corre en esta misma máquina."*

`localhost` es igual a escribir `127.0.0.1` es la dirección IP que siempre apunta a tu propio dispositivo.

### ¿Qué es un puerto?

Un puerto es como **la puerta específica** por donde entra la comunicación.

Tu computadora tiene miles de puertas (puertos) numeradas del 0 al 65535. Cada servicio usa una puerta diferente para no pisarse.

```
localhost:8080
 | |
 | Puerto: la puerta número 8080
 localhost: tu computadora
```

### Puertos más comunes que vas a ver

| Puerto | ¿Para qué se usa? |
|--------|-------------------|
| **80** | HTTP (web normal, Apache por defecto) |
| **443** | HTTPS (web segura) |
| **3306** | MySQL / MariaDB (base de datos) |
| **8080** | Servidor web alternativo (muy usado en desarrollo) |
| **3000** | Node.js / React (por defecto) |
| **5000** | Flask (Python) por defecto |
| **4200** | Angular por defecto |
| **8000** | Django (Python) / Laravel por defecto |

### ¿Por qué usamos el 8080 y no el 80?

Apache por defecto usa el puerto 80. Pero a veces ese puerto está ocupado por otro programa (como IIS en Windows). Por eso mucha gente configura Apache o su API en el **puerto 8080** para evitar conflictos.

Si tu API corre en el puerto 8080, las URLs serían así:
```
http://localhost:8080/api/usuarios
http://localhost:8080/api/productos
```

### Cómo cambiar el puerto de Apache en XAMPP

1. Abre XAMPP Panel.
2. Clic en **Config** de Apache.
3. Abre **httpd.conf**.
4. Busca la línea: `Listen 80`
5. Cámbiala a: `Listen 8080`
6. Guarda y reinicia Apache.

---

## 6. ¿Qué es Postman y para qué sirve?

### El problema sin Postman

Cuando desarrollas una API, no puedes simplemente abrir el navegador y hacer un POST o un DELETE. El navegador solo puede hacer peticiones **GET** por la barra de direcciones.

Para probar todos los métodos (POST, PUT, DELETE...) necesitas una herramienta especial.

### Postman al rescate

**Postman** es una aplicación (gratuita) que te permite **hacer cualquier tipo de petición HTTP de forma fácil y visual.**

Es la herramienta estándar de la industria para:
- Probar APIs que estás desarrollando.
- Explorar APIs de terceros.
- Documentar tu API.
- Hacer pruebas automáticas.

### Interfaz de Postman Partes principales

```

 [GET ] [http://localhost:8080/api/usuarios ] [Send] Barra principal

 Params | Authorization | Headers | Body Pestañas


 (Aquí escribes el body si es POST/PUT/PATCH) 


 RESPONSE (Respuesta del servidor) Resultado
 Status: 200 OK Time: 43ms Size: 1.2 KB 
 { "usuarios": [...] } 

```

### Cómo hacer una petición GET en Postman

1. Abre Postman.
2. En el desplegable de la izquierda, selecciona **GET**.
3. En la barra de URL escribe: `http://localhost:8080/api/usuarios`
4. Haz clic en **Send**.
5. Abajo verás la respuesta del servidor.

### Cómo hacer una petición POST en Postman

1. Selecciona **POST** en el desplegable.
2. Escribe la URL: `http://localhost:8080/api/usuarios`
3. Ve a la pestaña **Body**.
4. Selecciona **raw** y luego **JSON**.
5. Escribe el JSON que quieres enviar:
```json
{
 "nombre": "María López",
 "email": "maria@gmail.com",
 "edad": 25
}
```
6. Haz clic en **Send**.

### Cómo hacer una petición PUT en Postman

1. Selecciona **PUT**.
2. URL: `http://localhost:8080/api/usuarios/3` (el /3 es el ID del usuario a actualizar)
3. Body raw JSON:
```json
{
 "nombre": "María López García",
 "email": "maria.nueva@gmail.com",
 "edad": 26
}
```
4. Send.

### Cómo hacer una petición DELETE en Postman

1. Selecciona **DELETE**.
2. URL: `http://localhost:8080/api/usuarios/3`
3. No necesitas body.
4. Send.

### Collections en Postman

Una **Collection** es como una carpeta donde guardas todas tus peticiones organizadas.

Para crear una:
1. Clic en **New** **Collection**.
2. Dale un nombre (ej: "API de mi proyecto").
3. Agrega peticiones a la colección haciendo clic en los 3 puntos **Add request**.

Esto es muy útil para no tener que escribir las URLs cada vez.

### Variables de entorno en Postman

En vez de escribir `http://localhost:8080` en cada petición, puedes crear una variable:

1. Clic en el ícono de engranaje (arriba a la derecha).
2. **Add** Nuevo entorno Nómbralo "Local".
3. Agrega una variable: `base_url` = `http://localhost:8080`.
4. En tus URLs usa: `{{base_url}}/api/usuarios`.

Ahora si cambias el puerto, solo lo cambias en un lugar.

---

## 7. JSON el idioma de las APIs

### ¿Qué es JSON?

**JSON** son las siglas de **JavaScript Object Notation** (Notación de Objetos de JavaScript).

Es básicamente **el formato en que las APIs envían y reciben datos.**

Piensa en JSON como un formulario estructurado. En vez de texto libre, los datos tienen una forma específica y ordenada.

### Sintaxis básica de JSON

```json
{
 "clave": "valor"
}
```

Las reglas son:
- Las **claves** siempre van entre comillas dobles `"`.
- Los **valores** pueden ser de varios tipos.
- Se separan con comas `,`.
- El objeto empieza con `{` y termina con `}`.

### Tipos de valores en JSON

```json
{
 "nombre": "Carlos", String (texto): entre comillas
 "edad": 25, Number (número): sin comillas
 "activo": true, Boolean: true o false (sin comillas)
 "telefono": null, Null: valor vacío
 "hobbies": ["futbol", "programar", "leer"], Array (lista)
 "direccion": { Objeto anidado
 "calle": "Av. Principal 123",
 "ciudad": "Madrid"
 }
}
```

### Ejemplo completo de respuesta JSON de una API

```json
{
 "status": "success",
 "codigo": 200,
 "datos": {
 "total": 3,
 "usuarios": [
 {
 "id": 1,
 "nombre": "Ana García",
 "email": "ana@gmail.com",
 "activo": true
 },
 {
 "id": 2,
 "nombre": "Luis Martínez",
 "email": "luis@hotmail.com",
 "activo": true
 },
 {
 "id": 3,
 "nombre": "Sofía Rodríguez",
 "email": "sofia@yahoo.com",
 "activo": false
 }
 ]
 }
}
```

### Errores comunes con JSON

 **Mal** coma al final del último elemento:
```json
{
 "nombre": "Carlos",
 "edad": 25, Esta coma está de más, hay nada después
}
```

 **Bien:**
```json
{
 "nombre": "Carlos",
 "edad": 25
}
```

 **Mal** claves sin comillas:
```json
{
 nombre: "Carlos" Las claves SIEMPRE van entre comillas en JSON
}
```

 **Bien:**
```json
{
 "nombre": "Carlos"
}
```

### ¿Cómo validar tu JSON?

Si tienes dudas de si tu JSON está bien escrito, usa [jsonlint.com](https://jsonlint.com) pega tu JSON y te dice si tiene errores.

---

## 8. Códigos de estado HTTP

Cuando haces una petición a una API, el servidor siempre responde con un **código de estado**. Este código te dice si todo salió bien o qué pasó.

Son números de 3 dígitos agrupados por el primer número:

### Grupo 2xx Éxito 

| Código | Nombre | ¿Cuándo aparece? |
|--------|--------|------------------|
| **200** | OK | Todo salió bien. Petición exitosa. |
| **201** | Created | Se creó algo nuevo exitosamente (POST exitoso). |
| **204** | No Content | Éxito, pero no hay nada que devolver (DELETE exitoso). |

### Grupo 3xx Redirección 

| Código | Nombre | ¿Cuándo aparece? |
|--------|--------|------------------|
| **301** | Moved Permanently | La URL cambió para siempre. |
| **302** | Found | Redirección temporal. |
| **304** | Not Modified | El recurso no cambió (usa la caché). |

### Grupo 4xx Error del cliente (tú hiciste algo mal)

| Código | Nombre | ¿Cuándo aparece? |
|--------|--------|------------------|
| **400** | Bad Request | Mandaste datos incorrectos o mal formateados. |
| **401** | Unauthorized | No estás autenticado (no has iniciado sesión). |
| **403** | Forbidden | Estás autenticado pero no tienes permiso. |
| **404** | Not Found | La URL o el recurso no existe. |
| **405** | Method Not Allowed | Usaste el método equivocado (ej: GET en vez de POST). |
| **409** | Conflict | Hay un conflicto (ej: quieres crear un usuario que ya existe). |
| **422** | Unprocessable Entity | Los datos son válidos pero no tienen sentido lógico. |
| **429** | Too Many Requests | Hiciste demasiadas peticiones. Espera un poco. |

### Grupo 5xx Error del servidor (el servidor tuvo un problema)

| Código | Nombre | ¿Cuándo aparece? |
|--------|--------|------------------|
| **500** | Internal Server Error | Error genérico del servidor. Algo se rompió en el código. |
| **502** | Bad Gateway | El servidor recibió una respuesta inválida de otro servidor. |
| **503** | Service Unavailable | El servidor está caído o saturado. |
| **504** | Gateway Timeout | El servidor tardó demasiado en responder. |

### Regla fácil para recordarlos

- **2xx** = Todo bien 
- **3xx** = Ve a otro lado 
- **4xx** = Tú te equivocaste 
- **5xx** = Yo (el servidor) me equivoqué 

---

## 9. Estructura de una URL de API

Una URL de API sigue una estructura lógica y predecible. Vamos a diseccionarla parte por parte.

```
http://localhost:8080/api/v1/usuarios/42/posts?limit=10&page=2
|___| |___________| |_| |_| |______| |_| |__| |__________|
 1 2 3 4 5 6 7 8
```

| Número | Parte | Nombre | Ejemplo | Explicación |
|--------|-------|--------|---------|-------------|
| 1 | `http://` | Protocolo | `http://` | Cómo se comunican cliente y servidor |
| 2 | `localhost:8080` | Host + Puerto | `localhost:8080` | Dónde está el servidor |
| 3 | `/api` | Base path | `/api` | Indica que es una API (convención) |
| 4 | `/v1` | Versión | `/v1` | Versión de la API |
| 5 | `/usuarios` | Recurso | `/usuarios` | Con qué "colección" trabajas |
| 6 | `/42` | ID | `/42` | Un elemento específico de esa colección |
| 7 | `/posts` | Sub-recurso | `/posts` | Recurso relacionado con ese elemento |
| 8 | `?limit=10&page=2` | Query params | `?limit=10&page=2` | Filtros y opciones adicionales |

### Ejemplos de URLs bien diseñadas

```
GET /api/productos Obtener todos los productos
GET /api/productos/5 Obtener el producto con ID 5
POST /api/productos Crear un producto nuevo
PUT /api/productos/5 Actualizar completamente el producto 5
PATCH /api/productos/5 Actualizar parcialmente el producto 5
DELETE /api/productos/5 Eliminar el producto 5

GET /api/usuarios/3/pedidos Todos los pedidos del usuario 3
GET /api/usuarios/3/pedidos/7 El pedido 7 del usuario 3
```

### Buenas prácticas en el diseño de URLs

| Bien | Mal | ¿Por qué? |
|---------|--------|-----------|
| `/api/usuarios` | `/api/getUsuarios` | La URL no debe incluir el verbo, el método HTTP ya lo dice |
| `/api/productos` | `/api/Producto` | Usa minúsculas y plural |
| `/api/categorias-productos` | `/api/categoriasProductos` | Usa guiones, no camelCase en URLs |
| `/api/v1/usuarios` | `/api/usuarios_v1` | La versión va al principio del path |

---

## 10. Headers y Body

### Headers (Cabeceras)

Los **headers** son información extra que viaja junto con la petición o respuesta, pero que no es el contenido principal.

Son como los metadatos de una carta: el remitente, el destinatario, el tipo de carta, el idioma...

#### Headers de petición más comunes

| Header | ¿Para qué sirve? | Ejemplo |
|--------|-----------------|---------|
| `Content-Type` | Le dice al servidor en qué formato van los datos que mandas | `application/json` |
| `Accept` | Le dice al servidor en qué formato quieres recibir la respuesta | `application/json` |
| `Authorization` | Para enviar tokens de autenticación | `Bearer eyJhbGciOiJIUzI1...` |
| `User-Agent` | Identifica qué cliente hace la petición | `Mozilla/5.0...` |

#### Headers de respuesta más comunes

| Header | ¿Para qué sirve? | Ejemplo |
|--------|-----------------|---------|
| `Content-Type` | Le dice al cliente en qué formato están los datos | `application/json` |
| `Content-Length` | El tamaño de la respuesta en bytes | `1234` |
| `Cache-Control` | Instrucciones de caché | `no-cache` |

#### Cómo agregar headers en Postman

1. Ve a la pestaña **Headers**.
2. Agrega una fila con la **Key** y el **Value**:
 - Key: `Content-Type`
 - Value: `application/json`

#### Cómo agregar headers en código PHP

```php
header('Content-Type: application/json');
header('Access-Control-Allow-Origin: *');
```

Estas líneas le dicen al servidor que la respuesta va a ser JSON y que cualquier origen puede consumir esta API.

---

### Body (Cuerpo)

El **body** es el contenido principal de la petición los datos que realmente quieres enviar.

Solo los métodos **POST**, **PUT** y **PATCH** tienen body. GET y DELETE normalmente no.

#### Tipos de body en Postman

| Tipo | ¿Cuándo usarlo? |
|------|-----------------|
| **none** | Cuando no envías datos (GET, DELETE) |
| **form-data** | Para enviar formularios con archivos adjuntos |
| **x-www-form-urlencoded** | Para formularios HTML normales |
| **raw JSON** | Lo más común en APIs modernas |
| **binary** | Para enviar archivos directamente |

---

## 11. Parámetros en las peticiones

Hay 3 formas de pasar parámetros a una API:

### 1. Path Parameters (parámetros en la URL)

Van directamente en la URL, son parte de la ruta.

```
GET /api/usuarios/42

 ID del usuario (path parameter)
```

Se usan para identificar un recurso específico.

### 2. Query Parameters (parámetros de consulta)

Van al final de la URL después del signo `?`.

```
GET /api/productos?categoria=electronica&precio_max=500&pagina=2
 |___________________| |____________| |_____|
 parámetro 1 parámetro 2 param 3
```

Se usan para filtrar, ordenar o paginar resultados.

| Símbolo | ¿Qué hace? |
|---------|------------|
| `?` | Inicia los query parameters |
| `=` | Asigna valor a la clave |
| `&` | Separa varios parámetros |

#### En PHP cómo los recibes:
```php
$categoria = $_GET['categoria']; // "electronica"
$precio_max = $_GET['precio_max']; // "500"
$pagina = $_GET['pagina']; // "2"
```

### 3. Body Parameters

Van en el cuerpo de la petición (solo en POST, PUT, PATCH).

```json
POST /api/usuarios
Body:
{
 "nombre": "Pablo Torres",
 "email": "pablo@gmail.com"
}
```

#### En PHP cómo los recibes:
```php
$datos = json_decode(file_get_contents('php://input'), true);
$nombre = $datos['nombre']; // "Pablo Torres"
$email = $datos['email']; // "pablo@gmail.com"
```

---

## 12. Ejemplo completo paso a paso

Vamos a construir una **API REST básica en PHP** que gestione una lista de productos. Todo desde cero.

### Estructura de archivos

```
htdocs/
 api-productos/
 config/
 conexion.php
 api/
 productos.php
 index.php
```

### Paso 1 Crear la base de datos

Abre **phpMyAdmin** (http://localhost/phpmyadmin) y ejecuta esto:

```sql
-- Crear la base de datos
CREATE DATABASE tienda;

-- Usar esa base de datos
USE tienda;

-- Crear la tabla de productos
CREATE TABLE productos (
 id INT AUTO_INCREMENT PRIMARY KEY,
 nombre VARCHAR(100) NOT NULL,
 precio DECIMAL(10,2) NOT NULL,
 stock INT DEFAULT 0,
 categoria VARCHAR(50),
 creado_en TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Insertar datos de prueba
INSERT INTO productos (nombre, precio, stock, categoria) VALUES
('Laptop ASUS', 799.99, 15, 'electronica'),
('Mouse Logitech', 25.50, 100, 'electronica'),
('Cuaderno A4', 3.99, 500, 'papeleria'),
('Auriculares Sony', 89.99, 40, 'electronica');
```

---

### Paso 2 Archivo de conexión

**config/conexion.php**

```php
<?php

// Datos de conexión a la base de datos
$host = "localhost"; // Dónde está MySQL (en XAMPP es siempre localhost)
$dbname = "tienda"; // Nombre de la base de datos que creamos
$username = "root"; // Usuario de MySQL (en XAMPP por defecto es root)
$password = ""; // Contraseña (en XAMPP por defecto está vacía)

// Intentar conectar
try {
 // PDO = PHP Data Objects, es la forma moderna de conectar a MySQL en PHP
 $pdo = new PDO("mysql:host=$host;dbname=$dbname;charset=utf8", $username, $password);

 // Si hay un error, que lo muestre (modo de desarrollo)
 $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);

} catch(PDOException $e) {
 // Si no se pudo conectar, devolver error en formato JSON
 http_response_code(500);
 echo json_encode([
 "status" => "error",
 "mensaje" => "No se pudo conectar a la base de datos: " . $e->getMessage()
 ]);
 exit(); // Detener la ejecución
}
```

**Significado de cada línea:**
- `new PDO(...)` Crear una nueva conexión a la base de datos.
- `"mysql:host=$host;dbname=$dbname"` Le dice a PDO que usamos MySQL, en qué servidor y qué base de datos.
- `setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION)` Le dice que si hay un error, que lance una excepción (así podemos capturarla con `try/catch`).
- `http_response_code(500)` Envía el código de estado 500 al cliente.
- `json_encode(...)` Convierte un array de PHP a formato JSON.

---

### Paso 3 El archivo principal de la API

**api/productos.php**

```php
<?php

// Permitir que cualquier origen pueda consumir esta API (CORS)
header('Access-Control-Allow-Origin: *');
// Le decimos que vamos a responder con JSON
header('Content-Type: application/json');
// Permitir los métodos que nuestra API va a usar
header('Access-Control-Allow-Methods: GET, POST, PUT, DELETE');
// Permitir el header de Content-Type en las peticiones
header('Access-Control-Allow-Headers: Content-Type');

// Incluir el archivo de conexión (lo que está en config/conexion.php)
require_once '../config/conexion.php';

// Obtener el método HTTP que se usó (GET, POST, PUT, DELETE)
$method = $_SERVER['REQUEST_METHOD'];

// Obtener el ID si viene en la URL (?id=5)
$id = isset($_GET['id']) ? (int)$_GET['id'] : null;

// Según el método, ejecutar la acción correspondiente
switch($method) {
 case 'GET':
 obtenerProductos($pdo, $id);
 break;
 case 'POST':
 crearProducto($pdo);
 break;
 case 'PUT':
 actualizarProducto($pdo, $id);
 break;
 case 'DELETE':
 eliminarProducto($pdo, $id);
 break;
 default:
 // Si llega un método que no conocemos
 http_response_code(405); // Method Not Allowed
 echo json_encode(["error" => "Método no permitido"]);
}

// ==========================================
// FUNCIÓN: Obtener productos
// ==========================================
function obtenerProductos($pdo, $id) {

 if ($id) {
 // Si se pidió un ID específico, buscar solo ese producto
 $stmt = $pdo->prepare("SELECT * FROM productos WHERE id = ?");
 $stmt->execute([$id]);
 $producto = $stmt->fetch(PDO::FETCH_ASSOC);

 if ($producto) {
 // Producto encontrado
 http_response_code(200);
 echo json_encode([
 "status" => "success",
 "datos" => $producto
 ]);
 } else {
 // No existe ese ID
 http_response_code(404);
 echo json_encode([
 "status" => "error",
 "mensaje" => "Producto no encontrado"
 ]);
 }

 } else {
 // Sin ID = devolver todos los productos
 $stmt = $pdo->query("SELECT * FROM productos ORDER BY id ASC");
 $productos = $stmt->fetchAll(PDO::FETCH_ASSOC);

 http_response_code(200);
 echo json_encode([
 "status" => "success",
 "total" => count($productos),
 "datos" => $productos
 ]);
 }
}

// ==========================================
// FUNCIÓN: Crear un producto
// ==========================================
function crearProducto($pdo) {

 // Leer el body de la petición (viene en formato JSON)
 $body = file_get_contents('php://input');
 // Convertir el JSON a un array de PHP
 $datos = json_decode($body, true);

 // Validar que se mandaron los campos obligatorios
 if (!isset($datos['nombre']) || !isset($datos['precio'])) {
 http_response_code(400); // Bad Request
 echo json_encode([
 "status" => "error",
 "mensaje" => "Faltan campos obligatorios: nombre y precio"
 ]);
 return;
 }

 // Preparar la consulta SQL de inserción
 $stmt = $pdo->prepare(
 "INSERT INTO productos (nombre, precio, stock, categoria) VALUES (?, ?, ?, ?)"
 );

 // Ejecutar con los valores del body
 $stmt->execute([
 $datos['nombre'],
 $datos['precio'],
 $datos['stock'] ?? 0, // Si no mandaron stock, poner 0
 $datos['categoria'] ?? null // Si no mandaron categoría, poner null
 ]);

 // Obtener el ID del registro recién creado
 $nuevoId = $pdo->lastInsertId();

 http_response_code(201); // Created
 echo json_encode([
 "status" => "success",
 "mensaje" => "Producto creado exitosamente",
 "id" => $nuevoId
 ]);
}

// ==========================================
// FUNCIÓN: Actualizar un producto
// ==========================================
function actualizarProducto($pdo, $id) {

 if (!$id) {
 http_response_code(400);
 echo json_encode(["error" => "Se requiere el ID del producto"]);
 return;
 }

 // Verificar que el producto existe
 $check = $pdo->prepare("SELECT id FROM productos WHERE id = ?");
 $check->execute([$id]);

 if (!$check->fetch()) {
 http_response_code(404);
 echo json_encode(["error" => "Producto no encontrado"]);
 return;
 }

 // Leer y decodificar el body
 $datos = json_decode(file_get_contents('php://input'), true);

 // Actualizar el producto
 $stmt = $pdo->prepare(
 "UPDATE productos SET nombre = ?, precio = ?, stock = ?, categoria = ? WHERE id = ?"
 );
 $stmt->execute([
 $datos['nombre'],
 $datos['precio'],
 $datos['stock'],
 $datos['categoria'],
 $id
 ]);

 http_response_code(200);
 echo json_encode([
 "status" => "success",
 "mensaje" => "Producto actualizado exitosamente"
 ]);
}

// ==========================================
// FUNCIÓN: Eliminar un producto
// ==========================================
function eliminarProducto($pdo, $id) {

 if (!$id) {
 http_response_code(400);
 echo json_encode(["error" => "Se requiere el ID del producto"]);
 return;
 }

 // Verificar que existe antes de borrar
 $check = $pdo->prepare("SELECT id FROM productos WHERE id = ?");
 $check->execute([$id]);

 if (!$check->fetch()) {
 http_response_code(404);
 echo json_encode(["error" => "Producto no encontrado"]);
 return;
 }

 // Eliminar el producto
 $stmt = $pdo->prepare("DELETE FROM productos WHERE id = ?");
 $stmt->execute([$id]);

 http_response_code(200);
 echo json_encode([
 "status" => "success",
 "mensaje" => "Producto eliminado exitosamente"
 ]);
}
```

---

### Paso 4 Probar todo en Postman

Con XAMPP corriendo, prueba estas peticiones:

**Obtener todos los productos:**
```
GET http://localhost/api-productos/api/productos.php
```

**Obtener el producto con ID 1:**
```
GET http://localhost/api-productos/api/productos.php?id=1
```

**Crear un producto nuevo:**
```
POST http://localhost/api-productos/api/productos.php
Headers: Content-Type: application/json
Body (raw JSON):
{
 "nombre": "Teclado Mecánico",
 "precio": 59.99,
 "stock": 30,
 "categoria": "electronica"
}
```

**Actualizar el producto con ID 1:**
```
PUT http://localhost/api-productos/api/productos.php?id=1
Headers: Content-Type: application/json
Body (raw JSON):
{
 "nombre": "Teclado Mecánico RGB",
 "precio": 69.99,
 "stock": 25,
 "categoria": "electronica"
}
```

**Eliminar el producto con ID 4:**
```
DELETE http://localhost/api-productos/api/productos.php?id=4
```

---

## 13. Comandos útiles

### Comandos para la terminal / cmd relacionados con servidores

```bash
# Ver qué puertos están en uso
netstat -ano

# Ver si el puerto 8080 está ocupado (Windows)
netstat -ano | findstr :8080

# Ver el proceso que usa ese puerto
tasklist | findstr [PID_del_proceso]

# Hacer una petición GET desde cmd (Windows con curl)
curl http://localhost:8080/api/productos

# Hacer una petición POST desde cmd con curl
curl -X POST http://localhost:8080/api/productos \
 -H "Content-Type: application/json" \
 -d "{\"nombre\":\"Producto Test\",\"precio\":10.99}"

# Hacer ping a localhost (verificar que responde)
ping localhost
```

### Comandos de MySQL útiles (en phpMyAdmin o terminal)

```sql
-- Ver todas las bases de datos
SHOW DATABASES;

-- Usar una base de datos
USE nombre_db;

-- Ver todas las tablas
SHOW TABLES;

-- Ver la estructura de una tabla
DESCRIBE productos;
-- o también:
DESC productos;

-- Ver todos los registros
SELECT * FROM productos;

-- Buscar registros con condición
SELECT * FROM productos WHERE categoria = 'electronica';

-- Insertar
INSERT INTO productos (nombre, precio) VALUES ('Nuevo', 9.99);

-- Actualizar
UPDATE productos SET precio = 15.99 WHERE id = 1;

-- Eliminar
DELETE FROM productos WHERE id = 1;

-- Contar registros
SELECT COUNT(*) FROM productos;

-- Ordenar resultados
SELECT * FROM productos ORDER BY precio ASC; -- Ascendente (menor a mayor)
SELECT * FROM productos ORDER BY precio DESC; -- Descendente (mayor a menor)

-- Limitar resultados (útil para paginación)
SELECT * FROM productos LIMIT 10; -- Solo 10 resultados
SELECT * FROM productos LIMIT 10 OFFSET 20; -- 10 resultados empezando desde el 21
```

### Comandos de PHP en terminal

```bash
# Ver la versión de PHP
php -v

# Ejecutar un archivo PHP
php archivo.php

# Iniciar el servidor de desarrollo de PHP (sin XAMPP)
php -S localhost:8080

# Iniciar en una carpeta específica
php -S localhost:8080 -t /ruta/al/proyecto
```

---

## 14. Errores comunes y cómo resolverlos

### Error: "No se puede acceder a este sitio" en el navegador

**Causas posibles:**
- Apache no está corriendo Abre XAMPP y haz clic en Start en Apache.
- El puerto está mal Verifica que es `localhost` no `localhost:80` si configuraste el 8080.
- El nombre de la carpeta está mal Revisa que el proyecto está en `htdocs`.

---

### Error: 404 Not Found en Postman

**Causas posibles:**
- La URL está mal escrita.
- El archivo PHP no existe en esa ruta.
- La carpeta del proyecto no está en htdocs.

**Solución:** Verifica la URL paso a paso. Copia la URL al navegador para confirmar que el archivo existe.

---

### Error: 500 Internal Server Error

**Causas posibles:**
- Error de sintaxis en tu PHP.
- La base de datos no está corriendo.
- Error en la consulta SQL.

**Cómo encontrar el error:**
1. Abre el archivo `C:\xampp\apache\logs\error.log`.
2. Busca las últimas líneas, ahí estará el error específico.

---

### Error: "Access-Control-Allow-Origin"

Este error aparece en el navegador cuando una página web intenta usar tu API pero el navegador la bloquea por seguridad (CORS).

**Solución:** Agrega estos headers al inicio de tu PHP:
```php
header('Access-Control-Allow-Origin: *');
header('Access-Control-Allow-Methods: GET, POST, PUT, DELETE, OPTIONS');
header('Access-Control-Allow-Headers: Content-Type, Authorization');
```

---

### Error: JSON vacío o null en PHP

Si `json_decode(file_get_contents('php://input'), true)` te devuelve `null`:

**Causas posibles:**
- No mandaste el header `Content-Type: application/json` en Postman.
- El JSON está mal formateado.

**Solución:** En Postman, asegúrate de:
1. Ir a Body raw.
2. Cambiar el desplegable de "Text" a **JSON**.
3. Esto automáticamente agrega el header correcto.

---

### Puerto 8080 ocupado (en Windows)

```bash
# Encontrar qué proceso usa el 8080
netstat -ano | findstr :8080

# El número al final es el PID (Process ID)
# Terminar ese proceso:
taskkill /PID [número_del_pid] /F
```

---

## 15. Glosario de palabras importantes

| Término | Significado simple |
|---------|-------------------|
| **API** | Interfaz para que dos aplicaciones se comuniquen. Como el mesero entre tú y la cocina. |
| **REST** | Estilo de diseño de APIs usando HTTP. El más usado actualmente. |
| **HTTP** | Protocolo de comunicación entre navegadores y servidores. |
| **HTTPS** | HTTP con cifrado de seguridad. |
| **Endpoint** | Una URL específica de tu API. Cada endpoint hace una cosa (ej: `/api/usuarios`). |
| **Request** | La petición que hace el cliente al servidor. |
| **Response** | La respuesta que devuelve el servidor. |
| **Client** | Quien hace la petición (navegador, app, Postman...). |
| **Server** | Quien recibe y responde las peticiones (XAMPP, Node.js...). |
| **localhost** | Tu propia computadora vista como servidor. |
| **Puerto** | Número de "puerta" por donde entra la comunicación en un servidor. |
| **XAMPP** | Software que instala Apache + MySQL + PHP en tu máquina. |
| **Apache** | El servidor web. El que escucha y responde las peticiones web. |
| **MySQL** | Sistema de bases de datos relacionales. |
| **phpMyAdmin** | Interfaz gráfica para administrar MySQL desde el navegador. |
| **htdocs** | Carpeta raíz de Apache en XAMPP. Lo que pones aquí es accesible desde el navegador. |
| **JSON** | Formato de texto para intercambiar datos. Muy fácil de leer. |
| **Postman** | Herramienta para probar APIs. Permite hacer cualquier tipo de petición HTTP. |
| **GET** | Método HTTP para obtener datos. |
| **POST** | Método HTTP para crear datos nuevos. |
| **PUT** | Método HTTP para actualizar datos completamente. |
| **PATCH** | Método HTTP para actualizar solo una parte de los datos. |
| **DELETE** | Método HTTP para eliminar datos. |
| **CRUD** | Create, Read, Update, Delete. Las 4 operaciones básicas de datos. |
| **Status Code** | Código numérico que indica el resultado de una petición (200, 404, 500...). |
| **Header** | Información extra que viaja con una petición o respuesta. |
| **Body** | El contenido principal de una petición (los datos que envías). |
| **Query Parameter** | Parámetro que va en la URL después del `?`. Ej: `?id=5&pagina=2`. |
| **Path Parameter** | Parámetro que forma parte de la URL. Ej: `/usuarios/42`. |
| **PDO** | PHP Data Objects. Forma moderna de conectar PHP con bases de datos. |
| **SQL** | Lenguaje para hacer consultas a bases de datos. |
| **CORS** | Cross-Origin Resource Sharing. Mecanismo de seguridad que controla quién puede consumir una API. |
| **Token** | Cadena de texto que identifica a un usuario autenticado. Como una llave de acceso temporal. |
| **JWT** | JSON Web Token. Tipo de token muy usado para autenticación en APIs. |
| **Autenticación** | Verificar quién eres. (Iniciar sesión.) |
| **Autorización** | Verificar qué puedes hacer. (Si tienes permiso para hacer algo.) |
| **Endpoint** | URL específica de una API que realiza una función concreta. |
| **Recurso** | El "objeto" con el que trabaja tu API (usuario, producto, pedido...). |
| **Colección** | En Postman: carpeta donde guardas peticiones relacionadas. |
| **Variable de entorno** | En Postman: valor reutilizable (como la URL base) que puedes cambiar en un solo lugar. |
| **PHP** | Lenguaje de programación para el lado del servidor. XAMPP lo incluye. |
| **PDO prepare** | Forma segura de hacer consultas SQL evitando ataques de inyección SQL. |
| **json_encode** | Función de PHP que convierte un array a texto JSON. |
| **json_decode** | Función de PHP que convierte texto JSON a un array de PHP. |
| **file_get_contents** | Función de PHP para leer el contenido de un archivo o del body de la petición. |
| **php://input** | Stream especial de PHP para leer el body crudo de una petición HTTP. |

---

## Recursos adicionales

- [JSONLint](https://jsonlint.com) Validador de JSON online.
- [HTTP Status Codes](https://httpstatuses.com) Referencia completa de todos los códigos HTTP.
- [Postman Learning Center](https://learning.postman.com) Documentación oficial de Postman en inglés.
- [PHP Manual](https://www.php.net/manual/es/) Documentación oficial de PHP en español.
- [phpMyAdmin](http://localhost/phpmyadmin) Acceso a tu base de datos local (con XAMPP corriendo).

---

*Repositorio creado para los cursos de Programación de Servicios Web y Desarrollo de Aplicaciones Web.*
