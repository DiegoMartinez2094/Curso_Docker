En la terminal de nuestra maquina después de instalar docker podemos:

con el comando:

```
docker version

```

visualizamos la informacion:

```
Client:
 Cloud integration: v1.0.35-desktop+001
 Version:           24.0.5
 API version:       1.43
 Go version:        go1.20.6
 Git commit:        ced0996
 Built:             Fri Jul 21 20:36:24 2023
 OS/Arch:           windows/amd64
 Context:           default

Server: Docker Desktop 4.22.1 (118664)
 Engine:
  Version:          24.0.5
  API version:      1.43 (minimum version 1.12)
  Go version:       go1.20.6
  Git commit:       a61e2b4
  Built:            Fri Jul 21 20:35:45 2023
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.6.21
  GitCommit:        3dce8eb055cbb6872793272b4f20ed16117344f8
 runc:
  Version:          1.1.7
  GitCommit:        v1.1.7-0-g860f061
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```

---

```
docker ps
```

visualizamos los contenedores que estén activos

---

```
docker ps -a
```

visualizamos todos los contonedores:

```
CONTAINER ID   IMAGE         COMMAND    CREATED         STATUS                     PORTS     NAMES
453cab695f8b   hello-world   "/hello"   8 seconds ago   Exited (0) 4 seconds ago             optimistic_dubinsky
```

con su información

---

```
docker inspect  453cab695f8b
```

visualizamos la información del contenedor con el CONTAINER ID o el NAME

---

```
 docker run --name hello-Diego hello-world
```

creamos un contenedor nuevo, ponemos docker ps -a y miramos el nuevo contenedor:

```
CONTAINER ID   IMAGE         COMMAND    CREATED         STATUS                     PORTS     NAMES
5a49afb25c77   hello-world   "/hello"   6 seconds ago   Exited (0) 3 seconds ago             hello-Diego
453cab695f8b   hello-world   "/hello"   4 minutes ago   Exited (0) 4 minutes ago             optimistic_dubinsky
```

CADA VEZ QUE EJECUTAMOS DOCKER RUN, se crea el contenedor nuevo y lo corre

---

```
docker rename hello-Diego hola-platzy
```

Renombrar el contenedor

Es mejor manejar los contenedores con el ID y dejar el nombre por defecto que da docker

---

```
docker rm <CONTAINER ID> O <NAME>
```

se elimina e lcontenedor especifico

---

```
doker container prune 
```

Eliminar todos los contenedores parados

---

```
docker run ubuntu
```

```
docker run -it ubuntu
```

con este ultimo cambiamos la terminal de windows a Linux, -it se le suele llamar el modo interactivo

```
cat /etc/lsb-release
```

para ver la distribucion de linux que estamos usando me arroja:

```
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=22.04
DISTRIB_CODENAME=jammy
DISTRIB_DESCRIPTION="Ubuntu 22.04.3 LTS"
```

---

Ahora sin cerrar la terminal de ubuntu, abrimos otra terminal y con el comando

```
docker ps
```

```
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS         PORTS     NAMES
e974198145d9   ubuntu    "/bin/bash"   4 minutes ago   Up 4 minutes             quirky_payne
```

nos muestra que está efectivamente corriendo el contenedor de ubuntu

---

# CICLOS DE VIDAD DE UN CONTENEDOR:

CON EL COMANDO

```
exit
```

me salgo del contenedor de ubuntu y se apaga el proceso de ubuntu

---

```
docker run --name alwaysup -d ubuntu tail -f /dev/null
```

para mantener el proceso corriendo aunque de el comando exit, alwaysup es el nombre que le pusimos

---

para matar ese proceso docker kill alwaysup

---

Este comando de Docker se utiliza para ejecutar un contenedor basado en la imagen de Nginx en modo de demonio (`-d`). Nginx es un servidor web popular conocido por su alto rendimiento, estabilidad, configuración sencilla y bajo uso de recursos.

```
docker run -d --name proxy nginx
```

```
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS     NAMES
fd2b1705e5f7   nginx     "/docker-entrypoint.…"   54 seconds ago   Up 46 seconds   80/tcp    proxy
```



* `docker run`: este comando se utiliza para ejecutar un contenedor.
* `-d`: esta opción se usa para especificar que el contenedor se ejecutará en segundo plano, es decir, en modo de demonio.
* `--name proxy`: esto asigna el nombre "proxy" al contenedor. Puedes utilizar este nombre para hacer referencia al contenedor en lugar de usar su ID.
* `nginx`: esto especifica que el contenedor se basará en la imagen de Nginx. Docker descargará la imagen de Nginx si aún no está presente en el sistema y creará un nuevo contenedor basado en esa imagen.

```
docker stop proxy
```

detenemos el contenedor proxy

---



queremos exponer el puerto de proxy en la maquina principal 

```
docker run --name proxy -p 8080:80 nginx
```

pero queremos que apnte al puerto con este comando: 

```
docker run --name proxy -d -p 8080:80 nginx
```

como vemos cambio la parte del puerto: 

```
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                  NAMES
8d6e648ca4a0   nginx     "/docker-entrypoint.…"   8 seconds ago   Up 3 seconds   0.0.0.0:8080->80/tcp   proxy
```
