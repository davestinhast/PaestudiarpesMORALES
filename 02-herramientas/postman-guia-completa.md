# Postman Guía Completa

## ¿Por qué necesitamos Postman?

El navegador web (Chrome, Firefox, etc.) solo puede hacer peticiones **GET** fácilmente (cuando escribes una URL en la barra de direcciones, eso es un GET).

Para hacer POST, PUT, DELETE y controlar headers y body... el navegador no lo hace solo. Por eso usamos **Postman**.

---

## Instalar Postman

1. Ir a [postman.com/downloads](https://www.postman.com/downloads/)
2. Descargar la versión para tu sistema operativo.
3. Instalar y abrir.
4. Puedes crear una cuenta gratuita o saltar ese paso.

---

## La interfaz explicada completamente

```

 [+] New [Import] [ Settings] Barra superior

 [GET ] [https://tu-url.com/api/ruta ] [Send] Barra de petición
 Left 
 Bar [Params][Auth][Headers][Body][Pre-req.][Tests] Pestañas

(Cole- (Contenido de la pestaña seleccionada) Área de configuración
tion) 

 RESPONSE 200 OK 43ms 1KB Respuesta
 [Body][Cookies][Headers][Test Results] 

 { "status": "success", "datos": [...] } 

```

### Las pestañas de configuración

| Pestaña | ¿Para qué sirve? |
|---------|-----------------|
| **Params** | Agregar query parameters (los que van en la URL con `?`) |
| **Authorization** | Configurar autenticación (API Keys, Bearer Token, etc.) |
| **Headers** | Agregar cabeceras a la petición |
| **Body** | Escribir el cuerpo de la petición (para POST, PUT, PATCH) |
| **Pre-request Script** | Código JavaScript que corre antes de la petición |
| **Tests** | Código JavaScript para verificar la respuesta automáticamente |

---

## Hacer cada tipo de petición

### GET Obtener datos

```
Método: GET
URL: http://localhost:8080/api/productos
Headers: (ninguno necesario)
Body: (ninguno)
```

**Para filtrar con query params:**
1. Ve a la pestaña **Params**.
2. Agrega una fila:
 - Key: `categoria`
 - Value: `electronica`
3. Postman construye la URL automáticamente: `?categoria=electronica`.

O también puedes escribir directamente en la URL: `http://localhost:8080/api/productos?categoria=electronica`

---

### POST Crear datos nuevos

```
Método: POST
URL: http://localhost:8080/api/productos
Headers: Content-Type: application/json IMPORTANTE
Body: raw JSON
 {
 "nombre": "Laptop HP",
 "precio": 649.99,
 "stock": 10
 }
```

**Pasos en Postman:**
1. Selecciona **POST** en el menú desplegable.
2. Escribe la URL.
3. Ve a **Headers**, agrega:
 - Key: `Content-Type`
 - Value: `application/json`
4. Ve a **Body** **raw** cambia "Text" a **JSON**.
5. Escribe tu JSON.
6. Click en **Send**.

---

### PUT Actualizar completamente

```
Método: PUT
URL: http://localhost:8080/api/productos/5
Headers: Content-Type: application/json
Body: raw JSON
 {
 "nombre": "Laptop HP Actualizada",
 "precio": 699.99,
 "stock": 8,
 "categoria": "electronica"
 }
```

*El /5 al final de la URL es el ID del producto que quieres actualizar.*

---

### PATCH Actualizar parcialmente

```
Método: PATCH
URL: http://localhost:8080/api/productos/5
Headers: Content-Type: application/json
Body: raw JSON
 {
 "precio": 599.99
 }
```

Solo mandas los campos que quieres cambiar. El resto queda igual.

---

### DELETE Eliminar

```
Método: DELETE
URL: http://localhost:8080/api/productos/5
Headers: (ninguno necesario)
Body: (ninguno)
```

---

## Collections (Colecciones)

Una colección es como una **carpeta** donde guardas todas tus peticiones organizadas.

### Crear una colección

1. Clic en **New** **Collection**.
2. Dale un nombre: "API Mi Proyecto".
3. Opcional: agrega una descripción.
4. Clic en **Create**.

### Guardar una petición en la colección

1. Haz tu petición.
2. Clic en el disco () al lado del nombre de la pestaña, o **Save** arriba a la derecha.
3. Dale un nombre descriptivo: "GET - Obtener todos los productos".
4. Selecciona tu colección.
5. Save.

### Beneficios de las colecciones

- No tienes que escribir las URLs cada vez.
- Puedes compartir la colección con tu equipo (exportar/importar).
- Puedes ejecutar todas las peticiones en secuencia (Collection Runner).

---

## Environments (Entornos)

Los entornos te permiten tener **variables** que puedes cambiar fácilmente.

**Sin entorno:**
```
http://localhost:8080/api/usuarios
http://localhost:8080/api/productos
http://localhost:8080/api/pedidos
```
Si cambias el puerto, tienes que editar TODAS las URLs.

**Con entorno:**
```
{{base_url}}/api/usuarios
{{base_url}}/api/productos
{{base_url}}/api/pedidos
```
Solo cambias `base_url` en un lugar.

### Crear un entorno

1. Clic en el ícono de engranaje () arriba a la derecha.
2. **Add** Nuevo entorno.
3. Nombre: "Desarrollo Local".
4. Variable:
 - Variable: `base_url`
 - Initial Value: `http://localhost:8080`
 - Current Value: `http://localhost:8080`
5. **Save**.
6. Selecciona el entorno en el desplegable de la esquina superior derecha.

### Variables de entorno comunes

| Variable | Valor |
|----------|-------|
| `base_url` | `http://localhost:8080` |
| `token` | `tu_token_de_autenticacion` |
| `user_id` | `42` |

---

## Ver la respuesta

Después de hacer click en **Send**, la respuesta aparece abajo:

### Partes de la respuesta

| Parte | ¿Qué muestra? |
|-------|---------------|
| **Status** | El código de estado (200 OK, 404 Not Found, etc.) |
| **Time** | Cuánto tardó la petición en milisegundos |
| **Size** | Tamaño de la respuesta |
| **Body** | El contenido de la respuesta (normalmente JSON) |
| **Cookies** | Las cookies que devuelve el servidor |
| **Headers** | Los headers de la respuesta |

### Modos de visualización del Body

| Modo | ¿Para qué? |
|------|------------|
| **Pretty** | Muestra el JSON formateado y con colores. El más útil. |
| **Raw** | Muestra el texto exacto sin formatear. |
| **Preview** | Renderiza HTML si la respuesta es una página web. |

---

## Errores comunes en Postman

### "Could not get any response"
- Apache no está corriendo en XAMPP.
- La URL está mal escrita.
- El puerto es incorrecto.

### 404 Not Found
- La ruta en la URL no existe.
- El archivo PHP no está en esa ubicación.

### 500 Internal Server Error
- Hay un error en tu código PHP.
- Revisa `C:\xampp\apache\logs\error.log`.

### Body vacío en POST pero el servidor no lo recibe
- Falta el header `Content-Type: application/json`.
- El body no está en formato raw JSON.

---

## Atajos de teclado útiles en Postman

| Atajo | Acción |
|-------|--------|
| `Ctrl + Enter` | Enviar petición |
| `Ctrl + S` | Guardar petición |
| `Ctrl + N` | Nueva pestaña/petición |
| `Ctrl + T` | Nueva pestaña |
| `Ctrl + W` | Cerrar pestaña |
