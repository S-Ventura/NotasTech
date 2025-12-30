# Docker – Variables de entorno

## Conceptos generales
Las variables de entorno permiten configurar un contenedor sin modificar la imagen. Se pasan al iniciar el contenedor y suelen incluir credenciales, puertos, modos de ejecución o flags de la app.

## Formas de definir variables
```bash
docker run -e VAR=valor <imagen>

docker run --env VAR=valor <imagen>

docker run --env-file .env <imagen>
```

## Riesgos y notas
- Evita exponer contraseñas en el historial de la terminal; usa `.env` o un gestor de secretos.
- Sin volumen, la data se pierde al borrar el contenedor.
- `:latest` puede cambiar; usa tags específicos.

## Alternativas
- `--env-file .env` para no escribir variables sensibles en el comando.
- Volúmenes nombrados para persistencia de datos.

## Resumen rapido
- Configura contenedores con `-e` o `--env-file`.
- Usa puertos y volúmenes para acceso y persistencia.
- Prefiere tags fijos para reproducibilidad.

## Ejemplo (Postgres)
```bash
docker pull postgres:15

docker run --name postgres-db   -e POSTGRES_PASSWORD=mi_password   -p 5432:5432   -v postgres-db:/var/lib/postgresql/data   -d postgres:15
```

## Referencias oficiales (Docker)
- https://docs.docker.com/engine/reference/commandline/run/
- https://docs.docker.com/engine/storage/volumes/
- https://hub.docker.com/_/postgres
