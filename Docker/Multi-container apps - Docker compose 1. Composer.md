# Introducción a Docker Compose
## Temas puntuales de la sección

Puntualmente veremos:

    docker-compose.yml
    Crear servicios
    Volúmenes
        Bind
        Name
        Externos
    Imágenes con tags
    Comandos de ejecución al montar una imagen
    Puertos
    Manejo de variables de entorno
    Nombres de servicios y servidores
    Dependencias de otros servicios

**Docker Compose:** Herramienta que permite definir y levantar múltiples contenedores con un solo archivo *(docker-compose.yml)* y un solo comando. Esto facilita el trabajo con aplicaciones complejas que requieren varios servicios como bases de datos, réplicas, herramientas de administración, redes y volúmenes.

**Objetivo:** Automatizar la creación del entorno de desarrollo, ahorrar tiempo, evitar errores manuales y facilitar la colaboración entre desarrolladores.
# 💪 Ejercicio 
## Conectando postgres y pgAdmin

### Creando contenedor postgres
```bash
docker container run \
-d \
--name postgres-db \
-e POSTGRES_PASSWORD=123456 \
-v postgres-db:/var/lib/postgresql/data \
postgres:15.1
```
### Creando contenedor pgAdmin
```bash
docker container run \
--name pgAdmin \
-e PGADMIN_DEFAULT_PASSWORD=123456 \
-e PGADMIN_DEFAULT_EMAIL=superman@google.com \
-dp 8080:80 \
dpage/pgadmin4:6.17
```
### Creando network postgres-net
```bash
luna@luna:~/Documentos$ docker network create postgres-net
ccfc016e939218558d37e78e6a893c12779e13504056fdb6df27556987b17deb
```
### Conectando pgAdmin a postgres-net
```bash
luna@luna:~/Documentos$ docker network connect postgres-net pgAdmin
```
### Conectando postgres a postgres-net
```bash
luna@luna:~/Documentos$ docker network connect postgres-net postgres-db
```