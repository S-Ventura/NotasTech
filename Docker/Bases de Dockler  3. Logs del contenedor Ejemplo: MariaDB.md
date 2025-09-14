## Maria DB imagen oficial


TAG

lts-ubi9

Last pushed 1 day by doijanky
```bash
docker pull mariadb:lts-ubi9
```
10.6.23-jammy

TAG
Last pushed 3 days by doijanky
```bash
docker pull mariadb:10.6.23-jammy
```

```bash
luna@luna:~/Documentos/NotasTech$ docker container run --name mariadb1 -e MYSQL_ROOT_PASSWORD=admin -p 3306:3306 -d mariadb:latest
Unable to find image 'mariadb:latest' locally
latest: Pulling from library/mariadb
8057d4c0a8e3: Pull complete 
637be8612834: Pull complete 
edd82d392121: Pull complete 
162fcbecc8c8: Pull complete 
b6d05662ac2d: Pull complete 
b71466b94f26: Pull complete 
bcd08ae2b55f: Pull complete 
3409c89ba295: Pull complete 
Digest: sha256:b30cc65b57a11a2e791ad5c06284e599fe9f1bf3fe9081a88d85bcf36389be4a
Status: Downloaded newer image for mariadb:latest
ba5ba485a2c485312037eec2c456d67ee31968550845fbbca145f0810a254fc5
```

Aqui con MYSQL_ROOT_PASSWORD=admin estoy definiendo la contraseña explícitamente como admin mediante esta parte.
El usuario root tendrá como contraseña admin.
No necesitas buscar nada en los logs si ya pasaste esa variable.
🤔 ¿Y si no pasas MYSQL_ROOT_PASSWORD?

Si ejecutas el contenedor sin esa variable, por ejemplo:
```bash
docker run --name mariadb1 -p 3306:3306 -d mariadb
```
El contenedor no arrancará correctamente (por seguridad).

Usar la imagen **mariadb:jammy** probablemente causó el comportamiento inesperado, y te explico por qué:

# 🐧 ¿Qué es jammy?

**jammy** es el nombre clave de Ubuntu 22.04 LTS.

Cuando usas mariadb:jammy, estás usando una imagen basada en Ubuntu, no una imagen oficial de MariaDB.

Estas imágenes suelen tener un entorno más minimalista o personalizado y no garantizan que MariaDB esté configurado automáticamente, ni que incluya utilidades como el cliente mysql o los scripts de inicialización estándar.

```bash 
luna@luna:~/Documentos/NotasTech$ docker container run --name mariadb1 -e MYSQL_ROOT_PASSWORD=admin -p 3306:3306 -d mariadb:10.6.23-jammy
Unable to find image 'mariadb:10.6.23-jammy' locally
10.6.23-jammy: Pulling from library/mariadb
289dc4db3e19: Pull complete 
0c0a5c900eca: Pull complete 
51207dd589c2: Pull complete 
d718c600b876: Pull complete 
2e04a2e41adf: Pull complete 
a3be5d4ce401: Pull complete 
3b57d67d4492: Pull complete 
8876e34a0e4a: Pull complete 
Digest: sha256:8cc3b056dea3ae8abd4035aef165823d3158be2f2ca15cda4b8502bfc4dc746b
Status: Downloaded newer image for mariadb:10.6.23-jammy
2c9cbe652aebbe8f9ba0f44bd3f8384f83fc82938761834367109daa9b8ebba4
luna@luna:~/Documentos/NotasTech$ docker container exec -it mariadb1 bash
root@2c9cbe652aeb:/# wich mysql
bash: wich: command not found
root@2c9cbe652aeb:/# which mysql
/usr/bin/mysql
root@2c9cbe652aeb:/# docker container exec -it mariadb1 mysql -u root -p
bash: docker: command not found
root@2c9cbe652aeb:/# exit
exit
luna@luna:~/Documentos/NotasTech$ docker container exec -it mariadb1 mysql -u root -p
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 3
Server version: 10.6.23-MariaDB-ubu2204 mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> \h

General information about MariaDB can be found at
http://mariadb.org

List of all client commands:
Note that all text commands must be first on line and end with ';'...

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.002 sec)

MariaDB [(none)]> CREATE DATABASE testdb;
Query OK, 1 row affected (0.001 sec)

MariaDB [(none)]> USE testdb;
Database changed
MariaDB [testdb]> CREATE TABLE usuarios (
    ->   id INT AUTO_INCREMENT PRIMARY KEY,
    ->   nombre VARCHAR(50)
    -> );
Query OK, 0 rows affected (0.014 sec)

MariaDB [testdb]> INSERT INTO usuarios (nombre) VALUES ('Ana'), ('Luis'), ('María');
Query OK, 3 rows affected (0.001 sec)
Records: 3  Duplicates: 0  Warnings: 0

MariaDB [testdb]> SELECT * FROM usuarios;
+----+--------+
| id | nombre |
+----+--------+
|  1 | Ana    |
|  2 | Luis   |
|  3 | María  |
+----+--------+
3 rows in set (0.001 sec)

MariaDB [testdb]> exit;
Bye

```

