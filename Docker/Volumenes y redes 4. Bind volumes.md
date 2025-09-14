# Bind Volume (nest-graphql)
```bash
docker container run \
--name nest-app \
-w /app \
-p 80:3000 \
-v "$(pwd)":/app \ ---> bind volume
node:16-alpine3.16 \
sh -c "yarn install && yarn start:dev"
```

docker container run \
--name nest-app \
-w /app \
-dp 80:3000 \    ------> Detached mode -- Background
-v "$(pwd)":/app \
node:18-alpine3.21 \
sh -c "yarn install && yarn start:dev"

```bash
 docker container logs -f nest-app
```

 # Terminal interactiva 

 ```bash
 docker exec -it nest-app /bin/sh
 ```

docker container run \
--name nest-app \
-w /app \
-dp 80:3000 \    ------> Detached mode -- Background
-v "$(pwd)":/app \
node:18-alpine3.21 \
sh -c "yarn install && yarn start:dev"
docker pull python:3.11.13-slim-trixie