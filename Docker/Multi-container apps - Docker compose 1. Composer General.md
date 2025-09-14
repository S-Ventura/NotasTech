## 🔹 Forma general de un docker-compose.yaml
```bash
version: "3.9"  # Versión de formato de docker-compose

services:       # Aquí defines los contenedores que formarán tu aplicación
  nombre_servicio:
    image: imagen:tag       # Puedes usar una imagen ya existente en DockerHub
    build: ./ruta           # O bien construir desde un Dockerfile
    container_name: nombre  # Nombre opcional del contenedor
    ports:                  # Mapeo de puertos (host:contenedor)
      - "8080:80"
    volumes:                # Volúmenes (persistencia o compartir archivos)
      - ./data:/app/data
    environment:            # Variables de entorno
      - VAR=valor
    depends_on:             # Dependencias de servicios
      - otro_servicio

volumes:        # Definición de volúmenes (si quieres persistencia global)
  nombre_volumen:
    driver: local

networks:       # Definición de redes (si necesitas aislar servicios)
  nombre_red:
    driver: bridge

```
## 🔹 Ejemplo útil para un proyecto de IA / Ciencia de Datos

Imaginemos que quieres un stack sencillo con:
**JupyterLab** para trabajar notebooks.
**PostgreSQL** como base de datos para tus datasets de México.
**pgAdmin** para administrar la BD gráficamente.

```bash 
version: "3.9"

services:
  jupyter:
    image: jupyter/scipy-notebook:latest  # Incluye pandas, numpy, matplotlib, scikit-learn, etc.
    container_name: jupyterlab
    ports:
      - "8888:8888"
    volumes:
      - ./notebooks:/home/jovyan/work  # Carpeta local vinculada al contenedor
    environment:
      - JUPYTER_ENABLE_LAB=yes
    networks:
      - datanet

  db:
    image: postgres:14
    container_name: postgres_db
    restart: always
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: celestial
    ports:
      - "5432:5432"
    volumes:
      - db_data:/var/lib/postgresql/data
    networks:
      - datanet

  pgadmin:
    image: dpage/pgadmin4
    container_name: pgadmin
    environment:
      PGADMIN_DEFAULT_EMAIL: admin@celestial.io
      PGADMIN_DEFAULT_PASSWORD: admin
    ports:
      - "5050:80"
    depends_on:
      - db
    networks:
      - datanet

volumes:
  db_data:

networks:
  datanet:
    driver: bridge
```

