## VOLUMEN NOMBRADO
Un volumen nombrado (named volume) es un tipo de volumen gestionado por Docker que se almacena en una ubicación específica del sistema de archivos del host y se identifica mediante un nombre único. Los volúmenes nombrados no requieren que especifiques una ruta del sistema de archivos del host, y en su lugar, Docker se encarga de la gestión y el almacenamiento del volumen.


### Crear volumen
```
docker volume create <nombre volumen>
```

### Crear el volumen nombrado: vol-postgres

docker volume create vol-postgres

## MOUNTPOINT
Un mountpoint se refiere al lugar en el sistema de archivos donde un dispositivo de almacenamiento se une (o monta) al sistema de archivos. Es el punto donde los archivos y directorios almacenados en ese dispositivo de almacenamiento son accesibles para el sistema operativo y las aplicaciones.

Por ejemplo, en Windows las unidades de almacenamiento (como `C:`, `D:`, etc.) actúan como puntos de montaje principales para discos duros, unidades flash, unidades ópticas y otros dispositivos de almacenamiento.

Cuando creas un volumen nombrado, Docker asigna un punto de montaje específico en el sistema de archivos del host para ese volumen.

### ¿Cuál es el Mountpoint de vol-postgres?

/var/lib/docker/volumes/vol-postgres/_data

### Estructura del Punto de Montaje:
- /var/lib/docker/volumes/: Es la ubicación base donde Docker almacena todos los volúmenes en el sistema de archivos del host.
- nombreVolumen/: Es el nombre del volumen nombrado que has creado. Docker crea un directorio con este nombre dentro de /var/lib/docker/volumes/ para almacenar los datos del volumen.
- _data: Es el subdirectorio dentro de vol-postgres/ donde se almacenan los datos reales del volumen. El nombre _data es una convención utilizada por Docker para indicar el directorio donde se encuentran los datos del volumen.

### ¿Cómo acceder a ese Mountpoint?
En el contexto de WSL (Windows Subsystem for Linux), wsl$ se refiere al nombre de un recurso compartido de red especial que representa la raíz del sistema de archivos de Windows desde WSL. Cuando accedes a \\wsl$ desde el Explorador de archivos de Windows, puedes ver y acceder a los archivos del sistema de archivos de la distribución de Linux en WSL.
\\wsl.localhost\docker-desktop-data\data\docker\volumes

### Crear un contenedor vinculado a un volumen nombrado
```
docker run -d --name <nombre contenedor> -v <nombre volumen>:<ruta contenedor> <nombre imagen>
```

### Crear la red net-drupal de tipo bridge

docker network create net-drupal

### Crear un servidor postgres vinculado a la red net-drupal, completar la ruta del contenedor
docker run -d --name server-postgres -e POSTGRES_DB=db_drupal -e POSTGRES_PASSWORD=12345 -e POSTGRES_USER=user_drupal -v vol-postgres:<ruta contenedor> --network net-drupal postgres
_No es necesario exponer el puerto, debido a que nos vamos a conectar desde la misma red de docker_

docker run -d --name server-postgres -e POSTGRES_DB=db_drupal -e POSTGRES_PASSWORD=12345 -e POSTGRES_USER=user_drupal -v vol-postgres:/var/lib/postgresql/data --network net-drupal postgres

### Crear un cliente postgres vinculado a la red drupal a partir de la imagen dpage/pgadmin4, completar el correo
docker run -d --name client-postgres --publish published=9500,target=80 -e PGADMIN_DEFAULT_PASSWORD=54321 -e PGADMIN_DEFAULT_EMAIL=<correo> --network net-drupal dpage/pgadmin4

docker run -d --name client-postgres --publish published=9500,target=80 -e PGADMIN_DEFAULT_PASSWORD=54321 -e PGADMIN_DEFAULT_EMAIL=your_email@example.com --network net-drupal dpage/pgadmin4

### Usar el cliente postgres para conectarse al servidor postgres, para la conexión usar el nombre del servidor en lugar de la dirección IP.

### Crear los volúmenes necesarios para drupal, esto se puede encontrar en la documentación

docker volume create drupal-html
docker volume create drupal-data
docker volume create drupal-logs
docker volume create drupal-modules

### Crear el contenedor server-drupal vinculado a la red, usar la imagen drupal, y vincularlo a los volúmenes nombrados
docker run -d --name server-drupal --publish published=9700,target=80 -v <nombre volumen>:<ruta contenedor> -v <nombre volumen>:<ruta contenedor> -v <nombre volumen>:<ruta contenedor> -v <nombre volumen>:<ruta contenedor> --network net-drupal drupal

docker run -d --name server-drupal --publish published=9700,target=80 -v drupal-html:/var/www/html -v drupal-data:/var/www/data -v drupal-logs:/var/www/logs -v drupal-modules:/var/www/modules --network net-drupal drupal

### Ingrese al server-drupal y siga el paso a paso para la instalación.

![image](https://github.com/xknuclesx/2024A-ISWD633-GR1/assets/120606471/99f76f67-43dc-448b-9802-6319c9b683f2)


_La instalación puede tomar varios minutos, mientras espera realice un diagrama de los contenedores que ha creado en este apartado._

![diagram](https://github.com/xknuclesx/2024A-ISWD633-GR1/assets/120606471/93579ad7-ad6c-4321-8542-e3889cbf2529)

### Eliminar un volumen específico
```
docker volume rm <nombre volumen>
```
**Considerar**
Datos Persistentes: Asegúrate de que el volumen no contiene datos críticos antes de eliminarlo, ya que esta operación no se puede deshacer.
Contenedores Activos: No puedes eliminar un volumen que está actualmente en uso por un contenedor activo. Debes detener y/o eliminar el contenedor primero.
