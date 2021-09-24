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


## Para crear un link simbólico a la carpeta de un plugin en desarrollo hay que crear un volumen a dicha carpeta y desde dentro del contenedor crear el symlink, ya que docker no reconoce los enlaces simbólicos creados desde el host


## Debug
Si está activo el modo debug esta función sirve para volcar variables al archivo wp-content/debug.log (guardarla en functions.php o en code snippets)


```
function var_to_file($data){
    ob_start();
    var_dump($data);
    $contents = ob_get_contents();
    ob_end_clean();
    error_log($contents);	
}
```

## Instalar la librería int (para tener la función NumberFormatter entre otras) ejecutar en el contenedor:
```
docker exec -it {CONTAINER_ID} /bin/bash
apt update && apt-get install libicu-dev -y && docker-php-ext-install mysqli pdo pdo_mysql && docker-php-ext-configure intl && docker-php-ext-install intl

docker-compose restart
```


