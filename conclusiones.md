# Conclusiones

## Tarea 8: Idempotencia

**Qué significa (con mis palabras):**
Un método es idempotente cuando mandar la misma petición una vez o diez veces deja el servidor en el mismo estado. Lo que cuenta es el efecto final, no que la respuesta sea idéntica.

**Métodos idempotentes y no idempotentes:**
- **Idempotentes:** GET, PUT y DELETE.
  - GET solo lee, no cambia nada.
  - PUT reemplaza el recurso completo, así que repetirlo con los mismos datos deja el mismo resultado.
  - DELETE borra el recurso; si lo repito, el recurso sigue borrado.
- **No idempotente:** POST, porque cada envío crea un recurso nuevo.
- **PATCH:** depende de cómo se use. Si asigna un valor fijo (`"estado": "activo"`) se comporta como idempotente, pero si aplica un cambio relativo (sumar 1 a un contador), cada repetición cambia el resultado. Por eso la especificación no lo garantiza como idempotente.

**Qué observé al repetir PUT:**
Envié `05 PUT post 1` 4 veces y las cuatro respuestas fueron idénticas: `200 OK` con el mismo cuerpo. [Confirma que es lo que viste.]

**Qué observé al repetir POST:**
Envié `04 POST posts` 4 veces y el id devuelto fue siempre `101`. En una API real esperaría ids distintos (101, 102, 103...), uno por cada recurso creado. Para averiguar por qué se repite, ejecuté `GET /posts/101` y obtuve [código que te salió]. [Explica con eso si el recurso se guardó o no.]

**Qué observé al repetir DELETE:**
Envié `07 DELETE post 1` 3 veces y obtuve [códigos que te salieron en cada envío]. [Explica si la respuesta cambió y si el efecto final es el mismo.]

**Por qué importa esta diferencia:**
Si un pago se manda por POST y la conexión se cae antes de recibir la confirmación, reintentar puede cobrar dos veces. Con un método idempotente como PUT, reintentar es seguro porque no cambia nada más.

**Fuente:** [MDN Web Docs - Idempotente](https://developer.mozilla.org/es/docs/Glossary/Idempotent)