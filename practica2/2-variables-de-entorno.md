# Variables de Entorno
### ¿Qué son las variables de entorno?
Las variables de entorno son parámetros que se pueden pasar a un contenedor en el momento de su creación para configurar su comportamiento.
### Para crear un contenedor con variables de entorno

```
docker run -d --name <nombre contenedor> -e <nombre variable1>=<valor1> -e <nombre variable2>=<valor2>
```

### Crear un contenedor a partir de la imagen de nginx:alpine con las siguientes variables de entorno: username y role. Para la variable de entorno rol asignar el valor admin.

 docker run -d --name nginx -e username=admin -e role=admin nginx:alpine

![image](https://github.com/xknuclesx/2024A-ISWD633-GR1/assets/120606471/60ffabc5-2f75-432c-bebc-f5ba8a60ab44)
![image](https://github.com/xknuclesx/2024A-ISWD633-GR1/assets/120606471/0d36b50d-a635-44d1-9fcd-50e5a2ae661d)


### Crear un contenedor con mysql:8 , mapear todos los puertos

 docker pull mysql:8
 docker run -P -d --name mysql mysql:8

### ¿El contenedor se está ejecutando?
No se encuentra ejecutando, dado que está 
![image](https://github.com/xknuclesx/2024A-ISWD633-GR1/assets/120606471/9dc6431a-471f-4d41-905e-406b4bff163d)


### Identificar el problema
El problema es que no contiene las variables de entorno necesarias para inicializar el contenedor.
![image](https://github.com/xknuclesx/2024A-ISWD633-GR1/assets/120606471/0298f336-9615-4731-8fb9-07646e11b612)

### Eliminar el contenedor creado con mysql:8 
docker rm mysql

### Para crear un contenedor con variables de entorno especificadas
- Portabilidad: Las aplicaciones se vuelven más portátiles y pueden ser desplegadas en diferentes entornos (desarrollo, pruebas, producción) simplemente cambiando el archivo de variables de entorno.
- Centralización: Todas las configuraciones importantes se centralizan en un solo lugar, lo que facilita la gestión y auditoría de las configuraciones.
- Consistencia: Asegura que todos los miembros del equipo de desarrollo o los entornos de despliegue utilicen las mismas configuraciones.
- Evitar Exposición en el Código: Mantener variables sensibles como contraseñas, claves API, y tokens fuera del código fuente reduce el riesgo de exposición accidental a través del control de versiones.
- Control de Acceso: Los archivos de variables de entorno pueden ser gestionados con permisos específicos, limitando quién puede ver o modificar la configuración sensible.

Previo a esto es necesario crear el archivo y colocar las variables en un archivo, **.env** se ha convertido en una convención estándar, pero también es posible usar cualquier extensión como **.txt**.
```
docker run -d --name <nombre contenedor> --env-file=<nombreArchivo>.<extensión> <nombre imagen>
```
**Considerar**
Es necesario especificar la ruta absoluta del archivo si este se encuentra en una ubicación diferente a la que estás ejecutando el comando docker run.

### Crear un contenedor con mysql:8 , mapear todos los puertos y configurar las variables de entorno mediante un archivo
 docker run -d --name mysql --env-file=C:\Users\adria\OneDrive\Escritorio\mysecuele.env -P mysql:8

![image](https://github.com/xknuclesx/2024A-ISWD633-GR1/assets/120606471/2923357d-4436-4d2e-b506-b5d847100f84)

### ¿Qué bases de datos existen en el contenedor creado?
information_schema 

mysql             

performance_schema

sys

![image](https://github.com/xknuclesx/2024A-ISWD633-GR1/assets/120606471/1e15df3b-e2a0-4d78-a245-3d8ac987207b)
