# Hallazgos

## Fase 2: Tabla de peticiones

| # | Petición        | Código esperado | Código obtenido | ¿Coincide? |
|---|-----------------|-----------------|-----------------|------------|
| 1 | GET /posts/1    | 200             |    200          |   si       |
| 2 | GET /posts      | 200             |    200          |   si       |
| 3 | GET /posts/9999 | 404             |                 |            |
| 4 | POST /posts     | 201             |                 |            |
| 5 | PUT /posts/1    | 200             |                 |            |
| 6 | PATCH /posts/1  | 200             |                 |            |
| 7 | DELETE /posts/1 | 200             |                 |            |

## Tarea 4: GET de un recurso y de una colección

**Petición 1 (GET /posts/1):**
- Código de estado:
- Cantidad de elementos:
- Campos:

**Petición 2 (GET /posts):**
- Código de estado:
- Cantidad de elementos:
- Campos:

**¿En qué se diferencian los criterios de aceptación al pedir un recurso y al pedir una colección?**
(tu respuesta con tus palabras)