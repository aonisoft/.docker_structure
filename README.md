# Plantilla de contenedores para usar en proyectos web

## Introducción

Esta plantilla está pensada para proyectos que utilicen el siguiente stack 
tecnológico:

* Apache
* Php-fpm
* MariaDB
* Myphpadmin
* Sonarqube


> [!NOTE]
> Está inspirado en la tecnología XAMPP o LAMPP.


Se considera que quien use esta estructura tiene conocimientos previos mínimos 
en:

+ Docker
+ Docker Compose
+ Podman
+ GNU/Linux
+ Terminal
+ Systemctl

## Instalando gestor de contenedores

Para instalar docker revisar la [guía](./docs/docker/installer.md)



## Instalación de docker

Revisar en la [guía de instalación](https://docs.docker.com/engine/install) 
según que distro se esta usando.

Luego activar el servicio de docker. Para distros con `systemd` como manager de
servicio realizar:

```bash

systemctl status docker.service

```
Si lanza el siguiente mensaje:

```bash

docker.service - Docker Application Container Engine
     Loaded: loaded (/usr/lib/systemd/system/docker.service; disabled; preset: disabled)
    Drop-In: /usr/lib/systemd/system/service.d
             └─10-timeout-abort.conf
     Active: inactive (dead)
TriggeredBy: ○ docker.socket
       Docs: https://docs.docker.com

```

Se puede activar de dos formas: 

1. Al iniciar el sistema
2. De forma manual antes de usar
   
>[!IMPORTANT]
> Si no usamos todo el tiempo el servicio conviene la última opción por 
> cuestiones de rendimiento del SO


### Iniciar junto con el sistema e iniciar servicio

```bash

sudo systemctl enable docker && \
sudo systemctl start docker

```

Comprobación del status

```bash

Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; preset: disabled)
    Drop-In: /usr/lib/systemd/system/service.d
             └─10-timeout-abort.conf
     Active: active (running) since ; 19s ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 21232 (dockerd)
      Tasks: 53
      Memory: 115.9M
        CPU: 1.664s
      CGroup: /system.slice/docker.service
              ├─21232 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
...

```

### Iniciar de forma manual antes de usar

Para iniciarlo:

```bash

sudo systemctl start docker

```

Para detenerlo:

```bash

sudo systemctl stop docker.socket docker.service

```


> [!TIP]
> Para evitar el uso de `sudo` como en `sudo docker ...` o 
> `sudo docker-compose ...` de forma  constante se puede agregar el **user** de 
> docker al **group** de nuestro **user** que utilizamos habitualmente por 
> ejemplo:


```bash

sudo usermod -aG docker $USER

```

Luego reiniciar la sección y verificar si ya pertenece al group de nuestro user.
La [guía oficial](https://docs.docker.com/engine/install/linux-postinstall) para
más información.



## Pasos para utilizar los servicios

### Primero

Clonar este repositorio dentro de la carpeta que tiene el proyecto web

Por https
```bash

git clone https://github.com/aonisoft/docker-structure-website.git .docker && \
cd .docker

```

Por ssh
```bash

git clone git@github.com:aonisoft/docker-structure-website.git .docker && \
cd .docker

```

#### 💡 Recomendación 

Al tener un frontend donde se tenga que utilizar cualquier tecnología sobre node
se puede realizar la siguiente estructura de carpetas:

```bash

"project's name"
├── .infra
├── api
└── frontend

```
> [!NOTE]
> El directorio api puede llamarse también backend o server. Y en lugar de 
> frontend puede ser cliente


En caso que el proyecto no utilize `node` sino herramientas como twin o blade, 
correspondientes a frameworks de `php`, se puede realizar la siguiente 
estructura:

```bash

"project's name"
├── .infra
└── project
    ├── config
    ├── src
    ├── tests
    └── other files...

```


### Segundo

Copiar el archivo .env-example y ajusta las variables de entorno a gusto

```bash

cp composefiles/.env-example composefiles/.env

```

Las variables que requieren ser revisadas y modificadas son:

```bash

DATABASE_NAME=

ROOT_PASSWORD=

PATH_PROJECT_VOLUME=

TIMEZONE=

URL_WEBSITE=

URL_WEBSITE_ALIAS=

URL_SONARQUBE=

```

> [!NOTE]
> Algunas variables nombradas tienen por defecto algunos valores. Como 
> `URL_PHP_ADMIN` o `URL_SONARQUBE` 


### Tercero

Configura el archivo hosts con las urls que tengas configuradas para que sean 
visibles dentro de tu sistema.

Este archivo esta en

```bash

/etc/hosts

```

Donde se tiene que colocar al final del todo una linea como la siguiente. Donde
las urls se colocan una después de la otra.


```vim

127.0.0.1 phpadmin.loc sonarqube.tests

```

## Levantar los servicios del servidor

Para docker revisar la [guía](./docs/docker/load_containers.md)

Para podman revisar la [guía](./docs/podman/load_containers.md)


## Ejecutar composer desde el contenedor

Utilizando docker  

```bash

docker exec -it php_fpm composer **command**

```
Utilizando podman

```bash

podman exec -it php_fpm composer **command**

```

## Ejecutar node para el frontend

Si se requiere de algún framework de javascript se debe simplemente configurar
las variables de entorno correspondientes:

```bash

PATH_PROJECT_FRONTEND_VOLUME=

URL_FRONTEND=

URL_FRONTEND_ALIAS=

```

> [!IMPORTANT]
> No olvidarse de actualizar el archivo `hosts` con las url para el front

Guía para creación e instalación del frontend usando [docker](./docs/docker/frontend_install.md)  

Guía para creación e instalación del frontend usando [podman](./docs/podman/frontend_install.md)  

> [!NOTE]
> Esto es a modo de ejemplo. Puede cambiarse lo referente a `vite` o `yarn` por
> lo que requieras usar. Si cambiaste el nombre del contenedor de `frontend` a
> algún otro nombre, en el archivo `compose_frontend.yml` deberías cambiarlo 
> aquí también.


# Sitios de inspiración

[Estructura base que utiliza](https://github.com/ger86/librarify-back-symfony6/tree/master/.docker)

[Dockerfile para php](https://github.com/CodelyTV/php-ddd-example/blob/main/Dockerfile)

[Solución a problemas con Node](https://www.youtube.com/watch?v=qdqN8v5iU14)


# Contribuciones

Cualquier recomendación de mejora o crítica constructiva es bienvenida.
