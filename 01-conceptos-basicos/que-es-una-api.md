# ¿Qué es una API? Explicación profunda

## La analogía del restaurante (versión extendida)

Imagina que vas a un restaurante:

```
[TÚ / CLIENTE] [MESERO / API] [COCINA / SERVIDOR]
 | | |
 |-- "Quiero una pizza" -----> | |
 | |--- "Mesa 5 pide pizza" ----> |
 | | |
 | |<-- "Pizza lista" ----------- |
 |<-- "Aquí está tu pizza" --- | |
```

**Tú** = La aplicación o persona que necesita datos. 
**El mesero** = La API. 
**La cocina** = El servidor con la base de datos.

Tú no puedes entrar a la cocina. No sabes cómo funciona. No necesitas saberlo. Solo le dices al mesero lo que quieres, y él te lo trae.

---

## Definición técnica

Una **API (Application Programming Interface)** es un conjunto de reglas y protocolos que permiten que dos aplicaciones se comuniquen entre sí.

En el contexto web, una API define:
- **Qué** puedes pedir (los endpoints disponibles).
- **Cómo** lo puedes pedir (los métodos y formatos).
- **Qué** te va a devolver (la estructura de la respuesta).

---

## Ejemplo de la vida diaria

Cuando abres la app del clima en tu teléfono:

1. Tu app **no sabe** el clima por sí sola.
2. Tu app le **pregunta** a una API meteorológica: *"¿Cuál es el clima en Madrid ahora mismo?"*
3. La API meteorológica consulta sus servidores.
4. Devuelve la respuesta: *"22°C, soleado, humedad 45%"*.
5. Tu app **muestra** esa información.

Tu app nunca tuvo que saber cómo funcionan los satélites ni las estaciones meteorológicas. Solo usó la API.

---

## ¿Por qué son importantes las APIs?

### 1. Separación de responsabilidades
El frontend (lo que ves) y el backend (la lógica y datos) pueden trabajar por separado y comunicarse a través de la API.

### 2. Reutilización
Una misma API puede ser usada por:
- Una app web.
- Una app móvil iOS.
- Una app móvil Android.
- Otra empresa (APIs públicas).

### 3. Seguridad
El cliente nunca toca directamente la base de datos. Todo pasa por la API, que puede validar, filtrar y controlar el acceso.

### 4. Escalabilidad
Puedes actualizar el backend sin tocar el frontend, y viceversa, siempre que la API mantenga el mismo "contrato".

---

## API REST vs API SOAP

### REST (la que probablemente usas)
- Usa HTTP (el mismo protocolo del navegador).
- Devuelve JSON (fácil de leer y usar).
- Simple, liviano, rápido.
- El estándar moderno.

### SOAP (el veterano)
- Tiene su propio protocolo sobre HTTP.
- Usa XML (más verboso y difícil).
- Más rígido pero más formal.
- Usado en sistemas bancarios y corporativos viejos.

```
REST: { "nombre": "Juan" } Simple, limpio

SOAP: 
<?xml version="1.0"?>
<soap:Envelope xmlns:soap="...">
 <soap:Body>
 <nombre>Juan</nombre>
 </soap:Body>
</soap:Envelope> Mucho más verboso
```

**En tus cursos: REST.**

---

## ¿Qué es una API RESTful?

Una API es **RESTful** cuando sigue los principios REST:

1. **Sin estado (Stateless):** Cada petición es independiente. El servidor no recuerda peticiones anteriores. Si necesita saber quién eres, debes decírselo en cada petición (con un token).

2. **Interfaz uniforme:** Las URLs siguen convenciones predecibles. `/usuarios` para todos, `/usuarios/5` para uno específico.

3. **Recursos:** Todo es un "recurso" (usuario, producto, pedido). Cada recurso tiene su propia URL.

4. **Uso de HTTP:** Usa los métodos HTTP para indicar qué hacer (GET, POST, PUT, DELETE).

---

## API en el contexto de tu curso

En tus clases seguramente hiciste algo así:

```
[XAMPP con PHP corriendo en htdocs]

 Es tu SERVIDOR

 Tu PHP procesa las
 peticiones y responde

 Postman hace las
 peticiones para probar
```

El PHP que escribiste en htdocs **es la API**. Postman **es el cliente** que la prueba.
