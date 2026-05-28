# Tailwind CSS — Guia Completa

## Que es Tailwind CSS

Tailwind es una libreria de CSS igual que Bootstrap, pero con una filosofia diferente.

Bootstrap te da **componentes ya hechos** (un boton, una navbar, una card — todo prediseñado).

Tailwind te da **clases pequeñas** que hacen una sola cosa cada una, y tu las combinas para construir lo que quieras.

### Diferencia con Bootstrap

Bootstrap — usas un componente entero:
```html
<button class="btn btn-primary">Click</button>
```

Tailwind — construyes el boton pieza por pieza:
```html
<button class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-700">Click</button>
```

Que significan esas clases de Tailwind:
- `bg-blue-500` = fondo azul, intensidad 500
- `text-white` = texto blanco
- `px-4` = padding izquierda y derecha, tamanio 4
- `py-2` = padding arriba y abajo, tamanio 2
- `rounded` = bordes redondeados
- `hover:bg-blue-700` = al pasar el mouse, fondo azul mas oscuro

### Cual es mejor, Bootstrap o Tailwind?

No hay uno mejor. Depende del proyecto:

| Situacion | Recomendado |
|-----------|-------------|
| Quieres algo rapido con buen diseño predefinido | Bootstrap |
| Quieres diseño completamente personalizado | Tailwind |
| Es un proyecto academico/rapido | Bootstrap |
| Es un proyecto profesional moderno | Tailwind |
| Eres principiante | Bootstrap (mas facil al inicio) |

---

## Como incluir Tailwind en tu proyecto

### Opcion 1 — CDN (para practicar, sin configuracion)

Pega esto en el `<head>`:

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mi pagina con Tailwind</title>

    <!-- Tailwind CSS via CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body>
    <!-- Tu contenido aqui -->
</body>
</html>
```

Con esto ya puedes usar todas las clases de Tailwind.

### Opcion 2 — Instalacion con npm (para proyectos reales)

```bash
# En tu proyecto
npm install -D tailwindcss
npx tailwindcss init

# Configurar y compilar el CSS
npx tailwindcss -i ./src/input.css -o ./dist/output.css --watch
```

Para el curso, el CDN es suficiente.

---

## Sistema de colores

Tailwind tiene una paleta de colores con intensidades del 50 al 950.

```
{propiedad}-{color}-{intensidad}
```

Los colores principales:
`slate`, `gray`, `zinc`, `neutral`, `stone`, `red`, `orange`, `amber`, `yellow`, `lime`, `green`, `emerald`, `teal`, `cyan`, `sky`, `blue`, `indigo`, `violet`, `purple`, `fuchsia`, `pink`, `rose`

Las intensidades: `50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950`

Cuanto mayor el numero, mas oscuro. Cuanto menor, mas claro.

```html
<!-- Fondos -->
<div class="bg-blue-500">Azul mediano</div>
<div class="bg-blue-200">Azul claro</div>
<div class="bg-blue-800">Azul oscuro</div>
<div class="bg-red-500">Rojo</div>
<div class="bg-green-500">Verde</div>
<div class="bg-gray-100">Gris muy claro (casi blanco)</div>
<div class="bg-gray-900">Gris muy oscuro (casi negro)</div>
<div class="bg-white">Blanco</div>
<div class="bg-black">Negro</div>

<!-- Texto -->
<p class="text-blue-600">Texto azul</p>
<p class="text-red-500">Texto rojo</p>
<p class="text-gray-500">Texto gris</p>
<p class="text-white">Texto blanco</p>

<!-- Bordes -->
<div class="border border-blue-500">Borde azul</div>
<div class="border border-red-300">Borde rojo claro</div>
```

---

## Espaciado (padding y margin)

Tailwind usa una escala numerica. Cada unidad = 0.25rem = 4px.

```
p-{numero}  = padding en los 4 lados
px-{numero} = padding izquierda y derecha
py-{numero} = padding arriba y abajo
pt-{numero} = padding top (arriba)
pb-{numero} = padding bottom (abajo)
pl-{numero} = padding left (izquierda)
pr-{numero} = padding right (derecha)

(Lo mismo con m- en vez de p- para margin)
```

```html
<!-- Padding -->
<div class="p-4">Padding 4 en todos lados (16px)</div>
<div class="px-6 py-3">Padding horizontal 24px, vertical 12px</div>
<div class="pt-2 pb-8">Top 8px, bottom 32px</div>
<div class="p-0">Sin padding</div>

<!-- Margin -->
<div class="m-4">Margin en todos lados</div>
<div class="mx-auto">Centrar horizontalmente</div>
<div class="mt-8 mb-4">Margin top 32px, bottom 16px</div>
<div class="m-0">Sin margin</div>
```

Referencia de valores comunes:

| Clase | Valor en px |
|-------|-------------|
| `p-0` | 0px |
| `p-1` | 4px |
| `p-2` | 8px |
| `p-3` | 12px |
| `p-4` | 16px |
| `p-5` | 20px |
| `p-6` | 24px |
| `p-8` | 32px |
| `p-10` | 40px |
| `p-12` | 48px |

---

## Tipografia

```html
<!-- Tamanio de fuente -->
<p class="text-xs">Extra pequeno (12px)</p>
<p class="text-sm">Pequeno (14px)</p>
<p class="text-base">Normal (16px)</p>
<p class="text-lg">Grande (18px)</p>
<p class="text-xl">Extra grande (20px)</p>
<p class="text-2xl">2x grande (24px)</p>
<p class="text-3xl">3x grande (30px)</p>
<p class="text-4xl">4x grande (36px)</p>
<p class="text-5xl">5x grande (48px)</p>

<!-- Grosor -->
<p class="font-thin">Muy delgado</p>
<p class="font-normal">Normal</p>
<p class="font-medium">Mediano</p>
<p class="font-semibold">Semi negrita</p>
<p class="font-bold">Negrita</p>
<p class="font-extrabold">Extra negrita</p>

<!-- Alineacion -->
<p class="text-left">Izquierda</p>
<p class="text-center">Centro</p>
<p class="text-right">Derecha</p>

<!-- Transformacion -->
<p class="uppercase">MAYUSCULAS</p>
<p class="lowercase">minusculas</p>
<p class="capitalize">Primera Letra Mayuscula</p>

<!-- Decoracion -->
<p class="underline">Subrayado</p>
<p class="line-through">Tachado</p>
<p class="no-underline">Sin subrayado</p>

<!-- Altura de linea -->
<p class="leading-tight">Lineas juntas</p>
<p class="leading-normal">Normal</p>
<p class="leading-relaxed">Lineas separadas</p>
<p class="leading-loose">Muy separadas</p>
```

---

## Ancho y alto

```html
<!-- Ancho fijo -->
<div class="w-4">16px</div>
<div class="w-8">32px</div>
<div class="w-16">64px</div>
<div class="w-32">128px</div>
<div class="w-64">256px</div>

<!-- Ancho relativo -->
<div class="w-1/2">50% del padre</div>
<div class="w-1/3">33% del padre</div>
<div class="w-2/3">66% del padre</div>
<div class="w-1/4">25% del padre</div>
<div class="w-3/4">75% del padre</div>
<div class="w-full">100% del padre</div>

<!-- Ancho de pantalla -->
<div class="w-screen">100% del ancho de la pantalla</div>

<!-- Alto -->
<div class="h-16">64px de alto</div>
<div class="h-full">100% del alto del padre</div>
<div class="h-screen">100% del alto de la pantalla</div>
<div class="min-h-screen">Minimo 100% del alto de la pantalla</div>
```

---

## Flexbox

Tailwind hace el flexbox muy facil.

```html
<!-- Activar flex -->
<div class="flex">
    <div>Elemento 1</div>
    <div>Elemento 2</div>
    <div>Elemento 3</div>
</div>

<!-- Direccion -->
<div class="flex flex-row">Horizontal (por defecto)</div>
<div class="flex flex-col">Vertical</div>

<!-- Alineacion horizontal (justify) -->
<div class="flex justify-start">Al inicio</div>
<div class="flex justify-center">Centrado</div>
<div class="flex justify-end">Al final</div>
<div class="flex justify-between">Espacio entre elementos</div>
<div class="flex justify-around">Espacio alrededor</div>
<div class="flex justify-evenly">Espacio igual entre todos</div>

<!-- Alineacion vertical (align) -->
<div class="flex items-start">Arriba</div>
<div class="flex items-center">Centro vertical</div>
<div class="flex items-end">Abajo</div>

<!-- Espacio entre elementos -->
<div class="flex gap-4">Espacio de 16px entre elementos</div>
<div class="flex gap-x-4">Solo espacio horizontal</div>
<div class="flex gap-y-4">Solo espacio vertical</div>

<!-- Envolver elementos (wrap) -->
<div class="flex flex-wrap">Los elementos saltan a la siguiente linea si no caben</div>

<!-- Caso comun: centrar algo perfectamente -->
<div class="flex justify-center items-center h-screen">
    <div>Esto esta centrado en la pantalla</div>
</div>
```

---

## Grid

```html
<!-- Grid de 3 columnas -->
<div class="grid grid-cols-3 gap-4">
    <div>Col 1</div>
    <div>Col 2</div>
    <div>Col 3</div>
</div>

<!-- Grid de 2 columnas -->
<div class="grid grid-cols-2 gap-6">
    <div>Izquierda</div>
    <div>Derecha</div>
</div>

<!-- Grid responsivo (1 col en movil, 3 en pantalla grande) -->
<div class="grid grid-cols-1 md:grid-cols-3 gap-4">
    <div>Bloque 1</div>
    <div>Bloque 2</div>
    <div>Bloque 3</div>
</div>
```

---

## Responsivo (diseño adaptable)

Tailwind usa prefijos para aplicar clases segun el tamanio de la pantalla:

| Prefijo | Ancho minimo | Equivalente |
|---------|-------------|-------------|
| (sin prefijo) | 0px | Movil primero |
| `sm:` | 640px | Telefono horizontal |
| `md:` | 768px | Tablet |
| `lg:` | 1024px | Laptop |
| `xl:` | 1280px | Monitor |
| `2xl:` | 1536px | Monitor grande |

```html
<!-- Texto pequeno en movil, grande en pantalla grande -->
<h1 class="text-xl md:text-3xl lg:text-5xl">Titulo adaptable</h1>

<!-- Columnas responsivas -->
<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
    <div>1</div>
    <div>2</div>
    <div>3</div>
    <div>4</div>
</div>

<!-- Ocultar en movil, mostrar en pantalla grande -->
<div class="hidden lg:block">Solo visible en pantallas grandes</div>

<!-- Mostrar en movil, ocultar en pantalla grande -->
<div class="block lg:hidden">Solo visible en movil</div>
```

---

## Hover, Focus y otros estados

```html
<!-- Hover (al pasar el mouse) -->
<button class="bg-blue-500 hover:bg-blue-700 text-white px-4 py-2 rounded">
    Boton que cambia al hover
</button>

<!-- Focus (cuando el campo esta seleccionado) -->
<input class="border border-gray-300 focus:border-blue-500 focus:outline-none px-3 py-2 rounded">

<!-- Active (mientras se hace click) -->
<button class="bg-blue-500 active:bg-blue-900 text-white px-4 py-2 rounded">
    Click sostenido
</button>

<!-- Disabled (deshabilitado) -->
<button class="bg-blue-500 disabled:opacity-50 text-white px-4 py-2 rounded" disabled>
    Deshabilitado
</button>
```

---

## Bordes y sombras

```html
<!-- Bordes -->
<div class="border">Borde de 1px en todos lados</div>
<div class="border-2">Borde de 2px</div>
<div class="border-4">Borde de 4px</div>
<div class="border-t">Solo borde arriba</div>
<div class="border-b">Solo borde abajo</div>
<div class="border-l">Solo borde izquierda</div>

<!-- Colores de borde -->
<div class="border border-gray-300">Borde gris</div>
<div class="border border-red-500">Borde rojo</div>
<div class="border border-blue-500">Borde azul</div>
<div class="border-0">Sin borde</div>

<!-- Redondear bordes -->
<div class="rounded-sm">Muy poco redondeado</div>
<div class="rounded">Redondeado normal</div>
<div class="rounded-lg">Muy redondeado</div>
<div class="rounded-xl">Extra redondeado</div>
<div class="rounded-full">Circular (para avatares)</div>
<div class="rounded-none">Sin redondeo</div>

<!-- Sombras -->
<div class="shadow-sm">Sombra pequena</div>
<div class="shadow">Sombra normal</div>
<div class="shadow-md">Sombra mediana</div>
<div class="shadow-lg">Sombra grande</div>
<div class="shadow-xl">Sombra extra grande</div>
<div class="shadow-none">Sin sombra</div>
```

---

## Display y visibilidad

```html
<div class="block">Display block</div>
<div class="inline">Display inline</div>
<div class="inline-block">Display inline-block</div>
<div class="flex">Display flex</div>
<div class="grid">Display grid</div>
<div class="hidden">Oculto (display none)</div>
<div class="invisible">Invisible pero ocupa espacio</div>
```

---

## Ejemplo completo — Pagina con Tailwind y API

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Usuarios - Tailwind</title>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 min-h-screen">

    <!-- Navbar -->
    <nav class="bg-gray-900 text-white px-6 py-4 shadow-lg">
        <div class="max-w-6xl mx-auto flex justify-between items-center">
            <span class="text-xl font-bold">Mi App</span>
            <button class="bg-blue-500 hover:bg-blue-600 px-4 py-2 rounded text-sm" onclick="cargarUsuarios()">
                Recargar
            </button>
        </div>
    </nav>

    <!-- Contenido -->
    <div class="max-w-6xl mx-auto px-4 py-8">

        <h1 class="text-3xl font-bold text-gray-800 mb-6">Lista de Usuarios</h1>

        <!-- Alerta de error (oculta) -->
        <div id="alertaError" class="hidden bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded mb-4">
            No se pudieron cargar los usuarios.
        </div>

        <!-- Cards de usuarios -->
        <div id="contenedor" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            <!-- Se llena con JavaScript -->
        </div>

    </div>

    <script>
        async function cargarUsuarios() {
            try {
                const res = await fetch('http://localhost/api-usuarios/api/usuarios.php');
                const datos = await res.json();

                const contenedor = document.getElementById('contenedor');
                contenedor.innerHTML = '';

                datos.datos.forEach(usuario => {
                    contenedor.innerHTML += `
                        <div class="bg-white rounded-xl shadow-md p-6 hover:shadow-lg transition-shadow">
                            <div class="flex items-center gap-4 mb-4">
                                <div class="w-12 h-12 rounded-full bg-blue-100 flex items-center justify-center">
                                    <span class="text-blue-600 font-bold text-lg">
                                        ${usuario.nombre[0]}
                                    </span>
                                </div>
                                <div>
                                    <h3 class="font-semibold text-gray-800">
                                        ${usuario.nombre} ${usuario.apellido}
                                    </h3>
                                    <p class="text-gray-500 text-sm">${usuario.email}</p>
                                </div>
                            </div>
                            <div class="flex justify-between items-center">
                                <span class="text-xs px-2 py-1 rounded-full ${usuario.activo ? 'bg-green-100 text-green-700' : 'bg-gray-100 text-gray-500'}">
                                    ${usuario.activo ? 'Activo' : 'Inactivo'}
                                </span>
                                <div class="flex gap-2">
                                    <button class="text-sm text-blue-500 hover:text-blue-700">Editar</button>
                                    <button class="text-sm text-red-500 hover:text-red-700">Eliminar</button>
                                </div>
                            </div>
                        </div>
                    `;
                });

            } catch (error) {
                document.getElementById('alertaError').classList.remove('hidden');
            }
        }

        cargarUsuarios();
    </script>

</body>
</html>
```

---

## Bootstrap vs Tailwind — Comparacion rapida

| | Bootstrap | Tailwind |
|---|---|---|
| Instalacion | Una linea en el head | Una linea en el head (CDN) |
| Forma de trabajar | Clases de componentes completos | Clases utilitarias pequeñas |
| Personalizacion | Limitada (o sobreescribir con CSS) | Total libertad |
| Curva de aprendizaje | Mas facil al inicio | Mas a aprender, pero mas flexible |
| Tamanio del CSS final | Mas pesado | Muy ligero (solo lo que usas) |
| Diseño por defecto | Bonito y consistente | Minimo, tu controlas todo |
| Velocidad de desarrollo | Mas rapido para proyectos comunes | Mas lento al inicio, mas rapido despues |
| Uso en la industria | Proyectos medianos, academicos | Proyectos modernos y profesionales |

### Cual aprender primero

Si recien empiezas con librerias CSS: **Bootstrap primero**. Es mas intuitivo porque usas componentes ya hechos y ves resultados rapido.

Una vez que entiendes bien como funciona el HTML y CSS: **Tailwind** para proyectos mas personalizados.
