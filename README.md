# Practica03

# Arquitectura de una aplicación web LAMP en dos niveles.

## Composicion del sistema

En esta practica lo que queremos es hacer el la arquitectura de la pila LAMP dividida en dos pilares:

 - En uno de ellos meteremos nuestro servidor web apache con los modulos necesarios de php. 
 - Y en el otro pilar tendremos que instalar MySQL Server.

Nuestra arquitectura estara formada por dos capas:

 - Una capa front-end, que tendra el servidor web.
 - Una capa back-end, que tendra el servidor MySQL.
 
Utilizaremos una direccion Ip diferente para cada uno de las maquina que instalaremos.

## Configuracion de las maquinas que hemos creado.

Tendremos que ir al directorio de configuración utilizando la siguiente orden:

```
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

Dentro del archivo de configuración donde nos hemos metido buscaremos el bloque de [mysql] y cambiaremos la siguiente directiva:

```
[mysqld]
bind-address = 127.0.0.1
```
Tendremos que cambiar este valor(127.0.0.1) por la direccion ip donde estara la maquina con el servicio mysql.

Una vez terminada esta configuración tendremos que reiniciar la máquina contenedora de mysql por lo que utilizaremos el comando:

```
sudo /etc/init.d/mysql restart
```

Para volver a iniciarla con la configuración cambiada.

## Asignación de privilegios a los usuarios de mysql.

Para asignar los privilegios adecuados del ususario que se conectara desde la máquina que esta corriendo el servidor web apache HTTP.

Para ello nos meteremos ene el servicio mySQL y ejecutaremos las siguientes ordenes:

```
mysql -u root -p  
mysql> grant all privileges on DATABASE.* to USERNAME@IP-SERVIDOR-HTTP identified by 'PASSWORD';
mysql> flush privileges;
mysql> exit;
```

Si queremos que cualquier usuario se pueda conectar desde cualquier dirección ip utilizaremos el simbolo %.
Modificaremos con el siguiente comando:

```
mysql -u root -p  
mysql> grant all privileges on DATABASE.* to USERNAME@'%' identified by 'PASSWORD';
mysql> flush privileges;
mysql> exit;
```

Cambiaremos los siguientes apartados: DATABASE, USERNAME y IP-SERVIDOR-HTTP por los diferentes alores que vallamos a utilizar.

## Comprobar la lista de usuarios de MySQL.

Para ello emplearemos el comando:

```
select * from mysql.users
```

Lo que nos dervolvera este resultado:

```
+------------------+--------------+
| user             | host         |
+------------------+--------------+
| root             | %            |
| root             | localhost    |
| lamp_user        | %            |
| lamp_user        | localhost    |
| debian-sys-maint | localhost    |
| phpmyadmin       | localhost    |
| mysql.session    | localhost    |
| mysql.sys        | localhost    |
+------------------+--------------+
```

Para poder ver los permisos que tienen los diferentes usuarios emplearemos la siguiente orden:

```
SHOW GRANTS FOR root;
```

lo que nos devolvera el siguiente resultado:

```
+------------------------------------------------+
| Grants for root@%                              |
+------------------------------------------------+
| GRANT ALL PRIVILEGES ON *.* TO 'root'@'%'      |
+------------------------------------------------+
```

## Comprobación de inicio de sesion en MySQLServer

Conectaremos con MySQL desde la máquina de ApacheHTTP para ello tendremos que loguearnos con nuestro usuario por lo que emplearemos el comando:

```
mysql -u USERNAME -p -h IP-SERVIDOR-MYSQL
```

Tambien podemos hacer un telnet al puerto donde esta corriendo MySQL.

```
telnet IP-SERVIDOR-MYSQL 3306
```

En caso que no podamos conectar al servicio MySQL comprobaremos que el servicio esta habilitado y tambien comprobaremos que los puertos que emplea esten abiertos.


