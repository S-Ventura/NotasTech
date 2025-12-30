# Docker – Logs del contenedor

## Conceptos generales
Docker captura la salida estándar y de error del proceso principal del contenedor. Esto permite inspeccionar logs sin entrar al contenedor.

## Comandos básicos
```bash
docker logs <contenedor>                 # muestra logs
docker logs -f <contenedor>              # sigue logs en tiempo real
docker logs --tail 100 <contenedor>      # últimas 100 líneas
docker logs --since 10m <contenedor>     # últimas 10 min
```

## Riesgos y notas
- Logs muy grandes pueden saturar la salida; usa `--tail` o `--since`.
- Si la app no escribe a stdout/stderr, los logs pueden estar vacíos.

## Alternativas
- `docker exec -it <contenedor> bash` para diagnóstico interactivo.
- Configura rotación de logs en el daemon si el volumen crece demasiado.

## Resumen rapido
- `docker logs` es la forma estándar de ver salida de contenedores.
- Filtra con `--tail` y `--since` para evitar ruido.

## Ejemplo (MariaDB)
```bash
docker run --name mariadb1   -e MYSQL_ROOT_PASSWORD=mi_password   -p 3306:3306   -d mariadb:10.6

docker logs -f mariadb1
```

## Referencias oficiales (Docker)
- https://docs.docker.com/engine/reference/commandline/logs/
- https://docs.docker.com/engine/reference/commandline/exec/
- https://docs.docker.com/engine/reference/commandline/run/
- https://hub.docker.com/_/mariadb
