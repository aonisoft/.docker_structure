# Instalación del frontend con docker

Crear el proyecto usando una plantilla correspondiente a algún framework.

```bash

docker-compose -f ./composefiles/compose_frontend.yml run \
--rm --user 1000:1000 frontend \
yarn create vite . --template react-ts yarn

```
> [!IMPORTANT]
> El flag `user` es para que al crear los archivos, no lo hagan como usuario 
> `root`

Luego para instalar los paquetes ejecutar

```bash

docker-compose -f ./composefiles/compose_frontend.yml run \
--rm --user 1000:1000 frontend yarn install

```