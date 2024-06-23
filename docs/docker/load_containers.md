# Levantar los servicios del servidor

Primero crear una red en docker llamada `external_network`, para que funcione 
el contenedor levantado.

```bash

docker network create external_network

```

La primera vez luego de clonarlo o cuando se tenga que reconstruir la imagen 
realizar

```bash

docker-compose -f composefiles/compose_webserver.yml build --no-cache

```

Una vez realizado estos pasos, se puede levantar todos los servicios php, 
apache, mariadb y phpmyadmin haciendo

```bash

docker-compose -f composefiles/compose_webserver.yml up -d

```

## Levantar el servicio del frontend


```bash

docker-compose -f composefiles/compose_sonarqube.yml build --no-cache

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
> Demora unos segundos en iniciar. Tener paciencia


## Bajar los servicios del servidor y sonarqube


```bash

docker-compose -f composefiles/compose_sonarqube.yml down

```

```bash

docker-compose -f composefiles/compose_webserver.yml down

```
