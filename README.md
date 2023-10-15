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

para matar ese proceso 

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

En resumen, la diferencia principal entre los dos comandos es que el primer comando especifica explícitamente que el contenedor se debe ejecutar en segundo plano con la opción `-d`, mientras que el segundo comando no especifica explícitamente el modo de ejecución y, por lo tanto, el contenedor se ejecutará en primer plano.

como vemos cambio la parte del puerto:

```
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                  NAMES
8d6e648ca4a0   nginx     "/docker-entrypoint.…"   8 seconds ago   Up 3 seconds   0.0.0.0:8080->80/tcp   proxy
```


para ver en tiempo real que está pasando con el contenedor: 

```
docker logs <nombreContenedor>
```


---

# bind mount 

(compartir informacion, archivos o directorios entre contenedores y la maquina anfitriona) pero esto no es muy seguro

para ello Docker nos da una opcion llamada **Volumenes**


# *VOLUMENES  de Docker*

Volumenes es entre contenedores:

para ver los volumenes que hay :

```
docker volume ls
```

para crear Volumenes: 

```
docker volume create dbdata
```

volvemos a:

```
docker volume ls 
```

nos da: 

```
DRIVER    VOLUME NAME
local     dbdata
```

docker ya reservo un espacio en el disco para ese volumen,

ahora vamos a crear un contenedor y montarle el columen que creamos:

```
docker run -d --name db --mount src=dbdata,dst=/data/db mongo
```

creamos el contenedor db y montamos el volumen dbdata en el destino (/data/db) del contenedor= mongo

ahora como no tenemos la imagen de mongo, la instala automaticamente, cuando finaliza damos el comando 

```
docker ps
```

nos sale:

```
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS       NAMES
27f8ff941802   mongo     "docker-entrypoint.s…"   About a minute ago   Up About a minute   27017/tcp   db
```

ahora damos :

```
docker inspect db
```

nos sale la informacion del contenedor, en la parte de mount encontramos la informacion del volumen:

```
 "Mounts": [
            {
                "Type": "volume",
                "Name": "dbdata",
                "Source": "/var/lib/docker/volumes/dbdata/_data",
                "Destination": "/data/db",
                "Driver": "local",
                "Mode": "z",
                "RW": true,
                "Propagation": ""
            },
            {
                "Type": "volume",
                "Name": "0ae283cb84868d47dac85d75d219c432f2eee741a3f57c0249705cbc758bcc97",
                "Source": "/var/lib/docker/volumes/0ae283cb84868d47dac85d75d219c432f2eee741a3f57c0249705cbc758bcc97/_data",
                "Destination": "/data/configdb",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ],
```

---

Ahora para entrar al contenedor: 

```
docker exec -it db bash
```

luego el comando: 

```
mongosh
```

nos genera: 

```
Current Mongosh Log ID: 652c6539478070a05d7679b2
Connecting to:          mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+2.0.1
Using MongoDB:          7.0.2
Using Mongosh:          2.0.1

For mongosh info see: https://docs.mongodb.com/mongodb-shell/


To help improve our products, anonymous usage data is collected and sent to MongoDB periodically (https://www.mongodb.com/legal/privacy-policy).
You can opt-out by running the disableTelemetry() command.

------
   The server generated these startup warnings when booting
   2023-10-15T22:10:25.025+00:00: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine. See http://dochub.mongodb.org/core/prodnotes-filesystem
   2023-10-15T22:10:30.770+00:00: Access control is not enabled for the database. Read and write access to data and configuration is unrestricted
   2023-10-15T22:10:30.779+00:00: /sys/kernel/mm/transparent_hugepage/enabled is 'always'. We suggest setting it to 'never'
   2023-10-15T22:10:30.779+00:00: vm.max_map_count is too low
------

test>

```

ahora estamos dentro de la imagen de mongo:

para continuar, creamos una base de datyos llamada platzi:

```
use platzi
```

y luego insertamos un dato:

```
db.collection.insertOne({"nombre": "Diego"})
```

nos confirmará con el mensaje: 

```
{
  acknowledged: true,
  insertedId: ObjectId("652c67c1478070a05d7679b3")
}
```

verificamos con :

```
db.collection.find()

```

nos da: 

```
[ { _id: ObjectId("652c67c1478070a05d7679b3"), nombre: 'Diego' } ]
```

ya confirmado esto, cerramos el contenedor con los comandos exit, salimos de la imagen y luego exit salimos de linux, quedamos en la maquina anfitriona 

con el comando: docker ps, vemos que aún está activo el contenedor, eliminamos el contenedor con el comando: docker rm -f db

ahora para probar que el volumen que habíamos creado no se ha eliminado al eliminar el contenedor db, creamos otro contenedor con el mismo comando : 

```
docker run -d --name database --mount src=dbdata,dst=/data/db mongo
```

y entramos al contenedor:

```
docker exec -it database bash
```

ejecutamos el comando:

```
mongosh
```

luego el comando:

```
use platzi
```

luego el comando de encontrar datos de una coleccion:

```
db.collection.find()
```

y nos dará:

```
[ { _id: ObjectId("652c67c1478070a05d7679b3"), nombre: 'Diego' } ]
```
