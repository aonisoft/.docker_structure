# Plantilla de Docker para usar en proyectos web

## Introducción

Esta plantilla está pensada para proyectos que utilicen el siguiente stack 
tecnológico:

* Apache
* Php-fpm
* MariaDB
* Myphpadmin


> [!NOTE]
> Está inspirado en la tecnología XAMPP o LAMPP.


Se considera que quien use esta estructura tiene conocimientos previos mínimos 
en:

+ Docker
+ Docker Compose
+ GNU/Linux
+ Terminal
+ Systemctl



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

```bash

git clone git@github.com:aonisoft/docker-structure-website.git .docker && \
cd .docker

```
### Segundo

Copiar el archivo .env-example y ajusta las variables de entorno a gusto

```bash

cp .env-example .env

```



## Levantar los servicios del servidor


Ejecutar el servicio de docker se debe realizar:


> [!IMPORTANT]
> Realizar el `build` la primera vez luego de clonarlo o cuando se tenga que 
> reconstruir la imagen


```bash

docker-compose -f composefiles/compose_webserver.yml build --no-cache

```


El que levanta todos los servicios php, apache, mariadb y phpmyadmin es:

```bash

docker-compose -f composefiles/compose_webserver.yml up -d

```




## Levantar los servicios de sonarqube

Los servicios para ejecutar sonarqube requiere que estén levantados los 
servicios del servidor para poder utilizarlo.

> [!IMPORTANT]
> Realizar el `build` la primera vez o cuando se tenga que reconstruir la imagen


```bash

docker-compose -f composefiles/compose_sonarqube.yml build --no-cache

```


```bash

docker-compose -f composefiles/compose_sonarqube.yml up -d

```

> [!IMPORTANT]
> Demora unos segundos en iniciar


## Bajar los servicios del servidor y sonarqube


```bash

docker-compose -f composefiles/compose_sonarqube.yml down

```

```bash

docker-compose -f composefiles/compose_webserver.yml down

```

## Ejecutar composer desde el contenedor


```bash

docker exec -it php_fpm composer **command**

```
# Sitios de inspiración

[Estructura base que utilizada](https://github.com/ger86/librarify-back-symfony6/tree/master/.docker)

[Dockerfile para php](https://github.com/CodelyTV/php-ddd-example/blob/main/Dockerfile)


# Contribuciones

Cualquier recomendación de mejora o crítica constructiva es bienvenida.
