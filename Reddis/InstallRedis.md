## Instalar Redis en ubuntu
Comience por actualizar su aptcaché de paquetes local
````
sudo apt update
`````
Luego instale Redis
````
sudo apt install redis-server
````
Entramos a la configuracion de Redis
````
sudo nano /etc/redis/redis.conf
````
Dentro del archivo, busque la superviseddirectiva. Esta directiva le permite declarar un sistema de inicio para administrar Redis como un servicio. Dado que está ejecutando Ubuntu, que usa el sistema de inicio systemd , cambie esto a systemd
````
# If you run Redis from upstart or systemd, Redis can interact with your
# supervision tree. Options:
#   supervised no      - no supervision interaction
#   supervised upstart - signal upstart by putting Redis into SIGSTOP mode
#   supervised systemd - signal systemd by writing READY=1 to $NOTIFY_SOCKET
#   supervised auto    - detect upstart or systemd method based on
#                        UPSTART_JOB or NOTIFY_SOCKET environment variables
# Note: these supervision methods only signal "process is ready."
#       They do not enable continuous liveness pings back to your supervisor.
supervised systemd
````
Luego, inicie el servicio Redis
````
service redis-server start
````
Para probar que Redis está funcionando correctamente, conéctese al servidor usando redis-cliel cliente de línea de comandos de Redis
`````
redis-cli
`````
Compruebe que puede configurar claves ejecutando
````
set test "It's working!"
````
Recupere el valor escribiendo:
````
get test
````
## Vinculación a localhost
De forma predeterminada, solo se puede acceder a Redis desde localhost.
Para cambiar esto, abra el archivo de configuración de Redis para editarlo:
`````
sudo nano /etc/redis/redis.conf
`````
Localice esta línea y coméntela con #
````
#bind 127.0.0.1 ::1
````
Reinicie el servicio
````
service redis-server restart
````
Ahora Redis esta expuesto al mundo
## Configuración de una contraseña de Redis
La contraseña se configura directamente en el archivo de configuración de Redis /etc/redis/redis.conf, así que abra ese archivo con su editor preferido:
````
sudo nano /etc/redis/redis.conf
````
Desplácese hasta la SECURITYsección y busque una directiva comentada que dice:
````
. . .
# requirepass foobared
. . .
````
Elimine el comentario quitando el #, y cambie foobareda una contraseña segura. Podemos generar una contraseña segura con openssl
````
openssl rand 60 | openssl base64 -A
````
Después de configurar la contraseña, guarde y cierre el archivo, luego reinicie Redis
````
service redis-server restart
```` 
## Cambio de nombre de los comandos peligrosos
La otra función de seguridad integrada en Redis implica cambiar el nombre o deshabilitar por completo ciertos comandos que se consideran peligrosos [Lista comandos](https://redis.io/commands).

Algunos de los comandos que se consideran peligrosos incluyen: FLUSHDB, FLUSHALL, KEYS, PEXPIRE, DEL, CONFIG, SHUTDOWN, BGREWRITEAOF, BGSAVE, SAVE, SPOP, SREM, RENAME, y DEBUG.

Para cambiar el nombre o deshabilitar los comandos de Redis, abra el archivo de configuración una vez más:
````
sudo nano  /etc/redis/redis.conf
````
Para deshabilitar un comando, simplemente cámbiele el nombre a una cadena vacía
````
. . .
# It is also possible to completely kill a command by renaming it into
# an empty string:
#
rename-command FLUSHDB ""
rename-command FLUSHALL ""
rename-command DEBUG ""
. . .
````
Para cambiar el nombre de un comando, asígnele otro nombre como se muestra en los ejemplos siguientes.
````
. . .
# rename-command CONFIG ""
rename-command SHUTDOWN SHUTDOWN_MENOT
rename-command CONFIG ASC12_CONFIG
. . .
````
Después de cambiar el nombre de un comando, aplique el cambio reiniciando Redis
