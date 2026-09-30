# Taller de APIs y Postman

**Estudiante:** Stiven Arcila Sánchez
**Código:** **1193354544**
**Asignatura:** Ingeniería de Software II — Cotecnova

## Marco conceptual

Una API es la forma en que un programa le pide cosas a otro. Una API REST lo hace siguiendo un estilo de diseño: todo lo que maneja se trata como un **recurso** (por ejemplo un post o un usuario), cada recurso tiene su propia URL y se trabaja con los métodos de HTTP (GET para leer, POST para crear, PUT y PATCH para modificar y DELETE para borrar). Además, cada petición es independiente: el servidor no tiene que recordar las anteriores. El **recurso** es la "cosa" que la API maneja, y el **endpoint** es la URL concreta donde se accede a ella; por ejemplo, en este taller `/posts` es el endpoint de la colección de posts y `/posts/1` es el endpoint de un post en particular. Un ejemplo de aplicación que depende de APIs es la app del clima del celular: le pide los datos del clima a un servidor y los muestra en la pantalla.

Fuente consultada: [MDN Web Docs - REST](https://developer.mozilla.org/es/docs/Glossary/REST)

## Métodos HTTP

| Método | Operación CRUD | Qué hace |
|--------|----------------|----------|
| GET | Read (leer) | Pide los datos de un recurso sin modificarlo. |
| POST | Create (crear) | Envía datos al servidor para crear un recurso nuevo. |
| PUT | Update (reemplazo completo) | Reemplaza el recurso completo con los datos que envío. |
| PATCH | Update (parcial) | Modifica solo los campos que envío y deja el resto igual. |
| DELETE | Delete (eliminar) | Elimina el recurso indicado en la URL. |

**Lo que comprobé en Postman con JSONPlaceholder:**
- GET devolvió `200 OK` con el post o con la lista de 100 posts.
- POST devolvió `201 Created` con el id `101`, pero al pedir ese id después dio `404`: la API simula la creación.
- PUT, enviando solo `title`, devolvió únicamente `title` e `id`; PATCH devolvió el post completo con el `title` cambiado.
- DELETE devolvió `200 OK` con el cuerpo `{}`.

Fuente consultada: [MDN Web Docs - Métodos de petición HTTP](https://developer.mozilla.org/es/docs/Web/HTTP/Methods)

## Códigos de estado

| Familia | Significado | Ejemplo |
|---------|-------------|---------|
| 1xx | Informativa: la petición se recibió y el proceso continúa | 100 Continue |
| 2xx | Éxito: la petición se procesó correctamente | 200 OK, 201 Created |
| 3xx | Redirección: el recurso está en otra ubicación | 301 Moved Permanently |
| 4xx | Error del cliente: la petición está mal hecha o pide algo que no existe | 404 Not Found |
| 5xx | Error del servidor: la petición era válida pero el servidor falló | 500 Internal Server Error |

En el taller obtuve 200 (lecturas, PUT, PATCH y DELETE), 201 (POST) y 404 (el post 9999 y el 101).

**¿Por qué se separan los errores 4xx de los 5xx? ¿Qué cambia desde el punto de vista de quién tiene la culpa?**

Se separan porque cambia quién tiene que arreglar el problema. En un 4xx el error viene del cliente: pidió algo que no existe (como mi `GET /posts/9999`, que dio 404), mandó los datos mal o no tiene permiso. El servidor está funcionando bien, así que repetir la misma petición sin cambiarla dará el mismo error. En un 5xx la petición estaba bien hecha, pero el servidor falló al procesarla (por ejemplo un 500 por un error interno); en ese caso quien debe corregirlo es quien administra el servidor, y a veces reintentar más tarde sí funciona. Separarlos ayuda a saber dónde buscar el defecto: en la petición o en el servidor.

Fuente consultada: [MDN Web Docs - Códigos de estado HTTP](https://developer.mozilla.org/es/docs/Web/HTTP/Status)

## Cómo reproducir este taller

1. Instala Postman desde postman.com/downloads (se puede usar sin iniciar sesión).
2. Descarga este repositorio o clónalo:
   ```
   git clone https://github.com/STIVEN0919/taller-postman-Arcila-Sanchez.git
   ```
3. En Postman pulsa **Import** y selecciona el archivo `coleccion.json`. Aparecerá la colección `Taller-API`.
4. Ejecuta las peticiones una por una con **Send**, o toda la colección con **Run**. La API (JSONPlaceholder) es pública y no requiere clave.
5. Las pruebas automáticas están en la petición `01 GET post 1`, pestaña **Scripts → After response**. Al enviarla, en **Test Results** deben aparecer 4 pruebas en verde.
6. Compara lo que obtengas con las tablas de `hallazgos.md`.

## Archivos de este repositorio

| Archivo | Contenido |
|---------|-----------|
| `README.md` | Informe principal: marco conceptual, métodos HTTP, códigos de estado y cómo reproducir el taller |
| `hallazgos.md` | Tabla de peticiones con el código esperado y el obtenido, y los resultados de las Tareas 4 a 7 y 10 a 13 |
| `conclusiones.md` | Respuestas de las Tareas 8 y 9 y las dos preguntas de sustentación |
| `coleccion.json` | Colección `Taller-API` exportada desde Postman (formato v2.1) |
| `evidencias/` | Capturas de pantalla de las peticiones y de las pruebas automáticas |

**Capturas en `evidencias/`:**
- `01-get-recurso.png`, `02-get-coleccion.png`, `03-error-404.png`, `04-post-creacion.png`
- `05-test-automatico.png` (cuatro pruebas en verde), `05-test-automatico-verde-1prueba.png` y `05-test-automatico-rojo.png`
- `06-put.png`, `07-patch.png`, `08-delete.png`
- `09-limite-100.png` y `10-limite-101.png`
