# Bootstrap — Guia Completa

## Que es Bootstrap

Bootstrap es una **libreria de CSS y JavaScript** creada por Twitter. Te da clases ya escritas que puedes usar directamente en tu HTML para que las cosas se vean bien sin tener que escribir CSS desde cero.

Piensalo asi: en vez de escribir tu mismo el estilo de un boton, Bootstrap ya tiene ese boton hecho. Tu solo pones la clase en tu HTML y listo.

Sin Bootstrap:
```css
/* Tendrias que escribir todo esto tu */
.mi-boton {
    background-color: #0d6efd;
    color: white;
    padding: 8px 16px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 16px;
}
.mi-boton:hover {
    background-color: #0b5ed7;
}
```

Con Bootstrap:
```html
<!-- Solo pones la clase y ya tiene estilo -->
<button class="btn btn-primary">Click aqui</button>
```

El resultado visual es el mismo. Bootstrap ya escribio ese CSS por ti.

---

## Como incluir Bootstrap en tu proyecto

Hay dos formas:

### Opcion 1 — CDN (la mas facil, no descargas nada)

Pega esto en el `<head>` de tu HTML:

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mi pagina</title>

    <!-- Bootstrap CSS — va en el head -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>

    <!-- Tu contenido aqui -->

    <!-- Bootstrap JavaScript — va justo antes de cerrar el body -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

Necesitas internet para que funcione (el CSS se carga desde los servidores de Bootstrap).

### Opcion 2 — Descarga local

1. Ve a [getbootstrap.com](https://getbootstrap.com)
2. Click en Download
3. Descarga los archivos compilados
4. Copia `bootstrap.min.css` a tu carpeta `css/`
5. Copia `bootstrap.bundle.min.js` a tu carpeta `js/`
6. Enlaza los archivos localmente:

```html
<link href="css/bootstrap.min.css" rel="stylesheet">
<script src="js/bootstrap.bundle.min.js"></script>
```

Ventaja: funciona sin internet.

---

## El sistema de Grid (cuadricula)

El grid es lo mas importante de Bootstrap. Te permite dividir la pagina en columnas de forma facil.

### Como funciona

Bootstrap divide cada fila en **12 columnas**. Tu decides cuantas columnas ocupa cada elemento.

```
|  1  |  2  |  3  |  4  |  5  |  6  |  7  |  8  |  9  |  10 |  11 |  12 |
```

Ejemplos de distribucion:
```
[    6 columnas    ] [    6 columnas    ]   = dos mitades iguales
[   4 cols  ] [  4 cols  ] [  4 cols  ]    = tres tercios
[     8 columnas      ] [  4 cols  ]       = dos tercios y un tercio
[          12 columnas          ]          = ancho completo
```

### Estructura basica del grid

Siempre es: **container > row > col**

```html
<div class="container">          <!-- Contenedor principal con margenes -->
    <div class="row">            <!-- Fila -->
        <div class="col-6">      <!-- Columna de 6/12 = 50% del ancho -->
            Columna izquierda
        </div>
        <div class="col-6">      <!-- Columna de 6/12 = 50% del ancho -->
            Columna derecha
        </div>
    </div>
</div>
```

### Mas ejemplos de grid

```html
<!-- Tres columnas iguales -->
<div class="container">
    <div class="row">
        <div class="col-4">Columna 1</div>
        <div class="col-4">Columna 2</div>
        <div class="col-4">Columna 3</div>
    </div>
</div>

<!-- Una grande y una pequena -->
<div class="container">
    <div class="row">
        <div class="col-8">Contenido principal (ancho)</div>
        <div class="col-4">Barra lateral (angosta)</div>
    </div>
</div>

<!-- Columnas automaticas (Bootstrap las distribuye solas) -->
<div class="container">
    <div class="row">
        <div class="col">Se distribuyen</div>
        <div class="col">automaticamente</div>
        <div class="col">en partes iguales</div>
    </div>
</div>
```

### Grid responsivo (que se adapte al movil)

Bootstrap tiene prefijos para distintos tamanios de pantalla:

| Prefijo | Pantalla | Ancho minimo |
|---------|----------|-------------|
| `col-` | Todos los tamanios | 0px |
| `col-sm-` | Pequeno (telefono horizontal) | 576px |
| `col-md-` | Mediano (tablet) | 768px |
| `col-lg-` | Grande (laptop) | 992px |
| `col-xl-` | Extra grande (monitor) | 1200px |

```html
<!-- En pantalla grande: 3 columnas. En movil: cada una ocupa todo el ancho -->
<div class="container">
    <div class="row">
        <div class="col-12 col-md-4">Bloque 1</div>
        <div class="col-12 col-md-4">Bloque 2</div>
        <div class="col-12 col-md-4">Bloque 3</div>
    </div>
</div>
```

---

## Botones

```html
<!-- Tipos de boton -->
<button class="btn btn-primary">Azul (principal)</button>
<button class="btn btn-secondary">Gris (secundario)</button>
<button class="btn btn-success">Verde (exito)</button>
<button class="btn btn-danger">Rojo (peligro)</button>
<button class="btn btn-warning">Amarillo (advertencia)</button>
<button class="btn btn-info">Celeste (informacion)</button>
<button class="btn btn-light">Claro</button>
<button class="btn btn-dark">Oscuro</button>

<!-- Botones de contorno (sin relleno) -->
<button class="btn btn-outline-primary">Contorno azul</button>
<button class="btn btn-outline-danger">Contorno rojo</button>

<!-- Tamanios -->
<button class="btn btn-primary btn-lg">Grande</button>
<button class="btn btn-primary">Normal</button>
<button class="btn btn-primary btn-sm">Pequeno</button>

<!-- Boton ancho completo -->
<button class="btn btn-primary w-100">Ocupa todo el ancho</button>
```

---

## Colores de texto y fondo

```html
<!-- Colores de texto -->
<p class="text-primary">Texto azul</p>
<p class="text-success">Texto verde</p>
<p class="text-danger">Texto rojo</p>
<p class="text-warning">Texto amarillo</p>
<p class="text-muted">Texto grisaceo (secundario)</p>
<p class="text-white bg-dark">Texto blanco</p>

<!-- Colores de fondo -->
<div class="bg-primary text-white">Fondo azul</div>
<div class="bg-success text-white">Fondo verde</div>
<div class="bg-danger text-white">Fondo rojo</div>
<div class="bg-warning">Fondo amarillo</div>
<div class="bg-light">Fondo claro</div>
<div class="bg-dark text-white">Fondo oscuro</div>
```

---

## Tipografia

```html
<!-- Alineacion de texto -->
<p class="text-start">Alineado a la izquierda</p>
<p class="text-center">Centrado</p>
<p class="text-end">Alineado a la derecha</p>

<!-- Grosor -->
<p class="fw-bold">Negrita</p>
<p class="fw-normal">Normal</p>
<p class="fw-light">Delgado</p>

<!-- Estilo -->
<p class="fst-italic">Cursiva</p>

<!-- Tamanio -->
<p class="fs-1">Tamanio 1 (el mas grande)</p>
<p class="fs-2">Tamanio 2</p>
<p class="fs-3">Tamanio 3</p>
<p class="fs-4">Tamanio 4</p>
<p class="fs-5">Tamanio 5</p>
<p class="fs-6">Tamanio 6 (el mas pequeno)</p>

<!-- Titulos con estilo de heading sin ser un heading -->
<p class="h1">Parece un H1 pero es un parrafo</p>
```

---

## Espaciado (margin y padding)

Bootstrap tiene un sistema de clases para controlar el espacio.

### Formato de la clase

```
{propiedad}{lado}-{tamanio}

Propiedad:  m = margin    p = padding
Lado:       t = top       b = bottom
            s = start(izq) e = end(der)
            x = izq y der   y = arriba y abajo
            (sin lado = los 4 lados)
Tamanio:    0, 1, 2, 3, 4, 5, auto
```

### Ejemplos

```html
<!-- Margin -->
<div class="m-3">Margin en los 4 lados, tamanio 3</div>
<div class="mt-4">Margin solo arriba, tamanio 4</div>
<div class="mb-2">Margin solo abajo, tamanio 2</div>
<div class="mx-auto">Margin horizontal automatico (centra el elemento)</div>
<div class="my-5">Margin arriba y abajo, tamanio 5</div>
<div class="m-0">Sin margin</div>

<!-- Padding -->
<div class="p-3">Padding en los 4 lados</div>
<div class="pt-2">Padding solo arriba</div>
<div class="px-4">Padding izquierda y derecha</div>
<div class="py-3">Padding arriba y abajo</div>
```

Los tamanios van del 0 al 5:
- 0 = 0px
- 1 = 0.25rem (~4px)
- 2 = 0.5rem (~8px)
- 3 = 1rem (~16px)
- 4 = 1.5rem (~24px)
- 5 = 3rem (~48px)

---

## Alertas

```html
<div class="alert alert-primary">Informacion general</div>
<div class="alert alert-success">Operacion exitosa</div>
<div class="alert alert-danger">Ha ocurrido un error</div>
<div class="alert alert-warning">Advertencia importante</div>
<div class="alert alert-info">Nota informativa</div>

<!-- Alerta con boton para cerrar -->
<div class="alert alert-danger alert-dismissible fade show">
    Error al guardar los datos.
    <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
</div>
```

---

## Cards (tarjetas)

Las cards son cuadros con contenido. Muy usados para mostrar productos, perfiles, articulos.

```html
<div class="card" style="width: 300px;">
    <img src="imagen.jpg" class="card-img-top" alt="Imagen">
    <div class="card-body">
        <h5 class="card-title">Titulo de la card</h5>
        <p class="card-text">Descripcion del contenido de la card.</p>
        <a href="#" class="btn btn-primary">Ver mas</a>
    </div>
</div>
```

Cards en grid (3 columnas responsivas):

```html
<div class="container">
    <div class="row row-cols-1 row-cols-md-3 g-4">

        <div class="col">
            <div class="card h-100">
                <div class="card-body">
                    <h5 class="card-title">Producto 1</h5>
                    <p class="card-text">Descripcion del producto.</p>
                    <p class="card-text fw-bold">$29.99</p>
                </div>
                <div class="card-footer">
                    <button class="btn btn-success w-100">Comprar</button>
                </div>
            </div>
        </div>

        <div class="col">
            <div class="card h-100">
                <div class="card-body">
                    <h5 class="card-title">Producto 2</h5>
                    <p class="card-text">Descripcion del producto.</p>
                    <p class="card-text fw-bold">$49.99</p>
                </div>
                <div class="card-footer">
                    <button class="btn btn-success w-100">Comprar</button>
                </div>
            </div>
        </div>

    </div>
</div>
```

---

## Formularios

```html
<form>
    <!-- Campo de texto -->
    <div class="mb-3">
        <label for="nombre" class="form-label">Nombre</label>
        <input type="text" class="form-control" id="nombre" placeholder="Tu nombre">
    </div>

    <!-- Email -->
    <div class="mb-3">
        <label for="email" class="form-label">Email</label>
        <input type="email" class="form-control" id="email" placeholder="tu@email.com">
    </div>

    <!-- Textarea -->
    <div class="mb-3">
        <label for="mensaje" class="form-label">Mensaje</label>
        <textarea class="form-control" id="mensaje" rows="3"></textarea>
    </div>

    <!-- Select -->
    <div class="mb-3">
        <label for="categoria" class="form-label">Categoria</label>
        <select class="form-select" id="categoria">
            <option value="">Selecciona una opcion</option>
            <option value="1">Opcion 1</option>
            <option value="2">Opcion 2</option>
        </select>
    </div>

    <!-- Checkbox -->
    <div class="mb-3 form-check">
        <input type="checkbox" class="form-check-input" id="terminos">
        <label class="form-check-label" for="terminos">Acepto los terminos</label>
    </div>

    <!-- Boton de envio -->
    <button type="submit" class="btn btn-primary">Enviar</button>
</form>
```

---

## Navbar (barra de navegacion)

```html
<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
    <div class="container">

        <!-- Logo / nombre del sitio -->
        <a class="navbar-brand" href="#">Mi Sitio</a>

        <!-- Boton hamburguesa para movil -->
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
            <span class="navbar-toggler-icon"></span>
        </button>

        <!-- Links de navegacion -->
        <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav ms-auto">
                <li class="nav-item">
                    <a class="nav-link active" href="#">Inicio</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="#">Productos</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="#">Contacto</a>
                </li>
            </ul>
        </div>

    </div>
</nav>
```

---

## Tablas

```html
<!-- Tabla basica con estilo Bootstrap -->
<table class="table">
    <thead>
        <tr>
            <th>ID</th>
            <th>Nombre</th>
            <th>Email</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1</td>
            <td>Carlos Ramirez</td>
            <td>carlos@gmail.com</td>
        </tr>
        <tr>
            <td>2</td>
            <td>Sofia Herandez</td>
            <td>sofia@gmail.com</td>
        </tr>
    </tbody>
</table>

<!-- Variantes -->
<table class="table table-striped">...</table>    <!-- Filas alternadas con color -->
<table class="table table-hover">...</table>      <!-- Resalta la fila al pasar el mouse -->
<table class="table table-bordered">...</table>   <!-- Con bordes en todas las celdas -->
<table class="table table-dark">...</table>       <!-- Fondo oscuro -->

<!-- Tabla responsiva (scroll horizontal en pantallas pequenas) -->
<div class="table-responsive">
    <table class="table">...</table>
</div>
```

---

## Modal (ventana emergente)

```html
<!-- Boton que abre el modal -->
<button class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#miModal">
    Abrir ventana
</button>

<!-- El modal en si (puede ir en cualquier parte del body) -->
<div class="modal fade" id="miModal">
    <div class="modal-dialog">
        <div class="modal-content">

            <div class="modal-header">
                <h5 class="modal-title">Titulo del modal</h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
            </div>

            <div class="modal-body">
                Contenido del modal. Puedes poner lo que quieras aqui.
            </div>

            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cerrar</button>
                <button type="button" class="btn btn-primary">Guardar</button>
            </div>

        </div>
    </div>
</div>
```

---

## Clases de utilidad rapidas

```html
<!-- Ancho y alto -->
<div class="w-25">25% de ancho</div>
<div class="w-50">50% de ancho</div>
<div class="w-75">75% de ancho</div>
<div class="w-100">100% de ancho</div>

<!-- Display -->
<div class="d-none">Oculto</div>
<div class="d-block">Display block</div>
<div class="d-flex">Display flex</div>
<div class="d-inline">Display inline</div>

<!-- Flexbox -->
<div class="d-flex justify-content-center">Centrado horizontal</div>
<div class="d-flex justify-content-between">Separado en extremos</div>
<div class="d-flex align-items-center">Centrado vertical</div>
<div class="d-flex gap-3">Con espacio entre elementos</div>

<!-- Bordes -->
<div class="border">Con borde</div>
<div class="border border-primary">Borde azul</div>
<div class="rounded">Bordes redondeados</div>
<div class="rounded-circle">Circular (ideal para fotos de perfil)</div>

<!-- Sombra -->
<div class="shadow-sm">Sombra pequena</div>
<div class="shadow">Sombra normal</div>
<div class="shadow-lg">Sombra grande</div>

<!-- Posicion -->
<div class="position-relative">Relativo</div>
<div class="position-absolute">Absoluto</div>
<div class="fixed-top">Fijo arriba (como un navbar pegado)</div>

<!-- Overflow -->
<div class="overflow-auto">Scroll si el contenido desborda</div>
<div class="overflow-hidden">Ocultar lo que desborda</div>
```

---

## Ejemplo completo — Pagina con API y Bootstrap

Este es un ejemplo de como mostrar datos de tu API en una pagina con Bootstrap:

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lista de Usuarios</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>

    <!-- Navbar -->
    <nav class="navbar navbar-dark bg-dark mb-4">
        <div class="container">
            <a class="navbar-brand" href="#">Mi App</a>
        </div>
    </nav>

    <!-- Contenido principal -->
    <div class="container">

        <div class="d-flex justify-content-between align-items-center mb-3">
            <h2>Lista de Usuarios</h2>
            <button class="btn btn-success" onclick="cargarUsuarios()">Recargar</button>
        </div>

        <!-- Tabla donde aparecen los datos -->
        <div class="table-responsive">
            <table class="table table-striped table-hover">
                <thead class="table-dark">
                    <tr>
                        <th>ID</th>
                        <th>Nombre</th>
                        <th>Email</th>
                        <th>Estado</th>
                        <th>Acciones</th>
                    </tr>
                </thead>
                <tbody id="tablaUsuarios">
                    <!-- Los datos se insertan aqui con JavaScript -->
                </tbody>
            </table>
        </div>

        <!-- Alerta de error (oculta por defecto) -->
        <div class="alert alert-danger d-none" id="alertaError">
            No se pudieron cargar los usuarios.
        </div>

    </div>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
    <script>
        // Funcion para cargar usuarios desde la API
        async function cargarUsuarios() {
            try {
                const respuesta = await fetch('http://localhost/api-usuarios/api/usuarios.php');
                const datos = await respuesta.json();

                const tbody = document.getElementById('tablaUsuarios');
                tbody.innerHTML = ''; // Limpiar tabla

                datos.datos.forEach(usuario => {
                    tbody.innerHTML += `
                        <tr>
                            <td>${usuario.id}</td>
                            <td>${usuario.nombre} ${usuario.apellido}</td>
                            <td>${usuario.email}</td>
                            <td>
                                <span class="badge ${usuario.activo ? 'bg-success' : 'bg-secondary'}">
                                    ${usuario.activo ? 'Activo' : 'Inactivo'}
                                </span>
                            </td>
                            <td>
                                <button class="btn btn-sm btn-warning me-1" onclick="editarUsuario(${usuario.id})">Editar</button>
                                <button class="btn btn-sm btn-danger" onclick="eliminarUsuario(${usuario.id})">Eliminar</button>
                            </td>
                        </tr>
                    `;
                });

            } catch (error) {
                document.getElementById('alertaError').classList.remove('d-none');
            }
        }

        // Cargar al iniciar la pagina
        cargarUsuarios();
    </script>

</body>
</html>
```

---

## Resumen de clases mas usadas

| Categoria | Clase | Que hace |
|-----------|-------|----------|
| Grid | `container` | Contenedor con margenes |
| Grid | `row` | Fila del grid |
| Grid | `col-{n}` | Columna de n/12 del ancho |
| Boton | `btn btn-primary` | Boton azul |
| Boton | `btn btn-danger` | Boton rojo |
| Boton | `btn-sm / btn-lg` | Tamanio pequeno / grande |
| Texto | `text-center` | Centrar texto |
| Texto | `fw-bold` | Negrita |
| Texto | `text-danger` | Texto rojo |
| Fondo | `bg-dark` | Fondo oscuro |
| Fondo | `bg-success` | Fondo verde |
| Espaciado | `m-3 / p-3` | Margin / padding tamanio 3 |
| Espaciado | `mt-2 / mb-2` | Margin top / bottom |
| Espaciado | `mx-auto` | Centrar horizontalmente |
| Flex | `d-flex` | Activar flexbox |
| Flex | `justify-content-center` | Centrar horizontal |
| Flex | `align-items-center` | Centrar vertical |
| Ancho | `w-100` | Ancho completo |
| Borde | `rounded` | Bordes redondeados |
| Tabla | `table table-striped` | Tabla con filas alternas |
| Alerta | `alert alert-success` | Caja verde de exito |
| Card | `card / card-body` | Tarjeta con contenido |
