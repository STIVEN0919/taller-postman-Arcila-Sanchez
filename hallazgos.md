# Hallazgos

## Fase 2: Tabla de peticiones

| # | Petición        | Código esperado | Código obtenido | ¿Coincide? |
|---|-----------------|-----------------|-----------------|------------|
| 1 | GET /posts/1    | 200             |                 |            |
| 2 | GET /posts      | 200             |                 |            |
| 3 | GET /posts/9999 | 404             |                 |            |
| 4 | POST /posts     | 201             |                 |            |
| 5 | PUT /posts/1    | 200             |                 |            |
| 6 | PATCH /posts/1  | 200             |                 |            |
| 7 | DELETE /posts/1 | 200             |                 |            |