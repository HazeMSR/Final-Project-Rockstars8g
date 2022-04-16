# Instrucciones
Instrucciones para realizar los cambios del repo:
https://github.com/HazeMSR/Django-REST-Exercise/tree/pytest

1. Detener tanto los servicios de docker (postgres_db y web) tanto como los contenedores (postgres y django).
2. Borrar tanto los servicios de docker como los contenedores (Si no pueden borrar el contenedor porque no existe, está bien).
3. Borrar las carpetas de migraciones de todas las app del proyecto.
4. Crear las carpetas de migraciones con el comando mkdir únicamente y adentro crear un nuevo archivo con touch llamado __init__.py en dichas carpetas.
5. Modificar el archivo docker-compose.yml:
  - En el servicio "web" borrar de la linea de commando el comando pytest.
  - En el servicio "web" modificar el comando makemigrations de tal manera que haga todas las migraciones especificando una aplicación a la vez  
  > python manage.py makemigrations books
  - Obviamente concatenando con los carácteres && (Nota, si no les funciona bien el comando migrate, háganlo de igual manera uno a la vez por aplicación de su sistema).
6. Ingresar al repositorio de arriba, dar click donde dice "45 commits", seleccionar los commits que dicen "Adding swagger and User model" y "Reverse relationship example" para visualizar los cambios realizados en los archivos y copiarlos. (Obviamente omitir las migraciones y el pycache)
7. Correr los servicios con el comando:
> docker-compose --env-file .env.dev up --build

# Explicación
## Swagger
Es un documentador de endpoints realizado para la arquitectura REST (Ojo: en GraphQL ya tiene la documentación incluída).
Si entran a la página IP:PORT/swagger/ van a poder ver todo lo que solicitan sus peticiones de cada endpoint del sistema, así cómo los parámetros que regresa la respuesta.
Si quieren leer la documentación de éste módulo pueden entrar aquí (Swagger)[https://drf-yasg.readthedocs.io/en/stable/readme.html]

## Abstract User
Se modificó el Usuario por default de Django para ser capaz de asignarle un "type" a User ya que para su proyecto final van a tener que utilizar dos distintos tipos de usuario.
El administrador y el comprador, si vieron las modificaciones del proyecto utilicé un Enum para indicar que mi tipo de usuario 1 es el comprador y el 2 es el administrador, por default a la hora de agregar un nuevo registro se considerará al nuevo usuario como comprador así que no es necesario modificar el POST del usuario a menos que se necesite crear un admin.
Nota: Si les sale un error de BD como "INSERT INTO User, Thing, blablabla" o "Table doesn't exists" o "Your model is using an User instance" a la hora de hacer migrations.
Significa que no se borró bien la base de datos y deben borrarla de nuevo).

## Reverse relationship
Si se acuerdan de las clases de base de datos una relación normal es la forma en que conectamos una tabla con otra.
En el ejemplo que les acabo de agregar tengo dos nuevas tablas: Employee y Library la cual la relación es de la siguiente manera:
```
______________                   ________________
|  Employee  |  <-------------- |    Library    |
______________                  _________________
```
Es una relación 1 a muchos en la cuál UNA librería tiene MUCHOS empleados. Si ven el código de los modelos se darán cuenta que la librería es el modelo más simple de todos, pero el empleado tiene un atributo que la librería no tiene la llave foránea, ésto nos ayuda a identificar cada empleado con el ID de una librería.
Si nosotros necesitaramos realizar una consulta al empleado nos regresará algo similar a ésto:

```
{
  "id": 1,
  "name": "Jose",
  "library": 1
}
```
Entonces si nosotros necesitaramos saber a qué librería pertenece ese empleado nosotros, solamente tendríamos que utilizar el library ID que nos regresó el empleado en una nueva consulta, y nos regresaría algo similar a ésto:

```
{
  "id": 1,
  "name": "Gandhi centro"
}
```
¿Pero si nosotros quisieramos consultar la información al revés? Ósea que nos devuelva todos los usuarios relacionados con la librería con el ID igual a 1.
¿Cómo se realizaría? Para eso existen las relaciones inversas las cuales nos sirven para devolver un conjunto de datos utilizando el ID en cuestión.
Solamente se debe de agregar el nombre "related_name" en la llave fóranea, éste será el nombre de nuestra relación inversa. Específicar el atributo StringRelatedFields, con los parámetros Many igual a True y read_only igual a True.
Nota: Si no se específica read_only igual a True el serializador tomará en cuenta que ese atributo también se debe considerar al momento de la escritura (POST o PUT), así que te lo tomará como obligatorio.
Sin embargo, si lo quieren realizar de ésta manera deben sobreescribir el método create del serializador de tal manera, que guarde los dos modelos a la hora de crear el objeto.

Si introducimos varios empleados a nuestra base de datos a la hora de realizar nuestra relación inversa les va a devolver algo cómo ésto:

```
{
  "id": 1,
  "name": "Gandhi centro",
  "employees": [
    {
      "id": 1
    },
    {
      "id": 2
    }
  ]
}
```
De ésta manera obtuvimos los empleados que trabajan actualmente en la librería Gandhi sin tener que hacer un query extra.

También en la vista de las librerías les puse un ejemplo de sobreescritura de los métodos list y retrieve, para que tengan una idea de cómo realizar querysets más complejos a la hora de hacer sus viewsets.
Sin embargo si son más complejas las acciones que requieren de preferencia utilicen un APIView o GenericAPIView.

(REST Views)[https://www.django-rest-framework.org/api-guide/generic-views/]
