# Temas puntuales de la sección

- Terminal interactiva dentro del contenedor
- Aplicaciones con múltiples contenedores
- Redes
- Volúmenes
- Mapeo de directorios y relaciones
- Montar un servidor Apache con PHPMyAdmin junto a MariaDB
- Revisar el file system de alpine y node
- 
Esta sección empieza a dejar bases para el uso de los contenedores a otro nivel.

# Tarea sin volumenes


1. Montar la imagen de MariaDB con el tag jammy, publicar en el puerto 3306 del contenedor con el puerto 3306 de nuestro equipo, colocarle el nombre al contenedor de __world-db__ (--name world-db) y definir las siguientes variables de entorno:
    * MARIADB_USER=example-user
    * MARIADB_PASSWORD=user-password
    * MARIADB_ROOT_PASSWORD=root-secret-password
    * MARIADB_DATABASE=world-db

2. Conectarse usando Table Plus a la base de datos con las credenciales del usuario (NO EL ROOT)
3. Conectarse a la base de datos ```world-db```
4. Ejecutar el query de creación de tablas e inserción proporcionado
5. Revisar que efectivamente tengamos la data

```bash
luna@luna:~/Documentos/NotasTech$ docker container run \
> -dp 3306:3306 \
> --name world-db \
> --env MARIADB_USER=example-user \
> --env MARIADB_PASSWORD=user-password \
> --env MARIADB_ROOT_PASSWORD=root-secret-password \
> --env MARIADB_DATABASE=world-db \
> mariadb:jammy


Unable to find image 'mariadb:jammy' locally
jammy: Pulling from library/mariadb
a8b1c5f80c2d: Pull complete 
b13b8cff7564: Pull complete 
e5739d28aeee: Pull complete 
0b3f8ae1fce9: Pull complete 
61d4eb1159ff: Pull complete 
a0b237c7c6ae: Pull complete 
a6321fa47c19: Pull complete 
12077f74b1db: Pull complete 
Digest: sha256:e101f9db31916a5d4d7d594dd0dd092fb23ab4f499f1d7a7425d1afd4162c4bc
Status: Downloaded newer image for mariadb:jammy
5f78385472120644a05933024e274b548a4bf1131fc98f1e9d4fb74f899ee7f7
```