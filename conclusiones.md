# Conclusiones

## Tarea 8: Idempotencia

**¿Qué significa que un método HTTP sea idempotente?**
Significa que no importa cuántas veces haga exactamente la misma petición, el resultado final en el sistema es el mismo que si la hubiera hecho una sola vez. No duplica información en el servidor por mandar la orden varias veces.

**De los cinco métodos, ¿cuáles lo son y cuáles no?**
- **Idempotentes:** GET, PUT y DELETE.
  - GET solo lee, no modifica nada.
  - PUT reemplaza todo el recurso: si mando los mismos datos 5 veces, el recurso queda igual que después de la primera.
  - DELETE borra el recurso: la primera vez lo elimina y, si lo repito, sigue borrado.
- **No idempotente:** POST, porque cada envío crea un registro nuevo con un ID distinto.
- **PATCH:** depende de cómo esté programado. Si asigna un valor fijo (`"estado": "activo"`) se comporta como idempotente, pero si aplica un cambio relativo (`"intentos": "+1"`) el resultado cambia en cada envío.

**¿Qué observé al repetir las peticiones?**
- **PUT:** envié `05 PUT post 1` [N] veces y todas las respuestas fueron idénticas: `200 OK` con el mismo `title` y el mismo `id`.
- **POST:** envié `04 POST posts` [N] veces y siempre devolvió `201 Created` con el id 101. Después pedí `GET /posts/101` y me dio `404 Not Found`, es decir, la API respondió "creado" pero no guardó nada. En una API real esperaría ids distintos y un 200 en ese GET.
- **DELETE:** envié `07 DELETE post 1` [N] veces y obtuve [códigos de cada envío]. [Explica si el código cambió o se mantuvo.]

Fuente: [MDN Web Docs - Idempotente](https://developer.mozilla.org/es/docs/Glossary/Idempotent)

---

## Tarea 9: Cabeceras de la respuesta

**1. Content-Type**
- Valor que recibí: [el que aparezca en tu respuesta]
- Qué significa: indica el formato del contenido que devuelve el servidor, para que el cliente sepa cómo interpretarlo.

**2. [Nombre de la cabecera]**
- Valor que recibí:
- Qué significa y para qué sirve:

**3. [Nombre de la cabecera]**
- Valor que recibí:
- Qué significa y para qué sirve:

**¿Por qué Content-Type es importante cuando se prueba una API?**
Le confirma al tester o al cliente en qué formato vienen los datos. Si la API devuelve JSON pero la cabecera no lo indica, el cliente podría tratar la respuesta como texto plano o HTML y fallar al procesarla. En mi POST tuve que configurar raw y JSON en Postman, y eso hizo que se enviara `Content-Type: application/json`; sin él, el servidor no habría sabido cómo leer mi cuerpo.

Fuente: (enlace de MDN de cada cabecera)

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