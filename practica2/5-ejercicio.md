## Esquema para el ejercicio

![Imagen](imagenes/esquema-ejercicio5.PNG)

### Crear la red

docker network create net-wp

### Crear el contenedor mysql a partir de la imagen mysql:8, configurar las variables de entorno necesarias

docker run -d --name mysql-container --network net-wp -e MYSQL_ROOT_PASSWORD=rootpassword -e MYSQL_DATABASE=wordpressdb mysql:8

### Crear el contenedor wordpress a partir de la imagen: wordpress, configurar las variables de entorno necesarias

docker run -d --name wordpress-container --network net-wp -e WORDPRESS_DB_HOST=mysql-container:3306 -e WORDPRESS_DB_NAME=wordpressdb -e WORDPRESS_DB_USER=root -e WORDPRESS_DB_PASSWORD=rootpassword -p 9300:80 wordpress

De acuerdo con el trabajo realizado, en la a el esquema de ejercicio el puerto a es 9300

Ingresar desde el navegador al wordpress y finalizar la configuración de instalación.

![image](https://github.com/xknuclesx/2024A-ISWD633-GR1/assets/120606471/91064d2a-d08d-4447-b8b9-c95071d4c921)


Desde el panel de admin: cambiar el tema y crear una nueva publicación.
Ingresar a: http://localhost:9300/ 
recordar que a es el puerto que usó para el mapeo con wordpress

![image](https://github.com/xknuclesx/2024A-ISWD633-GR1/assets/120606471/269a0d55-0423-4c6a-a3f6-958a272243e7)


### Eliminar el contenedor wordpress
docker stop wordpress-container
docker rm wordpress-container

### Crear nuevamente el contenedor wordpress
Ingresar a: http://localhost:9300/ 
recordar que a es el puerto que usó para el mapeo con wordpress

### ¿Qué ha sucedido, qué puede observar?

primero la eliminacion del contenedor no es posible porque se encuentra corriendo, y al eliminarlo, se cae wordpress

![image](https://github.com/xknuclesx/2024A-ISWD633-GR1/assets/120606471/f66eef02-0fbb-42ff-8120-3607e2c6b0a4)






