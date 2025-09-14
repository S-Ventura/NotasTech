##`Volumenes

Hay 3 tipos de volúmenes, son usados para hacer persistente la data entre reinicios y levantamientos de imágenes.
## Named Volumes

Crear un nuevo volumen
```bash
docker volume create todo-db
```
Listar los volúmenes creados
```bash
docker volume ls
```
Inspeccionar el volumen específico
```bash
docker volume inspect todo-db
```
Remueve todos los volúmenes no usados
```bash 
docker volume prune 
```
Remueve uno o más volúmenes especicados
```bash 
docker volume rm VOLUME_NAME 
```
Usar un volumen al correr un contenedor
```bash 
docker run -v todo-db:/etc/todos getting-started 
```

## Bind Volumes - Vincular volúmenes

**Bind volumes trabaja con paths absolutos**
Terminal
```bash
docker run -dp 3000:3000 \
-w /app -v "$(pwd):/app" \
node:18-alpine \
sh -c "yarn install && yarn run dev"
```
-w /app: **Working directory:** donde el comando
empezará a correr.
-v “$(pwd):/app”: **Volumen vinculado:** vinculamos el
directorio del host con el directorio /app del contenedor
node:18-alpine: **Imagen a usar**
sh -c "yarn install && yarn run dev": **Comando Shell:**
Iniciamos un shell y ejecutamos *yarn install* y luego correr el **yarn run dev**

## Anonymous Volumes: 

**Volúmenes donde sólo se especifica el path del contenedor y Docker lo asigna automáticamente en el host**

```bash
docker run -v /var/lib/mysql/data
```

[https://hub.docker.com/_/mariadb]()

```bash
docker container run \
-dp 3306:3306 \
--name world-db \
--env MARIADB_USER=example-user \
--env MARIADB_PASSWORD=user-password \
--env MARIADB_ROOT_PASSWORD=root-secret-password \
--env MARIADB_DATABASE=world-db \
--volume world-db:/var/lib/mysql \
mariadb:jammy
```
### PHPMYADMIN
[https://hub.docker.com/_/phpmyadmin]
```bash
docker container run \
--name phpmyadmin \
-d  \
-e PMA_ARBITRARY=1 \
-P phpmyadmin:5.2.0-apache
```
![[http://localhost:55000/](http://localhost:55000/)](image-3.png)