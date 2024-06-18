#~ Imagen de Docker para Nginx con un archivo de configuración personalizado y un archivo index.html personalizado
FROM nginx:latest

#~ Copia el archivo de configuración personalizado a la ubicación de configuración de Nginx
COPY default.conf /etc/nginx/conf.d/default.conf

#~ Copia el archivo index.html al directorio raíz de Nginx
COPY index.html /usr/share/nginx/html/index.html
