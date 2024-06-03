### Crear contenedor de Postgres sin que exponga los puertos. Usar la imagen: postgres:11.21-alpine3.17

docker pull postgres:11.21-alpine3.17

docker run -d --name postgres --network none -e POSTGRES_PASSWORD=admin postgres:11.21-alpine3.17

### Crear un cliente de postgres. Usar la imagen: dpage/pgadmin4

docker pull dpage/pgadmin4

docker run -d --name pgadmin -e 'PGADMIN_DEFAULT_EMAIL=admin@gmail.com' -e 'PGADMIN_DEFAULT_PASSWORD=admin' -p 80:80 dpage/pgadmin4

La figura presenta el esquema creado en donde los puertos son:
- a: 80
- b: 80
- c: 5432

![Imagen](imagenes/esquema-ejercicio3.PNG)

## Desde el cliente
### Acceder desde el cliente al servidor postgres creado.
![image](https://github.com/xknuclesx/2024A-ISWD633-GR1/assets/120606471/e842773c-040c-43c1-ad67-f395bc3f4b97)
### Crear la base de datos info, y dentro de esa base la tabla personas, con id (serial) y nombre (varchar), agregar un par de registros en la tabla, obligatorio incluir su nombre.

## Desde el servidor postgresl
### Acceder al servidor
### Conectarse a la base de datos info
docker exec -it postgres psql -U postgres
\c INFO;
SELECT * FROM personas;
### Realizar un select *from personas
![image](https://github.com/xknuclesx/2024A-ISWD633-GR1/assets/120606471/2a8722ab-7ec3-4a60-99b6-9e145affa09d)
