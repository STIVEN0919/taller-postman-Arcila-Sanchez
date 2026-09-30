# Hallazgos

## Fase 2: Tabla de peticiones

| # | Petición        | Código esperado | Código obtenido | ¿Coincide? |
|---|-----------------|-----------------|-----------------|------------|
| 1 | GET /posts/1    | 200             | 200             | Sí         |
| 2 | GET /posts      | 200             | 200             | Sí         |
| 3 | GET /posts/9999 | 404             | 404             | Sí         |
| 4 | POST /posts     | 201             | 201             | Sí         |
| 5 | PUT /posts/1    | 200             | 200             | Sí         |
| 6 | PATCH /posts/1  | 200             | 200             | Sí         |
| 7 | DELETE /posts/1 | 200             | 200             | Sí         |

Los códigos esperados se escribieron y se subieron al repositorio antes de ejecutar las peticiones (ver el historial de commits).

## Tarea 4: GET de un recurso y de una colección

**Petición 1 (GET /posts/1):**
- Código de estado: 200 OK
- Cantidad de elementos: 1 recurso (un objeto JSON entre llaves `{}`).
- Campos: `userId` (id del usuario autor), `id` (id del post), `title` (título) y `body` (contenido).

**Petición 2 (GET /posts):**
- Código de estado: 200 OK
- Cantidad de elementos: 100 (una lista JSON entre corchetes `[...]`; el último `id` es 100).
- Campos: cada elemento tiene los mismos cuatro campos: `userId`, `id`, `title` y `body`.

**¿En qué se diferencian los criterios de aceptación al pedir un recurso y al pedir una colección?**
Cuando pido un recurso (`GET /posts/1`) verifico que llegue un solo objeto, que su `id` sea el que pedí y que tenga los cuatro campos. Cuando pido la colección (`GET /posts`) verifico que llegue una lista, que tenga la cantidad esperada de elementos (100) y que todos tengan la misma estructura. En el primer caso reviso el contenido de un objeto; en el segundo, la cantidad y que todos sean uniformes.

Evidencias: `evidencias/01-get-recurso.png` y `evidencias/02-get-coleccion.png`.

## Tarea 5: Provoca un error a propósito

**¿Este caso de prueba pasó o falló? Justifica tu respuesta.**
El caso **pasó**. Pedí un recurso que no existe (`/posts/9999`), esperaba un `404 Not Found` y el servidor devolvió un `404 Not Found` con el cuerpo `{}`. Un caso falla cuando el resultado obtenido es distinto del esperado, no porque el código sea distinto de 200.

**¿Qué pasaría si esa misma petición hubiera devuelto 200 con un cuerpo vacío? ¿Sería un defecto?**
Sí, sería un defecto. Un 200 le dice al cliente que la consulta salió bien y que el recurso existe, pero el post 9999 no existe. El cliente creería que recibió un dato válido cuando no hay nada, y no sabría que debe manejar un error.

Evidencia: `evidencias/03-error-404.png`.

## Tarea 6: Crear un recurso con POST

**Ids devueltos en los envíos:**
Ejecuté el POST seis veces seguidas (la tarea pedía cinco). En todos los envíos el servidor devolvió `201 Created` con el mismo id: `101`.

**¿Qué observé?**
Aunque ejecuté el POST varias veces seguidas, el id nunca cambió: siempre fue `101` y no subió a 102, 103, etc.

**¿Por qué creo que ocurre?**
JSONPlaceholder es una API de práctica que simula la creación pero no guarda nada. Lo comprobé pidiendo `GET /posts/101` después de los POST y obtuve `404 Not Found`, es decir, el recurso nunca se creó. El id 101 sale porque la colección tiene 100 posts y la API siempre devuelve el siguiente número.

**¿Cómo comprobaría, en una API real, que el recurso se creó de verdad?**
Haciendo un `GET` al id que devolvió el POST (por ejemplo `GET /posts/101`) y esperando un `200` con los mismos datos que envié. Si diera `404`, como en esta API, el recurso no se guardó. También se puede revisar directamente la base de datos del sistema.

Evidencia: `evidencias/04-post-creacion.png`.

## Tarea 7: PUT vs PATCH

En ambas peticiones envié únicamente el campo `title`.

**Respuesta completa de PUT /posts/1:**

```json
{
  "title": "Título actualizado con PUT",
  "id": 1
}
```

**Respuesta completa de PATCH /posts/1:**

```json
{
  "userId": 1,
  "id": 1,
  "title": "Título corregido",
  "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum est autem sunt rem eveniet architecto"
}
```

**Diferencia que encontré:**
Con PUT la respuesta trae solo lo que envié (`title`) más el `id`: los campos `userId` y `body` desaparecieron, porque PUT reemplaza el recurso completo por lo que le mando. Con PATCH la respuesta trae el recurso completo con `userId` y `body` intactos y solo cambió el `title`, porque PATCH modifica únicamente los campos que envío.

**Cuál usaría para corregir un error de escritura en un solo campo, y por qué:**
Usaría PATCH. Si usara PUT tendría que enviar todos los campos del recurso cada vez; si me olvido de alguno, se pierde. Con PATCH solo mando el campo que quiero corregir y el resto queda como estaba.

Evidencias: `evidencias/06-put.png` y `evidencias/07-patch.png`.

## Tarea 10: Valores límite

**Id más alto que devuelve 200:** 100 (`GET /posts/100`)

**Primer id que devuelve 404:** 101 (`GET /posts/101`)

**¿Cómo se llama este tipo de caso de prueba?**
Análisis de valores límite (boundary value analysis).

**¿Por qué se dice que los defectos se concentran ahí?**
Los errores de lógica suelen aparecer en los bordes: un `<` en vez de `<=`, o contar desde 0 en lugar de desde 1, desplaza el límite en un elemento. Con un valor intermedio como 50 esos errores no se ven, pero al probar 100 (último válido) y 101 (primero inválido) sí.

Fuente: [Baeldung - Boundary Value Analysis](https://www.baeldung.com/cs/bva)

Evidencias: `evidencias/09-limite-100.png` y `evidencias/10-limite-101.png`.

## Tarea 11: Otros recursos y ruta anidada

**[COMPLETAR: después de probar en Postman]**

- Recurso 1 probado (por ejemplo `/users`): código, cantidad de elementos y campos.
- Recurso 2 probado (por ejemplo `/comments`): código, cantidad de elementos y campos.
- Ruta anidada (por ejemplo `/posts/1/comments`): código y qué devuelve.
- Cómo deduje la estructura de la URL anidada:

## Tareas 12 y 13: Pruebas automáticas (petición `01 GET post 1`)

Las cuatro pruebas están en la pestaña Scripts → After response de la petición 1 y pasan (4/4).

```javascript
pm.test("El estado es 200", function () {
    pm.response.to.have.status(200);
});

pm.test("La respuesta contiene el campo title", function () {
    pm.expect(pm.response.json()).to.have.property("title");
});

pm.test("El tiempo de respuesta es menor a 1000 ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(1000);
});

pm.test("userId es de tipo número", function () {
    pm.expect(pm.response.json().userId).to.be.a("number");
});
```

| Prueba | Qué verifica |
|---|---|
| El estado es 200 | Que el código de estado de la respuesta sea 200 |
| La respuesta contiene el campo title | Que el JSON devuelto tenga la propiedad `title` |
| El tiempo de respuesta es menor a 1000 ms | Que el servidor responda en menos de un segundo |
| userId es de tipo número | Que el campo `userId` sea un número y no un texto |

**Prueba en verde y en rojo:** primero la prueba del estado pasó (`PASSED`). Luego cambié el 200 por 201 y salió en rojo (`FAILED`) con el mensaje `expected response to have status code 201 but got 200`. Volví a dejarla en 200.

**¿Por qué es importante ver una prueba fallar antes de confiar en ella?**
Porque una prueba que siempre pasa podría estar mal escrita y no comprobar nada. Al cambiar el 200 por 201 vi que la prueba sí se pone en rojo cuando la respuesta no coincide, y así sé que cuando sale en verde es porque de verdad se cumple lo que verifica.

Evidencias: `evidencias/05-test-automatico.png` (4 pruebas en verde), `evidencias/05-test-automatico-verde-1prueba.png` (primera prueba en verde) y `evidencias/05-test-automatico-rojo.png` (prueba en rojo).
