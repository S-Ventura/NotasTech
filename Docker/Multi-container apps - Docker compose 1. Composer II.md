```bash
luna@luna:~/Documentos/Docker/postgres-pgadmin$ docker compose up
WARN[0000] /home/luna/Documentos/Docker/postgres-pgadmin/docker-compose.yaml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion 
service "db" refers to undefined volume postgres-db: invalid compose project
```
```bash
luna@luna:~/Documentos/Docker/postgres-pgadmin$ docker compose up
service "db" refers to undefined volume postgres-db: invalid compose project
```
services:
  db:
    container_name: postgres_database
    image: postgres:15.1
    volumes:
      - postgres-db:/var/lib/postgres/data
    environment:
      - POSTGRES_PASSWORD=123456

  pgAdmin:
    depends_on:
      - db
    image: dpage/pgadmin4:6.17
    ports:
      - "8080:80"
    environment:
      - PGADMIN_DEFAULT_PASWORD=123456
      - PGADMIN_DEFAULT_EMAIL=superman@google.com


volumes:
  postgres-db: