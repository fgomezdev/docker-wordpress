## Archivo docker-compose.yml (Wordpress - Apache - Mysql)

Posibles soluciones en caso de tener problema de permisos para subir o actualizar plugins:

Entrar a la consola del contenedor de wordpress:

`docker exec -u root -it {CONTAINER_ID} /bin/bash`

(o si el directorio está en un volumen hacerlo directo desde consola)

Cambiar el propietario de los directorios wp-admin, wp-content, wp-includes

`chown -R www-data:www-data wp-content`

Dar permisos al totales propietario:

`chmod -R 755 wp-content`

O en caso de ser necesario permisos al grupo:

`chmod -R 775 wp-content`


### Para aumentar el limite de subida de archivos:
Si no existe, crear el archivo php.ini en /public y agregarle el siguiente contenido:

`upload_max_filesize = 1024M`

`post_max_size = 1024M`

`memory_limit = 15M`

