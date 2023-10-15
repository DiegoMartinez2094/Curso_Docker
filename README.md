
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
