## 🐳 `Ejercicio con Postgres`
# Imagen de Postgres
```bash
luna@luna:~/Documentos/NotasTech$ docker pull postgres
Using default tag: latest
latest: Pulling from library/postgres
ae28e2b99a62: Pull complete 
f7f2afaa1b41: Pull complete 
396b1da7636e: Pull complete 
be9fdbdba096: Pull complete 
085f0a899c07: Pull complete 
7fa725c973af: Pull complete 
36b4e7f51364: Pull complete 
901a9540064a: Pull complete 
c166c949e1c3: Pull complete 
f5465e2fc020: Pull complete 
5d91a345d79a: Pull complete 
85558a023eea: Pull complete 
b7a79609094c: Pull complete 
1f6dfcaad4e9: Pull complete 
Digest: sha256:19ad070ea172efd48d7cae52297caaf845a3625728674bbc1a6efb679ab7befe
Status: Downloaded newer image for postgres:latest
docker.io/library/postgres:latest
luna@luna:~/Documentos/NotasTech$ docker image ls -a
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
postgres     latest    19ad070ea172   34 hours ago   641MB
luna@luna:~/Documentos/NotasTech$ docker container run --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -d postgres
ecd227f9ebffe827a319a2f5f0b2e9386d5b2fe2e5175a23a361cebd3eb37671
```

# Variables de entorno
https://hub.docker.com/_/postgres#environment-variables

Luego se modifica el puerto de conexión de mi host con el default de postgres

```bash
luna@luna:~/Documentos/NotasTech$ docker container ls -a
CONTAINER ID   IMAGE      COMMAND                  CREATED          STATUS          PORTS      NAMES
ecd227f9ebff   postgres   "docker-entrypoint.s…"   28 seconds ago   Up 28 seconds   5432/tcp   some-postgres
luna@luna:~/Documentos/NotasTech$ docker container rm -f ecd
ecd
luna@luna:~/Documentos/NotasTech$ docker container ls -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
luna@luna:~/Documentos/NotasTech$ docker container run --name some-postgres -dp 5432:5432 -e POSTGRES_PASSWORD=mysecretpassword postgres

316bdded7f3ef2f404fe69645a10d29d8430d85217fdde0793d88b0049fb7f37
```
## Multiples instancias de Postgres

```bash
luna@luna:~/Documentos/NotasTech$ docker container run \
> --name postgres-alpha \
> -e POSTGRES_PASWORD=mypass1 \
> -dp 5432:5432 \
> postgres

f380e6df17250cf2a17af518e97d9b7209df50e436b6ccbde49901edd19d6cbc
```

Asignamos un segundo contenedor al puerto 5433
```bash 
luna@luna:~/Documentos/NotasTech$ docker container run \
> --name postgres-beta -e POSTGRES_PASSWORD=mypass1 -dp 5433:5433 postgres:14-alpine3.17
6c97d23d17c1ecaa81961950934c0ae9397e793595cbcec90a494f0ae42fad97

luna@luna:~/Documentos/NotasTech$ docker container ls -a
CONTAINER ID   IMAGE                    COMMAND                  CREATED          STATUS          PORTS                                         NAMES
6c97d23d17c1   postgres:14-alpine3.17   "docker-entrypoint.s…"   34 seconds ago   Up 34 seconds   0.0.0.0:5433->5433/tcp, [::]:5433->5433/tcp   postgres-beta
3ebd3cc59dde   postgres                 "docker-entrypoint.s…"   7 minutes ago    Up 7 minutes    0.0.0.0:5432->5432/tcp, [::]:5432->5432/tcp   postgres-alpha

´´´