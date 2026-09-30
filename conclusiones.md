# Conclusiones

## Tarea 8: Idempotencia

**¿Qué significa que un método HTTP sea idempotente?**
Significa que no importa cuántas veces haga exactamente la misma petición, el resultado final en el sistema es el mismo que si la hubiera hecho una sola vez. No duplica información en el servidor por mandar la orden varias veces.

**De los cinco métodos, ¿cuáles lo son y cuáles no?**
- **Idempotentes:** GET, PUT y DELETE.
  - GET solo lee, no modifica nada.
  - PUT reemplaza todo el recurso: si mando los mismos datos 5 veces, el recurso queda igual que después de la primera.
  - DELETE borra el recurso: la primera vez lo elimina y, si lo repito, sigue borrado.
- **No idempotente:** POST, porque cada envío crea un registro nuevo con un id distinto.
- **PATCH:** depende de cómo esté programado. Si asigna un valor fijo (`"estado": "activo"`) se comporta como idempotente, pero si aplica un cambio relativo (`"intentos": "+1"`) el resultado cambia en cada envío.

**¿Qué observé al repetir las peticiones?**
- **PUT:** envié `05 PUT post 1` 5 veces seguidas y todas las respuestas fueron idénticas: `200 OK` con el mismo `title` y el mismo `id`.
- **POST:** envié `04 POST posts` 4 veces seguidas y siempre devolvió `201 Created` con el id 101. Después pedí `GET /posts/101` y me dio `404 Not Found`: la API respondió "creado" pero no guardó nada. En una API real esperaría ids distintos y un 200 en ese GET.
- **DELETE:** envié `07 DELETE post 1` 3 veces seguidas y las tres respondieron `200 OK` con el cuerpo `{}`. El código no cambió entre un envío y otro. En una API real lo normal sería que la primera vez diera 200 o 204 y las siguientes 404, porque el recurso ya no existe; aun así el efecto en el servidor es el mismo (el recurso queda borrado), y eso es lo que define la idempotencia: mismo efecto, aunque la respuesta pueda cambiar. En esta API además el DELETE es simulado: después de borrar, `GET /posts/1` siguió respondiendo `200 OK` (se ve en mis pruebas automáticas), es decir, no borró nada.

**¿Por qué importa esta diferencia?**
Si un pago se manda por POST y la conexión se cae antes de recibir la confirmación, reintentar puede cobrar dos veces. Con un método idempotente como PUT, reintentar es seguro porque no cambia nada más.

Fuente: [MDN Web Docs - Idempotente](https://developer.mozilla.org/es/docs/Glossary/Idempotent)

---

## Tarea 9: Cabeceras de la respuesta

Cabeceras tomadas de la respuesta de `GET /posts/100` (pestaña Headers de la respuesta).

**1. Content-Type**
- Valor que recibí: `application/json; charset=utf-8`
- Qué significa: le dice al cliente en qué formato viene el cuerpo de la respuesta (JSON, codificado en UTF-8) para que sepa cómo interpretarlo.

**2. Cache-Control**
- Valor que recibí: `max-age=43200`
- Qué significa: indica cuánto tiempo se puede reutilizar la respuesta sin volver a pedirla al servidor: 43200 segundos, o sea 12 horas. En la misma respuesta vi `Age: 5687`, que indica que llevaba casi 95 minutos guardada en caché. Al probar una API esto importa, porque puedo estar viendo un dato guardado y no uno recién consultado.

**3. X-Ratelimit-Remaining (junto con X-Ratelimit-Limit)**
- Valor que recibí: `X-Ratelimit-Limit: 1000` y `X-Ratelimit-Remaining: 998`
- Qué significa: el servidor limita cuántas peticiones puedo hacer en un periodo (1000) y me avisa cuántas me quedan (998). Si las agoto, el servidor rechaza mis peticiones, algo a tener en cuenta cuando se automatizan muchas pruebas.

**¿Por qué Content-Type es importante cuando se prueba una API?**
Le confirma al tester o al cliente en qué formato vienen los datos. Si la API devuelve JSON pero la cabecera no lo indica, el cliente podría tratar la respuesta como texto plano o HTML y fallar al procesarla. En mi POST tuve que configurar raw y JSON en Postman, y eso hizo que se enviara `Content-Type: application/json`; sin él, el servidor no habría sabido cómo leer mi cuerpo.

Fuentes:
- [MDN Web Docs - Content-Type](https://developer.mozilla.org/es-ES/docs/Web/HTTP/Headers/Content-Type)
- [MDN Web Docs - Cache-Control](https://developer.mozilla.org/es/docs/Web/HTTP/Headers/Cache-Control) y [MDN Web Docs - Age](https://developer.mozilla.org/es/docs/Web/HTTP/Headers/Age)
- X-Ratelimit no es una cabecera estándar de HTTP (no está en MDN): cada API la define; me guié por la documentación de otra API que usa la misma convención, [WHOOP - API Rate Limiting](https://developer.whoop.com/docs/developing/rate-limiting)

---

## Preguntas de sustentación

**1. ¿Qué le faltaría a la tabla de la Fase 2 para ser un plan de pruebas formal?**
- Precondiciones y datos de prueba.
- Pasos detallados de ejecución.
- Ambiente de pruebas (desarrollo, staging o producción).
- Criterios de aceptación más allá del código HTTP.
- Prioridad y severidad de cada caso.
- Responsable y versión de lo que se prueba.

**2. ¿Por qué un 404 puede ser una buena noticia y un 200 puede ser un defecto?**
Una prueba pasa cuando el resultado obtenido coincide con el esperado, no cuando el código es 200. En mi fila 3 pedí `GET /posts/9999`, esperaba 404 y obtuve 404, así que el caso pasó y el sistema validó bien. Si esa misma petición hubiera devuelto 200 con cuerpo vacío, la API le diría al cliente que la consulta salió bien cuando el dato no existe, y eso sí sería un defecto.
