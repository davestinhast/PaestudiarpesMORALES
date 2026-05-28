# XAMPP Guía Completa

## ¿Qué instala XAMPP exactamente?

Cuando instalas XAMPP, estás instalando un servidor web completo en tu computadora. Todo lo que un servidor real en internet tiene, tú lo tienes en local.

```
XAMPP instala:
 Apache El servidor web (escucha peticiones en el puerto 80 u 8080)
 MySQL La base de datos (guarda y recupera datos)
 PHP El lenguaje de servidor (procesa la lógica)
 Perl Otro lenguaje (casi no se usa)
 phpMyAdmin Interfaz web para administrar MySQL
```

---

## Panel de control de XAMPP

Cuando abres el panel de control, ves algo así:

```

 XAMPP Control Panel 

 Module PID(s) Port(s) Actions 

 Apache 1234 80, 443 [Stop][Admin][Config] Verde = corriendo
 MySQL 5678 3306 [Stop][Admin][Config] 
 FileZilla [Start] Rojo = parado
 Mercury [Start] 

```

### Botones

| Botón | ¿Qué hace? |
|-------|------------|
| **Start** | Inicia el servicio |
| **Stop** | Detiene el servicio |
| **Admin** | Abre la página de administración (Apache abre el navegador; MySQL abre phpMyAdmin) |
| **Config** | Abre el archivo de configuración |
| **Logs** | Muestra los registros de errores |

---

## La carpeta htdocs

`htdocs` es la carpeta más importante de XAMPP para ti.

**¿Dónde está?**
```
Windows: C:\xampp\htdocs\
Mac: /Applications/XAMPP/htdocs/
Linux: /opt/lampp/htdocs/
```

**¿Qué es?** 
Es la **raíz del servidor web**. Apache sirve los archivos que están aquí.

**Analogía:** Piensa en `htdocs` como el mostrador de una tienda. Lo que pones en ese mostrador, los clientes pueden verlo y pedirlo.

### Cómo funciona htdocs

Si creas este archivo:
```
C:\xampp\htdocs\hola.php
```

Puedes verlo en:
```
http://localhost/hola.php
```

Si creas una carpeta y un archivo:
```
C:\xampp\htdocs\mi-proyecto\api\usuarios.php
```

Puedes accederlo en:
```
http://localhost/mi-proyecto/api/usuarios.php
```

**La regla:** `htdocs` = `http://localhost/`

---

## Configurar Apache en el puerto 8080

Por defecto Apache usa el puerto 80. Pero en Windows a veces el puerto 80 está ocupado por Skype, IIS, u otros programas. Solución: usar el 8080.

### Pasos para cambiar al puerto 8080

1. Abre el Panel de XAMPP.
2. En la fila de Apache, click en **Config**.
3. Click en **httpd.conf** se abre el Bloc de notas.
4. Busca (Ctrl+F): `Listen 80`
5. Cámbialo a: `Listen 8080`
6. Busca: `ServerName localhost:80`
7. Cámbialo a: `ServerName localhost:8080`
8. Guarda el archivo.
9. En el panel de XAMPP, click en **Stop** en Apache y luego en **Start** de nuevo.

Ahora tu servidor corre en `http://localhost:8080`.

---

## phpMyAdmin

**phpMyAdmin** es una herramienta web para administrar tu base de datos MySQL de forma visual, sin tener que escribir comandos.

**Cómo acceder:**
1. Asegúrate de que Apache y MySQL están corriendo en XAMPP.
2. Abre el navegador.
3. Ve a: `http://localhost/phpmyadmin`

### Lo que puedes hacer en phpMyAdmin

| Acción | ¿Para qué? |
|--------|------------|
| Crear base de datos | Nueva base de datos para tu proyecto |
| Crear tabla | Estructura donde guardarás datos |
| Insertar filas | Agregar datos manualmente |
| Ver datos | Ver todos los registros de una tabla |
| Ejecutar SQL | Escribir consultas directamente |
| Exportar | Guardar tu base de datos como archivo .sql |
| Importar | Cargar una base de datos desde un archivo .sql |

### Crear una base de datos en phpMyAdmin

1. Click en **Nueva** (en el panel izquierdo).
2. Nombre de la base de datos: `mi_proyecto` (sin espacios, en minúsculas).
3. Cotejamiento: `utf8_general_ci` (soporta caracteres en español).
4. Click en **Crear**.

### Crear una tabla en phpMyAdmin

1. Selecciona tu base de datos en el panel izquierdo.
2. Nombre de la tabla: `usuarios`.
3. Número de columnas: 4.
4. Click en **Continue**.
5. Define las columnas:

| Nombre | Tipo | Longitud | Extra |
|--------|------|----------|-------|
| id | INT | | AUTO_INCREMENT |
| nombre | VARCHAR | 100 | |
| email | VARCHAR | 150 | |
| creado_en | TIMESTAMP | | DEFAULT CURRENT_TIMESTAMP |

6. Marca `id` como **PRIMARY KEY**.
7. Click en **Save**.

---

## Archivos de log (registro de errores)

Cuando algo sale mal, Apache escribe el error en sus logs.

**Dónde encontrarlos:**
```
C:\xampp\apache\logs\error.log Errores de Apache y PHP
C:\xampp\apache\logs\access.log Registro de cada petición
```

**Cómo verlos:**
- Abre el Panel de XAMPP.
- Click en **Logs** en la fila de Apache.
- Click en **Apache (error.log)**.

**Leer un error típico:**
```
[Thu Jan 01 12:00:00 2025] [error] [pid 1234] PHP Parse error: 
syntax error, unexpected token "}", in 
C:\xampp\htdocs\mi-api\index.php on line 45
```

Traduciendo: *"Error de PHP: sintaxis incorrecta, llave `}` inesperada, en el archivo index.php en la línea 45."*

---

## Estructura recomendada de proyecto en htdocs

```
htdocs/
 nombre-proyecto/
 index.php Punto de entrada
 config/
 database.php Configuración de BD
 models/ Clases que representan los datos
 Usuario.php
 Producto.php
 controllers/ Lógica de negocio
 UsuarioController.php
 ProductoController.php
 api/ Archivos que responden a las peticiones
 usuarios.php
 productos.php
 helpers/ Funciones de ayuda
 response.php
```

---

## Solución a problemas comunes de XAMPP

### Apache no inicia (puerto ocupado)

El error en los logs sería: `Address already in use: make_sock: could not bind to address 0.0.0.0:80`

**Solución:**
1. Cambiar al puerto 8080 (ver sección arriba).
2. O encontrar y matar el proceso que usa el puerto 80:
 - Abrir cmd como administrador.
 - `netstat -ano | findstr :80`
 - Apuntar el PID.
 - `taskkill /PID [numero] /F`

### MySQL no inicia

**Causas posibles:**
- Otro MySQL ya está corriendo (si tienes MySQL instalado por separado).
- Problema con los archivos de datos.

**Solución rápida:**
1. Abrir el Panel de XAMPP.
2. Click en **Config** de MySQL.
3. Click en **my.ini**.
4. Busca `port=3306` y cámbialo a `port=3307`.
5. Reinicia MySQL.

### No puedo conectar a MySQL desde PHP

Verifica estos datos en tu archivo de conexión:
```php
$host = "localhost"; // Siempre localhost en XAMPP
$port = 3306; // Puerto de MySQL (3306 por defecto)
$usuario = "root"; // Usuario por defecto en XAMPP
$password = ""; // Contraseña vacía por defecto en XAMPP
```
