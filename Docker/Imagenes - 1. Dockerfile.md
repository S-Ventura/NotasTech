# 📘 Notas de la Sección 5 – Construcción de imágenes con Dockerfile

    **Dockerfile** = conjunto de instrucciones para construir imágenes.
    * Cada instrucción crea una capa (ej: crear directorio, exponer puerto, copiar archivos, instalar dependencias, ejecutar comandos).
    * **Resultado:** una imag*n lista para ejecutar la aplicación sin instalaciones extra.
    * **Objetivo:** empaquetar el código fuente en una imagen que contenga todo lo necesario. 
    * Se puede ejecutar con docker run imagen:tag.
    Posibilidad de correr versiones anteriores fácilmente usando tags.
    - **Tamaño de las imágenes:** Inicialmente pueden ser muy grandes (500 MB – 2 GB). Se verán buenas prácticas para reducirlas (ej: imágenes base más ligeras).

    * **Buenas prácticas al construir imágenes:**
    Incluir unit tests en el proceso de build.

    * Si los tests fallan → no se construye la imagen.
    * Si pasan → se genera la imagen final.

    * **Compatibilidad entre arquitecturas:** Existen diferentes arquitecturas (x86, ARM64, etc.).
    * Se aprenderá a crear imágenes multi-arquitectura con Buildx.
    * **Metodología de aprendizaje:** Paso a paso, comprendiendo cada instrucción del Dockerfile.
    * **Luego:** imágenes multi-stage (etapas), más optimizadas y profesionales.

# 📘 Temas puntuales de la sección

Esta sección es sumamente importante para la comprensión y creación de imágenes personalizadas.

    Aquí veremos:
        Dockerfile
        .dockerignore

    Principales comandos del Dockerfile
        FROM
        RUN
        COPY
        WORKDIR
        Entre otros

    Asignar tags
    Build
    Buildx (Para múltiples arquitecturas)

Es una sección indispensable para poder crear de forma eficiente nuestras futuras imágenes que desplegaremos en servidores con arquitecturas diferentes a la nuestra computadora.

# Ejercicio: Creando dockerfile para app Cron-Ticker
📘 Notas – Creación de imágenes en Docker (introducción con aplicación Node.js)

Concepto clave:

Localizar / Realizar una aplicación = empaquetar el código fuente en una imagen lista para correr en un contenedor.

Para esto se usa un Dockerfile, que es un blueprint con instrucciones (copiar archivos, exponer puertos, definir variables, entrypoints, etc.).

Cada instrucción genera un layer (capa) → capas pueden reutilizarse entre imágenes, lo que ahorra recursos.

Dificultad inicial:

Los Dockerfiles al inicio parecen confusos.

Objetivo: comenzar desde ejemplos simples e ir aumentando complejidad.

Más adelante → imágenes multi-stage y multi-arquitectura.

Ejemplo práctico de la sección:

Se crea un proyecto Node.js sencillo.

Pasos:

Crear carpeta cron-ticker y abrir en VS Code.

Inicializar proyecto con npm init.

Crear app.js con un Hola mundo y script start en package.json.

Ejecutar con npm start is a command used in Node.js projects to initiate the application.

Extensión de la aplicación:

Instalar librería node-cron (npm install node-cron).

Configurar un cron job que ejecute código en intervalos de tiempo.

Ejemplo: imprimir un mensaje en el segundo 5, 10, 15, 20, etc.

Se agrega un contador de ticks para mostrar cuántas veces se ejecutó.

Cancelar ejecución con Ctrl+C.

Idea práctica del ejercicio:

Simular tareas programadas (como en Linux con cron).

Útil para procesos automáticos: sincronizaciones, ejecuciones periódicas, etc.

# Dockerfile
Docker permite ejecutar aplicaciones con todas sus dependencias sin instalar nada en el servidor.

Se construye una imagen usando un Dockerfile, que contiene instrucciones paso a paso.

Cada instrucción (FROM, COPY, RUN, etc.) crea una capa reutilizable.

Se elige una imagen base (ej. Node + Alpine) y se define un directorio de trabajo (WORKDIR).

Se copian los archivos de la aplicación ( Por ejemplo: app.js, package.json) y luego se construye la imagen lista para ejecutar.

# Construcción de la imagen Docker
Se copian los archivos de la aplicación (app.js y package.json) al directorio de trabajo (WORKDIR /app).

Se ejecuta RUN npm install para instalar todas las dependencias de Node y generar los módulos necesarios.

Se define el comando de ejecución de la aplicación con CMD, que indica qué hacer al iniciar el contenedor (node app.js o npm start).

Se construye la imagen con docker 
```bash
docker build -t nombre_imagen .
```
El punto indica la ubicación del Dockerfile.

Cada comando crea capas, lo que permite que futuras construcciones sean más rápidas si no hay cambios en los pasos anteriores.

La imagen se puede ejecutar con 
```bash
docker container run nombre_imagen
```
, iniciando automáticamente la aplicación dentro del contenedor.

Se recomienda usar buenas prácticas: colocar primero los comandos que cambian menos para aprovechar la caché y acelerar la construcción.

Resultado: se obtiene una imagen completa con Linux, Node, dependencias y el código listo para ejecutarse en cualquier servidor.



# Reconstrucción y actualización de imágenes Docker

1. Motivos para reconstruir imágenes:

Corrección de errores en el Dockerfile.

Actualizaciones en el código o cambios en la aplicación.

2. Reconstrucción usando el mismo tag:


docker build -t cron_ticker .

Docker reutiliza las capas en caché si no hay cambios, acelerando el proceso.

Solo se reconstruyen las capas afectadas por los cambios (por ejemplo, si solo se modificó app.js, las capas anteriores permanecen en caché).

1. Revisión de imágenes:
docker image ls

Muestra todas las imágenes, incluyendo versiones anteriores y latest.

Buenas prácticas: usar nomenclatura y tags para identificar versiones y evitar confusión.

4. Asignar o renombrar tags a una imagen existente:
   
   docker image tag cron_ticker:latest cron_ticker:1.0.0
docker image tag cron_ticker:latest cron_ticker:buffalo
Esto permite mantener múltiples versiones de la misma imagen sin reconstruirla.

Ejemplo de nombres de versiones: 1.0.0, Buffalo, Castor, etc.

Construcción tras cambios en el código:
Esto permite mantener múltiples versiones de la misma imagen sin reconstruirla.

Ejemplo de nombres de versiones: 1.0.0, Buffalo, Castor, etc.

Construcción tras cambios en el código:

docker build -t cron_ticker:latest .
docker image ls
Genera una nueva capa (latest) sin afectar las versiones anteriores (1.0.0, Buffalo).

Docker solo reconstruye las capas que tuvieron cambios.

6. Ejecutar contenedores con versiones específicas:
docker container run cron_ticker:castor
docker container run cron_ticker:1.0.0
docker container run cron_ticker:buffalo

Permite probar distintas versiones de la aplicación según el tag.

7. Manejo de contenedores activos:
docker container ls
docker container rm -f <container_id>
Cancelar contenedores en ejecución si quedan pegados al cerrar.

Resumen práctico:

Cada cambio en el código o en Dockerfile puede requerir reconstrucción.

Docker usa caché de capas para acelerar reconstrucciones.

Mantener un sistema de tags claro permite gestionar versiones y mantener un historial organizado.

# Subir imágenes a Docker Hub

Preparar la cuenta y el repositorio: 
- Crear una cuenta en Docker Hub y un repositorio con el nombre de tu aplicación, por ejemplo cron_ticker.

Renombrar la imagen local con el nombre del repositorio: usar docker image tag para asignar el nombre de usuario y el tag correspondiente. 
```bash
docker image tag cron_ticker:latest usuario_docker/cron_ticker:latest y docker image tag cron_ticker:castor usuario_docker/cron_ticker:castor. 
```

Autenticarse en Docker Hub: usar docker login e ingresar usuario y contraseña o token de acceso.

Subir la imagen al repositorio: usar docker push usuario_docker/cron_ticker:latest y docker push usuario_docker/cron_ticker:castor. Docker sube solo las capas nuevas y crea referencias para las existentes.

Verificar que la imagen está subida: con docker image ls desde la terminal o recargando la página del repositorio en Docker Hub para ver los tags.

Notas importantes: se pueden subir múltiples tags de la misma imagen para manejar versiones. Docker Hub permite escaneo automático de vulnerabilidades. Tener un sistema de nombres claro evita perder referencias a versiones anteriores. En Macs con procesadores M1 (ARM) la imagen puede diferir en tamaño o compatibilidad con arquitecturas x86_64.

# Resumen 

## Ver imágenes locales
docker image ls

# Construir una imagen desde Dockerfile
docker build -t <nombre_imagen>:<tag> .

# Ejecutar un contenedor desde la imagen
docker container run <nombre_imagen>:<tag>

# Listar contenedores activos
docker container ls

# Detener y eliminar contenedor
docker container stop <ID_contenedor>
docker container rm <ID_contenedor>

# Instalar dependencias dentro de Dockerfile
RUN npm install

# Definir directorio de trabajo en Dockerfile
WORKDIR /app

# Copiar archivos al contenedor
COPY <origen> <destino>

# Definir comando por defecto al iniciar el contenedor
CMD ["node", "api.js"]

# Renombrar o agregar tag a imagen local
docker image tag <imagen_actual>:<tag> <usuario>/<repo>:<nuevo_tag>

# Autenticarse en Docker Hub
docker login

# Subir imagen a Docker Hub
docker push <usuario>/<repo>:<tag>

## Riesgos y notas
- Dockerfiles grandes generan imagenes pesadas; optimiza capas y cache.
- No incluyas secretos en capas de build.
- Ordena instrucciones para aprovechar la cache.

## Alternativas
- Usa multi-stage builds para reducir tamano.
- Agrega `.dockerignore` para excluir archivos innecesarios.
- Buildx para multi-arquitectura.

## Resumen rapido
- Un Dockerfile define capas reproducibles para construir imagenes.
- Construye con `docker build`, etiqueta con `docker image tag` y publica con `docker push`.
- Usa buenas practicas para imagenes pequenas y seguras.

## Referencias oficiales (Docker)
- https://docs.docker.com/engine/reference/builder/
- https://docs.docker.com/build/
- https://docs.docker.com/build/building/multi-stage/
- https://docs.docker.com/engine/reference/commandline/build/
- https://docs.docker.com/engine/reference/commandline/buildx/
- https://docs.docker.com/engine/reference/commandline/image_tag/
- https://docs.docker.com/engine/reference/commandline/push/
- https://docs.docker.com/engine/reference/commandline/login/
- https://docs.docker.com/build/building/ignore/
