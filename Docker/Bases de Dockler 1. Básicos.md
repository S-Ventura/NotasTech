# Docker – Comandos básicos

## Conceptos generales
- **Imagen**: plantilla inmutable con dependencias y runtime.
- **Contenedor**: instancia en ejecución de una imagen.
- **Registry**: repositorio de imágenes (Docker Hub por defecto).

## Comandos esenciales
```bash
docker pull <imagen>:<tag>              # descarga una imagen
docker run <imagen>:<tag>               # ejecuta un contenedor
docker ps                               # lista contenedores activos
docker ps -a                            # lista todos (activos y detenidos)
docker stop <contenedor>                # detiene un contenedor
docker rm <contenedor>                  # elimina un contenedor
docker images                           # lista imágenes locales
docker image prune                      # elimina imágenes no usadas
```

## Publicar puertos y modo detached
```bash
docker run -d -p 8080:80 <imagen>:<tag>  # 8080 host -> 80 contenedor
```

## Riesgos y notas
- `:latest` puede cambiar sin aviso; usa tags fijos en entornos estables.
- `docker rm` y `docker image prune` eliminan recursos; revisa antes de borrar.
- Publicar puertos expone servicios en tu host; evita puertos sensibles.

## Alternativas
- `docker ps` es equivalente a `docker container ls`.
- `docker run --rm` borra el contenedor al finalizar (útil para pruebas).

## Resumen rapido
- Descarga imágenes con `docker pull` y ejecuta con `docker run`.
- Lista y limpia contenedores/imágenes para ahorrar espacio.
- Usa tags y puertos de forma explícita.

## Ejemplo
```bash
docker pull hello-world
docker run --rm hello-world

docker run -d -p 8080:80 docker/getting-started
```

## Referencias oficiales (Docker)
- https://docs.docker.com/engine/reference/commandline/docker/
- https://docs.docker.com/engine/reference/commandline/pull/
- https://docs.docker.com/engine/reference/commandline/run/
- https://docs.docker.com/engine/reference/commandline/container_ls/
- https://docs.docker.com/engine/reference/commandline/image/
