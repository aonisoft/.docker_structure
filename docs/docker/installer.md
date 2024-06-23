# Utilizando docker

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


### Iniciar el servicio desde el arranque del sistema 

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

Luego reiniciar la sección y verificar si ya pertenece al **group** de nuestro 
**user**.
La [guía oficial](https://docs.docker.com/engine/install/linux-postinstall) para
más información.

