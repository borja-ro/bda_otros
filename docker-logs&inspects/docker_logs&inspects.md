# BIG DATA APLICADO · RA2

<br><br>
> Ejercicio Docker Logs & Inspect 

<br><br>
<br><br>
---

```bash
$ cd /Alumno/BorjaRamosOliva
$ pwd
/Alumno/BorjaRamosOliva

$ date
Sat Jan 17 2026


$ echo $ASIGNATURA
Big Data Aplicado (BDA)

$ uname -a
Ubuntu Server 24.04.3 LTS · Raspberry Pi
```
---


<div class="page"></div>


## Ejercicio 1: Docker logs

En este ejercicio se trabaja el comando `docker logs`, que permite consultar la salida estándar (STDOUT y STDERR) de un contenedor en ejecución o detenido. A lo largo de la práctica se lanza un contenedor nginx, se analiza su arranque mediante los logs y se comprueba cómo estos registros reflejan la actividad generada dentro del contenedor.

Personalmente tengo docker instalado en Ubuntu Server 24.04 en una Raspberry Pi casera (nombre de equipo "raspborjen"), a la que accedo por ssh / remoto.

---

#### 1.- Preparación / chequeo del sistema

Verificamos la fecha, nombre del equipo y versión del sistema operativo.

```bash
borjaramos@raspborjen:~$ date
Sat Jan 17 13:01:41 CET 2026
borjaramos@raspborjen:~$ hostname
raspborjen
borjaramos@raspborjen:~$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 24.04.3 LTS
Release:        24.04
Codename:       noble
borjaramos@raspborjen:~$ 
```

---

#### 2.- Creación y ejecución de un contenedor (en este caso nginx)

Lanzamos un contenedor con la imagen oficial de nginx, asignándole el nombre "nginx1".

```bash
borjaramos@raspborjen:~$ date
Sat Jan 17 13:04:45 CET 2026
borjaramos@raspborjen:~$ docker run -d --name nginx1 nginx
32d6323797b8380c9caddee09e91b9ef1f884f8d4f779bd39e8ba201a9e97df1
borjaramos@raspborjen:~$ 
```

---

#### 3.- Listado de contenedores en ejecución

Comprobamos que el contenedor nginx1 está corriendo.

```bash
borjaramos@raspborjen:~$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS     NAMES
32d6323797b8   nginx     "/docker-entrypoint.…"   About a minute ago   Up About a minute   80/tcp    nginx1
borjaramos@raspborjen:~$ 
```

---

#### 4.- Análisis de logs de un contenedor

Consultamos los logs del contenedor nginx1 para ver su actividad

```bash
borjaramos@raspborjen:~$ date
Sat Jan 17 13:07:21 CET 2026
borjaramos@raspborjen:~$ docker logs nginx1
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/01/17 12:04:58 [notice] 1#1: using the "epoll" event method
2026/01/17 12:04:58 [notice] 1#1: nginx/1.29.4
2026/01/17 12:04:58 [notice] 1#1: built by gcc 14.2.0 (Debian 14.2.0-19)
2026/01/17 12:04:58 [notice] 1#1: OS: Linux 6.8.0-1044-raspi
2026/01/17 12:04:58 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1024:524288
2026/01/17 12:04:58 [notice] 1#1: start worker processes
2026/01/17 12:04:58 [notice] 1#1: start worker process 29
2026/01/17 12:04:58 [notice] 1#1: start worker process 30
2026/01/17 12:04:58 [notice] 1#1: start worker process 31
2026/01/17 12:04:58 [notice] 1#1: start worker process 32
borjaramos@raspborjen:~$ 
```

---

#### 5.- Acceso al contenedor en ejecución

Entramos dentro del contenedor nginx1 con una sesión bash

```bash
borjaramos@raspborjen:~$ date
Sat Jan 17 13:08:28 CET 2026
borjaramos@raspborjen:~$ docker exec -it nginx1 bash
root@32d6323797b8:/# 
```

---

#### 6.- Instalación de paquetes y prueba dentro del contenedor

Actualizamos los repositorios, instalamos wget y probamos que nginx responde correctamente en localhost.

```bash
borjaramos@raspborjen:~$ docker exec -it nginx1 bash
root@32d6323797b8:/# apt-get update
Get:1 http://deb.debian.org/debian trixie InRelease [140 kB]
Get:2 http://deb.debian.org/debian trixie-updates InRelease [47.3 kB]
Get:3 http://deb.debian.org/debian-security trixie-security InRelease [43.4 kB]
Get:4 http://deb.debian.org/debian trixie/main arm64 Packages [9607 kB]
Get:5 http://deb.debian.org/debian trixie-updates/main arm64 Packages [5404 B]
Get:6 http://deb.debian.org/debian-security trixie-security/main arm64 Packages [96.4 kB]
Fetched 9940 kB in 2s (6597 kB/s)
Reading package lists... Done
root@32d6323797b8:/# apt-get install -y wget
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following NEW packages will be installed:
  wget
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 970 kB of archives.
After this operation, 3871 kB of additional disk space will be used.
Get:1 http://deb.debian.org/debian trixie/main arm64 wget arm64 1.25.0-2 [970 kB]
Fetched 970 kB in 0s (8809 kB/s)
debconf: unable to initialize frontend: Dialog
debconf: (No usable dialog-like program is installed, so the dialog based frontend cannot be used. at /usr/share/perl5/Debconf/FrontEnd/Dialog.pm line 79, <STDIN> line 1.)
debconf: falling back to frontend: Readline
debconf: unable to initialize frontend: Readline
debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC entries checked: /etc/perl /usr/local/lib/aarch64-linux-gnu/perl/5.40.1 /usr/local/share/perl/5.40.1 /usr/lib/aarch64-linux-gnu/perl5/5.40 /usr/share/perl5 /usr/lib/aarch64-linux-gnu/perl-base /usr/lib/aarch64-linux-gnu/perl/5.40 /usr/share/perl/5.40 /usr/local/lib/site_perl) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 8, <STDIN> line 1.)
debconf: falling back to frontend: Teletype
Selecting previously unselected package wget.
(Reading database ... 6704 files and directories currently installed.)
Preparing to unpack .../wget_1.25.0-2_arm64.deb ...
Unpacking wget (1.25.0-2) ...
Setting up wget (1.25.0-2) ...
root@32d6323797b8:/# wget localhost
Prepended http:// to 'localhost'
--2026-01-17 12:10:05--  http://localhost/
Resolving localhost (localhost)... ::1, 127.0.0.1
Connecting to localhost (localhost)|::1|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 615 [text/html]
Saving to: 'index.html'

index.html                100%[==================================>]     615  --.-KB/s    in 0s

2026-01-17 12:10:05 (49.5 MB/s) - 'index.html' saved [615/615]

root@32d6323797b8:/# 
```

---

#### 7.- Verificación de logs tras wget

Los logs ahora muestran la petición GET realizada desde dentro del contenedor.

```bash
borjaramos@raspborjen:~$ date
Sat Jan 17 13:11:05 CET 2026
borjaramos@raspborjen:~$ docker logs nginx1
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/01/17 12:04:58 [notice] 1#1: using the "epoll" event method
2026/01/17 12:04:58 [notice] 1#1: nginx/1.29.4
2026/01/17 12:04:58 [notice] 1#1: built by gcc 14.2.0 (Debian 14.2.0-19)
2026/01/17 12:04:58 [notice] 1#1: OS: Linux 6.8.0-1044-raspi
2026/01/17 12:04:58 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1024:524288
2026/01/17 12:04:58 [notice] 1#1: start worker processes
2026/01/17 12:04:58 [notice] 1#1: start worker process 29
2026/01/17 12:04:58 [notice] 1#1: start worker process 30
2026/01/17 12:04:58 [notice] 1#1: start worker process 31
2026/01/17 12:04:58 [notice] 1#1: start worker process 32
2026/01/17 12:09:55 [notice] 1#1: signal 17 (SIGCHLD) received from 72
2026/01/17 12:09:55 [notice] 1#1: unknown process 72 exited with code 0
::1 - - [17/Jan/2026:12:10:05 +0000] "GET / HTTP/1.1" 200 615 "-" "Wget/1.25.0" "-"
borjaramos@raspborjen:~$ 
```

---

#### 8.- Análisis de procesos en ejecución

Listamos los procesos que están corriendo dentro del contenedor nginx1.

```bash
borjaramos@raspborjen:~$ docker top nginx1
UID                 PID                 PPID                TIME                CMD
root                16226               16203               00:00:00            nginx: master process nginx -g daemon off;
uuidd               16275               16226               00:00:00            nginx: worker process
uuidd               16276               16226               00:00:00            nginx: worker process
uuidd               16277               16226               00:00:00            nginx: worker process
uuidd               16278               16226               00:00:00            nginx: worker process
borjaramos@raspborjen:~$ 
```

---

#### 9.- Comando en segundo plano dentro del contenedor

Ejecutamos un comando `sleep` en el contenedor y volvemos a listar los procesos.

```bash
borjaramos@raspborjen:~$ docker exec nginx1 sleep 500
borjaramos@raspborjen:~$ docker top nginx1
UID                 PID                 PPID                TIME                CMD
root                16226               16203               00:00:00            nginx: master process nginx -g daemon off;
uuidd               16275               16226               00:00:00            nginx: worker process
uuidd               16276               16226               00:00:00            nginx: worker process
uuidd               16277               16226               00:00:00            nginx: worker process
uuidd               16278               16226               00:00:00            nginx: worker process
borjaramos@raspborjen:~$ 
```

---

#### 10.- Estadísticas de uso de recursos

Consultamos el consumo de CPU, memoria, red y disco del contenedor en tiempo real.

```bash
borjaramos@raspborjen:~$ docker stats nginx1
CONTAINER ID   NAME      CPU %     MEM USAGE / LIMIT     MEM %     NET I/O           BLOCK I/O     PIDS
32d6323797b8   nginx1    0.00%     5.988MiB / 7.751GiB   0.08%     11.3MB / 348kB    0B / 3.8MB    5
^C
borjaramos@raspborjen:~$ 
```

---

#### 11.- Parada de contenedor y verificación de estado

Forzamos la parada del contenedor y comprobamos que ya no está en ejecución.

```bash
borjaramos@raspborjen:~$ docker kill nginx1
nginx1
borjaramos@raspborjen:~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
borjaramos@raspborjen:~$ docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS                       PORTS     NAMES
32d6323797b8   nginx     "/docker-entrypoint.…"   22 minutes ago   Exited (137) 6 seconds ago             nginx1
borjaramos@raspborjen:~$ 
```
---

<div class="page"></div>

---

## Ejercicio 2: Inspects

En este ejercicio se trabaja el comando `docker inspect`, que permite obtener información
detallada tanto de imágenes como de contenedores. A lo largo de la práctica se inspecciona
una imagen de Node.js y un contenedor en ejecución, filtrando y extrayendo datos relevantes.

---

#### 1.- Preparación / chequeo del sistema

Verificamos la fecha, nombre del equipo y versión del sistema operativo.

```bash
borjaramos@raspborjen:~$ date
Sat Jan 17 17:39:48 CET 2026
borjaramos@raspborjen:~$ hostname
raspborjen
borjaramos@raspborjen:~$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 24.04.3 LTS
Release:        24.04
Codename:       noble
```

---

#### 2.- Descarga de la imagen "node"

Descargamos la imagen oficial de Node.js

```bash
borjaramos@raspborjen:~$ date
Sat Jan 17 17:40:46 CET 2026
borjaramos@raspborjen:~$ docker pull node
Using default tag: latest
latest: Pulling from library/node
1029f5ddc0d2: Pull complete
d72c713ab317: Pull complete
687ad46596f0: Pull complete
f018151ab3f3: Pull complete
56d64d6bb217: Pull complete
d870503b198d: Pull complete
b91d2b90b08d: Pull complete
2a212c016bb2: Pull complete
Digest: sha256:a2f09f3ab9217c692a4e192ea272866ae43b59fabda1209101502bf40e0b9768
Status: Downloaded newer image for node:latest
docker.io/library/node:latest
borjaramos@raspborjen:~$ 
```

---

#### 3.- Comprobamos que existe

Verificamos que la imagen de Node está disponible localmente.

```bash
borjaramos@raspborjen:~$ docker images | grep node
WARNING: This output is designed for human readability. For machine-readable output, please use --format.
node               latest    6b60a32b8603   1.13GB    0B
borjaramos@raspborjen:~$ 
```

---

#### 4.- Inspeccionamos

Obtenemos información detallada de la imagen de Node.js.

```bash
borjaramos@raspborjen:~$ docker image inspect node
[
    {
        "Id": "sha256:6b60a32b8603a187feaf24d4eb9431a676f28ee6d186558695426994d57edc60",
        "RepoTags": [
            "node:latest"
        ],
        "RepoDigests": [
            "node@sha256:a2f09f3ab9217c692a4e192ea272866ae43b59fabda1209101502bf40e0b9768"
        ],
        "Comment": "buildkit.dockerfile.v0",
        "Created": "2026-01-14T17:54:28.355878123Z",
        "Config": {
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NODE_VERSION=25.3.0",
                "YARN_VERSION=1.22.22"
            ],
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "Cmd": [
                "node"
            ]
        },
        "Architecture": "arm64",
        "Variant": "v8",
        "Os": "linux",
        "Size": 1133739147,
        ...
    }
]
```

---

#### 5.- Filtramos con grep para ver la versión

Extraemos la versión de Node.js de la información de la imagen.

```bash
borjaramos@raspborjen:~$ docker image inspect node | grep NODE_VERSION
            "NODE_VERSION=25.3.0",
```

---

#### 6.- Guardamos a un fichero para revisarlo después

Exportamos la información de inspect a un archivo JSON.

```bash
borjaramos@raspborjen:~$ docker image inspect node > node.json
borjaramos@raspborjen:~$ ls -lh node.json
-rw-rw-r-- 1 borjaramos borjaramos 2.9K Jan 17 17:45 node.json
```

---

#### 7.- Creamos un contenedor con la imagen de Node

Creamos un contenedor interactivo en modo detached.

```bash
borjaramos@raspborjen:~$ docker run -d -it --name node1 node
d9e28120304db7434c7fb67aea5805c3a314979edc9ab7cadc4d615853654989
borjaramos@raspborjen:~$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS     NAMES
d9e28120304d   node      "docker-entrypoint.s…"   10 seconds ago   Up 9 seconds              node1
```

---

#### 8.- Inspeccionamos el contenedor

Obtenemos información detallada del contenedor node1.

```bash
borjaramos@raspborjen:~$ docker inspect node1
[
    {
        "Id": "d9e28120304db7434c7fb67aea5805c3a314979edc9ab7cadc4d615853654989",
        "Created": "2026-01-17T16:47:12.676404818Z",
        "Path": "docker-entrypoint.sh",
        "Args": [
            "node"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 23745,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2026-01-17T16:47:12.834204009Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:6b60a32b8603a187feaf24d4eb9431a676f28ee6d186558695426994d57edc60",
        "LogPath": "/var/lib/docker/containers/d9e28120304db7434c7fb67aea5805c3a314979edc9ab7cadc4d615853654989/d9e28120304db7434c7fb67aea5805c3a314979edc9ab7cadc4d615853654989-json.log",
        "Name": "/node1",
        ...
        "NetworkSettings": {
            ...
            "Gateway": "172.17.0.1",
            "IPAddress": "172.17.0.2",
            ...
        }
    }
]
```

---

#### 9.- Filtramos IP asignada al contenedor

Extraemos la dirección IP del contenedor usando grep.

```bash
borjaramos@raspborjen:~$ docker inspect node1 | grep IPAddress
            "SecondaryIPAddresses": null,
            "IPAddress": "172.17.0.2",
                    "IPAddress": "172.17.0.2",
```

---

#### 10.- Ver la memoria compartida asignada

Extraemos el tamaño de memoria compartida (ShmSize).

```bash
borjaramos@raspborjen:~$ docker inspect node1 | grep ShmSize
            "ShmSize": 67108864,
```

---

#### 11.- Usamos --format para extraer propiedades específicas

**Dirección IP del contenedor:**
```bash
docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' node1
172.17.0.2
```

**Ruta del fichero de log del contenedor:**
```bash
docker inspect --format='{{.LogPath}}' node1
/var/lib/docker/containers/d9e28120304db7434c7fb67aea5805c3a314979edc9ab7cadc4d615853654989/d9e28120304db7434c7fb67aea5805c3a314979edc9ab7cadc4d615853654989-json.log
```

**Ver la imagen en la que está basado el contenedor:**
```bash
docker inspect --format='{{.Config.Image}}' node1
node
```

---
