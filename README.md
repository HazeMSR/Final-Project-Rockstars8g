# Final-Project-Rockstars8g

## Descripción del sistema
El proyecto final debe constar de una tienda de música, ésta tienda permitirá comprar al usuario final dos tipos de canciones: físicas y virtuales. Si el formato es físico obviamente no va a poder comprar una canción solamente a menos que sea un disco del tipo "Single", tendrá que comprar todo el álbum. 

Si es virtual el usuario podrá comprarlas canciones individualmente, obviamente el precio del álbum físico debe ser diferente al del virtual. Las canciones deben contar con un identificador de álbum, nombre, duración, lista de artistas, archivo y preview, si el usuario previamente compró el álbum o la canción, sin importar que sea virtual o físico debe poder escuchar la canción completa (Si el usuario compra la canción en físico también debe poder escucharla de manera virtual), de otra manera solo podrá escuchar el preview qué constará de los 15 segundos más significativos de la canción. 

Los artistas deben tener nombre, nacionalidad, álbumes y top 5 de canciones más escuchadas. Los álbumes deben tener imagen, nombre, géneros, lista de id de canciones, fecha de lanzamiento. 

Existirán dos tipos de usuarios el comprador y el administrador, el comprador podrá crear playlist con las canciones que ha comprado, deberá tener una lista de direcciones, en la cual tendrá una dirección por default para mandar los álbumes físico que haya comprado pero también podrá cambiar ésta dirección al momento de la compra, podrá buscar los álbumes o canciones por nombre, género, álbum o artista. Al igual que los podrá ordenar para que aparezcan ordenados de manera ascendente o descendente utilizando la fecha se subida como referencia, está funcionalidad también debe aplicar para las canciones en una playlist.

La compra solamente debe tener un arreglo de álbumes o canciones (Según a usted le parezca debido), la fecha de la compra, usuario y el ID de la compra, se puede crear un carrito de comprar para implementar ésta funcionalidad o hacerla así directo. Así también el usuario debe poder ver todas las compras que ha hecho en el pasado. La función de comprar álbumes solamente se va a "Simular" mediante un botón que diga "Buy", éste realizará la compra en automático al momento de presionarlo.

# Requisitos del Back
- No deben utilizar la API de Spotify u otra API de música.
- Todos los endpoints deben utilizar autenticación excepto el login y el registro de usuario (Si quieren también pueden hacer un endpoint sin permiso que permita únicamente la reproducción de los preview sin necesidad de autenticarse, pero ésto no es obligatorio lo consideraré un extra).
- Deben utilizar una base de datos relacional, no es obligatorio que sea Postgres pero si utilizan otra deben ocuparse de los problemas de configuración ustedes mismos.
- Puede ser en cualquier lenguaje (Python, Java, Javascript, .NET, Ruby, Scala) y con arquitectura REST o GraphQL (Si lo hacen con arquitectura GraphQL lo tomaré como un extra), si utilizan arquitectura REST deben documentar todos sus endpoints, pueden utilizar Swagger o cualquier otra herramienta que conozcan.
- Recuerden que sólamente pueden usar los puertos que utilizamos en las clases previas, ya que por motivos de seguridad no es apto tener muchos puertos abiertos a la vez.

# Requisitos MongoDB (NoSQL)

Cuando se consulte las canciones de un álbum, en lugar de devolver las canciones en conjunto de el ID del álbum y el arreglo de IDs de los artistas debe devolver el nombre de los artistas y el del álbum también.

Se debe poder clasificar utilizando el número de compras, ésto para saber cuáles son las canciones más vendidas y las menos vendidas, ósea, a la hora de la consulta se debe crear un nuevo atributo o "columna"  pero sólo a la hora de la consulta que nos permita saber el rango o lugar que ocupa esa canción, no nos interesa guardar éste rango dentro de la BD así que se debe crear únicamente cuando se realiza la consulta. A la vez ésta consulta se debe poder clasificar por álbum, artista o entre un rango de fechas (fecha de inicio y fecha de termino) éstas fechas las pueden sacar de los tickets utilizados en las compras de los usuarios.


Se debe realizar una consulta que permita realizar una comparativa cuál fue el semestre, mes o bimestre que tuvo más ventas en canciones. A la vez se deben poder clasificar por álbum o artista.
Se debe poder consultar las canciones en las que un artista hizo colaboración, descartando las canciones en las que no aparece del mismo álbum así también en las que canta solo.


Se debe poder buscar las canciones utilizando varios artistas y no solamente uno, para ésto se introducirá una coma en la barra de búsqueda para separar cada uno de los artistas. La consulta debe devolver las canciones en las que colaboran y en segundo lugar sus canciones más escuchadas de ambos (Las 5 primeras de cada uno y después el resto).

(MongoDB Views)[https://www.mongodb.com/docs/manual/core/views/]

## Deployar node a producción
(Node)[https://dev.to/sudarshansb143/deploy-react-apps-in-production-pretty-easily-49jc]

## Puerto para el front en el server
Sus puertos serán 110XX

## Extras
- Integración de la API de Paypal o MercadoPago para realizar la compra de los álbumes.
- Compartir las playlist o canciones utilizando la API de Facebook.
- Gráficas mostrando diferentes análisis realizadoss a las canciones o álbumes.
