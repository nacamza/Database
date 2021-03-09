### Instalar Mysql
En el servidor donde se va a alojar el backend, además de los pasos anteriores, vamos a instalar la base de datos.

Para instalar la base de datos ejecutamos los siguientes comandos
````
sudo apt-get update
sudo apt install mysql-server -y
sudo usermod -d /var/lib/mysql/ mysql
sudo /etc/init.d/mysql start
````
Entramos a la base de datos con ``sudo mysql`` y ejecutamos los siguientes comandos
````
CREATE DATABASE name;
CREATE USER 'develop'@'%' IDENTIFIED BY '@pass';
GRANT ALL PRIVILEGES ON name.* TO 'develop'@'%';

exit
````
Configuramos la bd modificando el archivo mysqld.cnf 
````
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
````
Y comentamos con **#** la siguiente línea
````
bind-address           = 127.0.0.1
````
Por último, reiniciamos el servicio
````
sudo service mysql restart
````
