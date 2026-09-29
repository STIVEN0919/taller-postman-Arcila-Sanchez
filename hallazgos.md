# Hallazgos

## Fase 2: Tabla de peticiones

| # | Petición        | Código esperado | Código obtenido | ¿Coincide? |
|---|-----------------|-----------------|-----------------|------------|
| 1 | GET /posts/1    | 200             |    200          |   si       |
| 2 | GET /posts      | 200             |    200          |   si       |
| 3 | GET /posts/9999 | 404             |    404          |   si       |
| 4 | POST /posts     | 201             |                 |            |
| 5 | PUT /posts/1    | 200             |                 |            |
| 6 | PATCH /posts/1  | 200             |                 |            |
| 7 | DELETE /posts/1 | 200             |                 |            |

## Tarea 4: GET de un recurso y de una colección

**Petición 1 (GET /posts/1):**
- Código de estado: 200 OK
- Cantidad de elementos: 1 recurso (objeto JSON único encerrado entre llaves `{}`).
- Campos: `userId` (ID del usuario autor), `id` (ID único del post), `title` (título) y `body` (contenido).

**Petición 2 (GET /posts):**
- Código de estado: 200 OK
- Cantidad de elementos: 100 elementos (un arreglo/lista JSON encerrado entre corchetes `[...]`).
- Campos: Cada elemento dentro de la lista contiene exactamente los mismos campos: `userId`, `id`, `title` y `body`.

**¿En qué se diferencian los criterios de aceptación al pedir un recurso y al pedir una colección?**
Al solicitar un **recurso individual**, el criterio de aceptación evalúa que la respuesta devuelva un único objeto JSON bien estructurado con los datos específicos solicitados por su ID. En cambio, al solicitar una **colección**, el criterio de aceptación valida que el servidor retorne un arreglo o lista con múltiples elementos, verificando la estructura uniforme de los registros, la cantidad total de elementos o la correcta aplicación de paginación o filtros si existieran.