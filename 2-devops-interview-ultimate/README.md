# Proyecto Django y React.js Dockerizado

## Descripción

Este proyecto combina un backend en Django con un frontend en React.js, desplegados y orquestados mediante Docker Compose.

## Requisitos
Antes de comenzar, asegúrate de tener instalado:

- Docker y Docker Compose
- Cuenta de AWS con los permisos necesarios
- AWS CLI configurado en tu máquina

## Despliegue Local

1. **Clonar el Repositorio**

   Primero, clona el repositorio a tu máquina local.  
   `git clone <url_repositorio>`  
   `cd <nombre_repositorio>`  


2. **Construir y Ejecutar con Docker Compose**

Dentro del directorio del proyecto, ejecuta el siguiente comando para construir y levantar los contenedores:  
    `docker-compose up --build`  

Construirá las imágenes necesarias y levantará los contenedores para el backend, la base de datos y el frontend.

3. **Acceder a la Aplicación**

Una vez que los contenedores estén en funcionamiento, podrás acceder a la aplicación en:

- Frontend: `http://localhost:3000`
- Backend: `http://localhost:8000`

## Despliegue en AWS

Para desplegar en AWS, utilizaremos Amazon ECS (Elastic Container Service) y Amazon RDS (Relational Database Service) para la base de datos.

1. **Crear una Base de Datos en RDS**

- Ve a la consola de RDS y crea una nueva instancia de base de datos PostgreSQL.
- Asegúrate de anotar el nombre de la base de datos, el usuario y la contraseña, ya que los necesitarás más adelante.

2. **Crear un Repositorio en Amazon ECR**

Para cada imagen (backend, frontend), necesitarás un repositorio en Amazon ECR.    
    `aws ecr create-repository --repository-name <nombre-repositorio-backend>`  
    `aws ecr create-repository --repository-name <nombre-repositorio-frontend>`  

3. **Subir las Imágenes a ECR**

   Construye las imágenes Docker y súbelas a los repositorios ECR creados.

   `$(aws ecr get-login --no-include-email --region <tu-region>)`  
   `docker tag <imagen-backend>:latest <url-repositorio-ecr-backend>:latest`  
   `docker push <url-repositorio-ecr-backend>:latest`  
   `docker tag <imagen-frontend>:latest <url-repositorio-ecr-frontend>:latest`  
   `docker push <url-repositorio-ecr-frontend>:latest`  

4. **Crear un Cluster en ECS**

   - Ve a la consola de ECS y crea un nuevo cluster.
   - Elige el tipo de cluster "EC2 Linux + Networking".

5. **Crear Tareas y Servicios en ECS**

   - Para cada contenedor (backend, frontend), necesitarás definir una tarea en ECS con la imagen correspondiente de ECR.
   - Crea un servicio en ECS para cada tarea, asegurándote de configurar la red y los puertos correctamente.

6. **Actualizar el Archivo de Configuración del Backend**

   Asegúrate de actualizar el archivo `.env` del backend con los detalles de la base de datos RDS.

7. **Acceder a la Aplicación**

   Una vez que los servicios estén en funcionamiento, podrás acceder a tu aplicación utilizando la dirección IP pública de la instancia EC2 o el balanceador de carga, si configuraste uno.








