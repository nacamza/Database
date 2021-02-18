## Install PostgreSQL

[Tutorial instalar postgresql](https://www.digitalocean.com/community/tutorials/como-instalar-y-utilizar-postgresql-en-ubuntu-18-04-es)
````
sudo apt update
sudo apt install postgresql postgresql-contrib
````
Iniciar PostgreSQL
````
sudo service postgresql start
````
Para ver el estado del servicio usamos
````
sudo service postgresql status
````
Para ver la Ip y el puerto de los servicios instalados usamos
````
sudo lsof -i -P -n
````
### Crear estructura en PostgreSQL

Vamos a crear en PostgreSQL lo siguiente:
- User: vizion
- Pass: @viziona2021
- Tabla: vizion

#### Crear Usuario vizion en PostgreSQL:
Ejecutamos el comando ``createuser`` en PostgreSQL con el usuario **postgres**, nos va a pedir el nombre del usuario y nos permite asignarle permisos de superusuario
````
   sudo -u postgres createuser --interactive
````
#### Crear el usuario vizion en Linux, podemos usar la misma password de PostgreSQL **@vizion2021**
````
sudo adduser vizion
````
#### Creamos la base de datos vizion
````
sudo -u postgres createdb vizion
````
#### Ingresar a PostgreSQL con el usuario vizion
````
sudo -u vizion psql
````
#### Establecemos el pass **@nacamza2021** al usuario nacamza en PostgreSQL
````
\password vizion
````
### Probamos la conexión con la bd
````
psql -U vizion -d vizion -h 127.0.0.1 -W
````
#### Para listar las bd de PostgreSQL
````
\l
````
#### Para ver las tablas 
````
\d
````
