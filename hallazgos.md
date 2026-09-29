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

## Tarea 5: Provoca un error a propósito

**¿Este caso de prueba pasó o falló? Justifica tu respuesta.**
El caso de prueba **PASÓ**. La prueba consistía en validar cómo reacciona la API ante la solicitud de un recurso inexistente. Como se esperaba un código `404 Not Found` y el servidor devolvió un `404 Not Found`, la prueba cumplió con la expectativa. Un caso de prueba solo falla cuando el resultado obtenido es diferente al esperado, no porque el código devuelto sea distinto de 200.

**¿Qué pasaría si esa misma petición hubiera devuelto 200 con un cuerpo vacío? ¿Sería un defecto?**
Sí, **sería un defecto del sistema (un bug)**. Devolver un código `200 OK` indica al cliente que la petición fue exitosa y el recurso existe. Si el recurso `9999` no existe en la base de datos, responder con 200 engañaría al cliente haciéndole creer que la consulta fue correcta, violando las buenas prácticas y estándares de la arquitectura REST[cite: 2].

## Tarea 6: Crear un recurso con POST

**Ids devueltos en los cinco envíos:**
En los 5 envíos consecutivos, el servidor devolvió exactamente el mismo ID: `101`[cite: 2, 5].

**¿Qué observé?**
Observé que, a pesar de ejecutar la petición POST cinco veces seguidas, la API responde en cada ocasión con el código de estado `201 Created`[cite: 5] y asigna el mismo id de recurso (`101`)[cite: 2, 5]. El contador de IDs no incrementó a 102, 103, etc.

**¿Por qué creo que ocurre?**
Ocurre porque JSONPlaceholder es una API falsa (mock API) destinada únicamente a pruebas y prototipado. No guarda de manera persistente los nuevos datos en una base de datos real. Simula la creación del recurso y responde de forma estática con el id sintético `101` (ya que la colección original contiene 100 registros).

**¿Cómo comprobaría, en una API real, que el recurso se creó de verdad?**
En un entorno de producción o API real, lo comprobaría de las siguientes dos maneras:
1. **Mediante una petición GET:** Realizando una petición `GET /posts/101` inmediatamente después para consultar si el recurso existe y devuelve el mismo cuerpo de datos que envié.
2. **Consultando la base de datos:** Verificando directamente en el motor de base de datos o almacenamiento persistente del backend si el registro fue insertado con éxito.