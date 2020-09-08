# Curso de Docker <!-- omit in toc -->

![Docker Logo](img/logo-docker.png)

# Tabla de contenidos <!-- omit in toc -->

- [SECCIÓN 1: Introducción al curso](#sección-1-introducción-al-curso)
  - [1.1 Introducción al curso](#11-introducción-al-curso)
  - [1.2 Introducción a Docker](#12-introducción-a-docker)
  - [1.3 Diferencia con máquinas virtuales](#13-diferencia-con-máquinas-virtuales)
  - [1.4 Versiones de Docker](#14-versiones-de-docker)
  - [1.5 Instalación de Docker en Debian 10 Buster](#15-instalación-de-docker-en-debian-10-buster)
  - [1.6 Arquitectura de Docker, imágenes, contenedores y daemons](#16-arquitectura-de-docker-imágenes-contenedores-y-daemons)
- [SECCIÓN 2: Trabajar con contenedores](#sección-2-trabajar-con-contenedores)
  - [2.1 Arrancar y parar Docker](#21-arrancar-y-parar-docker)
  - [2.2 Crear nuestro primer contenedor](#22-crear-nuestro-primer-contenedor)

# SECCIÓN 1: Introducción al curso

## 1.1 Introducción al curso

En este curso vamos a ver todo lo relacionado con la tecnología Docker, desde las bases hasta la parte de *clustering* con Docker Swarm y la parte de KUBERNETES, qué es y cómo se integra con Docker.

![Introducción al curso](img/introducción-al-curso.png)

- **Introducción teórica**: Veremos las bases de la tecnología Docker, por ejemplo, qué es un contenedor, diferencias con las máquinas virtuales, ...
- **Instalación en Linux**
- **Instalación en Windows** Veremos algunos detalles, aunque el curso está enfocado al sistema GNU/Linux porque es el sistema operativo habitual en este tipo de plataformas
- **Trabajar con contenedores Docker**: Veremos cómo trabajar con contenedores dentro de Docker
- **Redes en Docker**: Veremos todo lo relacionado con redes en Docker, es decir, cómo se comunican estos contenedores entre sí y con el mundo exterior
- **Trabajar con volúmenes**: Veremos cómo trabajar con volúmenes, es decir, cómo guardar nuestros datos de forma que cada vez que trabajemos con un contenedor, tengamos la persistencia del dato y podamos recuperar la información generada con anterioridad.
- **Crear imágenes**: Veremos cómo crear nuestras propias imágenes y cómo subirlas a Docker Hub o a nuestros propios repositorios privados y en este caso vamos a utilizar la herramienta oficial de Docker que es Docker registry.
- **Orquestar contenedores con Compose**: Veremos Docker Compose, que es un orquestador sencillo de contenedores, que se utiliza mucho sobre todo cuando nuestros contenedores alcanzan cierta complejidad y más aún cuando tengo varios contenedores que se relacionan entre sí, de tal forma que no voy a poder trabajar de la manera habitual, sino que voy a necesitar de alguna herramienta que me permita integrar de manera correcta esos contenedores y sus relaciones. Esa herramienta es Docker Compose.
- **Introducción a Docker Swarm**: Docker Swarm nos permite realizar *clustering* de contenedores, es también un orquestador pero a mucho mayor nivel. Nos va a permitir alta disponibilidad, escalamiento de servicios, ... Digamos que es la herramienta de orquestación oficial de Docker.
- **Introducción a KUBERNETES**: Veremos una introducción a este *framework*, a este entorno. No es un curso completo, sino una idea general de qué es KUBERNETES, cómo funciona, crearemos un pequeño cluster y veremos cómo podemos trabajar con contenedores Docker dentro de ese cluster. Lo suficiente para introducirse en KUBERNETES.

## 1.2 Introducción a Docker

Vamos a ver una introducción sobre la arquitectura y los componentes que tiene Docker, veremos sus características y posibilidades y la diferencia respecto a las máquinas virtuales.

Docker es un proyecto *Open Source* que permite automatizar el despliegue de aplicaciones dentro de contenedores o *containers*.

Un contenedor empaqueta de manera ligera (ya veremos la diferencia con las máquinas virtuales) todo lo necesario para que uno o más procesos o aplicaciones funcionen: código, herramientas del sistema, bibliotecas del sistema, dependencias, ...

Dado que un contenedor es como una cápsula, una caja cerrada, podemos coger el contenedor y lanzarlo o usarlo en distintos entornos sin necesidad de tener que cambiar nada, es el propio contenedor el que se encarga de mantener esta infraestructura de manera transparente para el desarrollador o usuario final.

![Arquitectura de docker](img/arquitectura-de-docker.png)

Para empezar, un contenedor se va a ejecutar en un servidor en el que, por supuesto, lo primero que tenemos es el kernel del sistema operativo del servidor.

Encima del kernel lo que vamos a tener son contenedores, y ya veremos que entre el kernel y los contenedores lo que hay es otra capa intermedia que es *dockerd* el *daemon* o proceso en *background* que es el que se encarga de gestionar los contenedores.

Cada contenedor va a tener sus propios productos, en el primer caso de la imagen, una aplicación que va con Apache, Python y que por debajo lleva Ubuntu y dentro del mismo entorno, podemos tener otro contenedor que corre otra aplicación con Tomcat, MySQL y con Debian y así sucesivamente.

Lo interesante de todo esto es que estos contenedores aprovechan la capa del *kernel* para utilizar de manera conjunta los recursos de la máquina donde están. Ya veremos que esto se diferencia y mucho con el concepto de máquina virtual.

Por lo que podemos decir que Docker es un conjunto de contenedores que están funcionando sobre una capa del sistema operativo que es el kernel de la máquina.

**¿Y qué diferencia hay entre el contenedor y cómo se hacía antes?**

Si hacemos una comparación con la evolución del transporte de mercancías.

Antes, cada integrante de la cadena de transporte tenía sus propios contenedores con sus propias medidas, lo que provocaba un problema de logística.

Entonces se deció crear un formato estándar de contenedor que evitar estos problemas, de tal forma que todos los integrantes de este flujo de trabajo, desde el camión, el barco, la grúa del puerto, los vagones de tren, ... están adaptados para estos contenedores estándar, lo que supone un enorme ahorro de coste y energía.

Es decir, no me importa lo que contenga un contenedor, lo importante es que tiene unas medidas estándar que me permiten transportarlo en camión, tren, barco, ...

![Contenedores en el transporte](img/contenedores-en-el-transporte.png)

Pues igual ocurre con el mundo informático, si quiero empaquetar una aplicación, lo que puedo hacer es hacer una orientación con Docker a la de la vida real. Tengo una serie de productos que yo quiero poder utilizar en distintas plataformas.

Nuestro producto puede ser una aplicación web, entornos de datos, entornos de desarrollo y me gustaría que funcionaran en cualquier sistema operativo que soporte Docker, como Linux, Windows, Mac, o plataformas en cloud como puede ser AWS o Azure.

![Contenedores en informática](img/contenedores-en-informática.png)

Entonces esto es lo que pretende solucionar Docker, que el entorno se pueda *contenedorizar*, es decir, meter en un contenedor y que ese contenedor se pueda desplegar en cualquier plataforma soportada sin tener que cambiar absolutamente nada.

## 1.3 Diferencia con máquinas virtuales

Vamos a ver que aunque la filosofía es parecida, la solución impuesta a nivel de infraestructura es bastante distinta.

Una **máquina virtual** está compuesta básicamente por el hardware, el sistema operativo que tenemos por encima, por ejemplo GNU/Linux y luego un componente llamado hipervisor, que es el software de virtualización, por ejemplo VMware, VirtualBox, ... y lo que hace este hipervisor es que mantiene máquinas virtuales totalmente independientes. Cada máquina virtual está compuesta de su propio sistema operativo huesped, luego tiene su propio conjunto de librerías, binarios, ... y encima se ejecutan las aplicaciones. 

Entonces vemos que las máquinas virtuales tienen todas una estructura muy similar, sistema operativo, los componentes que lleve y aplicaciones. Es decir que son componentes monolíticos totalmente independientes unos de otros, con lo cual cada uno tiene su propia memoria, sus propios procesadores y sus propios rescursos de la máquina donde se aloja.

Por otro lado tendríamos los **contenedores**, que aunque parece que son algo parecido, no lo es, tenemos también el hardware, tenemos también el sistema operativo y luego tenemos un *daemon*, un demonio, un proceso llamado dockerd que se está ejecutando también en los servidores similar al hipervisor, pero a diferencia de las máquinas virtuales, los contenedores lo que hacen es aprovechar los recursos del sistema operativo donde se alojan para poder compartir opciones, características, librerías, binarios, ... es decir, no implementan su propio sistema operativo, no implementan sus propias librerías, no implementa sus propios binarios, sino que la comparten con el sistema operativo host, con la máquina donde están alojadas. Esto permite disponer de entornos más bajos de recursos de memoria, procesador, ... porque los contenedores, a diferencia de las máquinas virtuales, hacen uso compartido de esos recursos.

![Diferencia Docker y Máquinas Virtuales](img/diferencia-docker-maquinas-virtuales.png)

Viendo la imagen podemos observar que los contenedores son como procesos que se ejecutan en el sistema operativo y que son como cápsulas independientes unos de otros pero que a su vez también comparten recursos. En cambio las máquinas virtuales son entes totalmente monolíticos, independientes, que son máquinas realmente funcionales dentro de otra máquina. No es el caso de los contenedores, que utilizan más el concepto de procesos que comparten recursos.

Esto presenta distintas ventajas:

- Se pueden aplicar en distintos entornos tecnológicos, como modernizar aplicaciones tradicionales.
- Se puede utilizar de manera muy sencilla en CI (*Continuous Integration*) y CD (*Continuous Delivery*) en DEVOPS.
- Se puede integrar fácilmente en entornos cloud.
- Es una solución ideal para el concepto de microservicios, donde hay servicios totalmente independientes funcionando dentro de su entorno. Ahí los contenedores es la plataforma natural para este tipo de productos.
- ...

Realmente, los contenedores se pueden utilizar en muchos ámbitos del mundo tecnológico, y seguramente también podríamos alcanzar la misma o parecida solución utilizando máquinas virtuales, pero el ahorro de recursos hace que los contenedores sea una opción muy interesante para distintas plataformas y entornos tecnológicos.

## 1.4 Versiones de Docker

A la hora de hacer este curso, disponemos de dos versiones de Docker:

- *Docker CE (Docker Community Edition)*
- *Docker EE (Docker Enterprise Edition)*

La versión **Docker Community Edition** está pensada para desarrolladores y pequeñas empresas que necesitan este tipo de infraestructura.

Tiene dos líneas de actualización:

- *Stable*: Contiene la última versión estable
- *Edge*: Contiene las últimas novedades pero que no es muy, muy estable y no es recomendable utilizarla en entornos de producción.

La versión **Docker Enterprise Edition** está orientada para grandes infraestructuras donde vamos a necesitar soporte y una serie de herramientas más avanzadas, como herramientas gráficas, de soporte a la seguridad, ...

A su vez tiene tres versiones distintas:

- *Basic*
- *Standard*
- *Advanced*

Cuanto más avanzada, más dinero cuesta y más cosas tiene, más ámbitos de soporte, más herramientas, ...

Todo esto se puede consultar en la página oficial de [Docker](https://www.docker.com)

Este curso está enfocado a la versión  Docker Community Edition, aunque todo se puede aplicar a la versión Docker Enterprise Edition.

## 1.5 Instalación de Docker en Debian 10 Buster

Para realizar la instalación de Docker para los diferentes sistemas operativos, lo mejor es seguir las instrucciones que aparecen en la documentación oficial en [https://docs.docker.com](https://docs.docker.com)

En este caso, vamos a seguir las instrucciones para la instalación de Docker en Debian 10 Buster, para ello acudimos a la documentación oficial de Docker [Install Docker Engine on Debian](https://docs.docker.com/engine/install/debian/)

Actualizar los repositorios del sistema e instalar los paquetes necesarios

```console
$ sudo apt-get update

$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```

Añadir la clave GPG oficial de Docker

```console
$ curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
```

Verificar que tenemos la clave con el fingerprint `9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88`, mostrando los últimos 8 caracteres de la fingerprint

```console
$ sudo apt-key fingerprint 0EBFCD88

pub   4096R/0EBFCD88 2017-02-22
      Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid                  Docker Release (CE deb) <docker@docker.com>
sub   4096R/F273FCD8 2017-02-22
```

Crear un nuevo archivo de repositorio para Docker

```console
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
```

Actualizar los repositorios e instalamos la última versión de Docker Engine y containerd

```console
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```

Comprobar el estado del servicio Docker y que está enabled para que arranque el servicio al iniciar el sistema

```consolesudo
$ systemctl status docker

● docker.service - Docker Application Container Engine
   Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
   Active: active (running) since Fri 2020-05-29 19:48:09 CEST; 2h 5min ago
     Docs: https://docs.docker.com
 Main PID: 4878 (dockerd)
    Tasks: 26
   Memory: 57.0M
   CGroup: /system.slice/docker.service
           └─4878 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
```

Comprobar que lo tenemos funcionando

```console
$ sudo docker --version

Docker version 19.03.11, build 42e35e61f3
```

## 1.6 Arquitectura de Docker, imágenes, contenedores y daemons

Vamos a ver los conceptos teóricos básicos sobre la arquitectura de Docker y cómo funciona.

**Arquitectura de Docker**

![Arquitectura de Docker](img/arquitectura-docker.png)

Dentro de la arquitectura de Docker, vamos a tener el *Docker Host*, que es la máquina, el servidor donde se va a ejecutar el servicio de Docker, el daemon *dockerd*. Este proceso es el que se va a encargar de toda la relación y todos los comandos que hay que dar entre los distintos clientes y los contenedores que vamos a utilizar en nuestro trabajo diario.

Luego tenemos los clientes, por defecto tenemos un cliente docker, el *docker client*, que se ejecuta en modo comando (la versión enterprise y algunos proyectos tienen herramientas gráficas orientadas a Docker) y mediante el docker client vamos a ir ejecutando determinados comandos: pull, push, run, ... Y este cliente es el que se conecta al docker daemon, y en realidad, por debajo lo que hace es comandos de tipo REST-API que son las que comunican al dockerd, al daemon, lo que tiene que hacer.

Por otra parte tenemos las *imágenes*, que son como plantillas de un determinado contenedor. Por ejemplo, podemos tener una imágen con Ubuntu, Python y con Git y en esa imagen estaría reflejado todo lo necesario para construir un contenedor con esas características, por eso una imagen es como una plantilla, y es de sólo lectura y a partir de ellas construimos los *containers*.

Entonces, las imágenes son las plantillas y los contenedores son los objetos ya reales, las máquinas que están ya funcinando, basadas en esas plantillas. Si hacemos un simil con programación, las imágenes serían como el concepto de una clase de objeto y los contenedores serían como las instancias de esos objetos.

Yo puedo crear tantos contenedores como quiera de esas imágenes y también veremos cómo podemos crear imágenes con un determinado sistema operativo, una determinada versión de python, ...

Las imágenes, como veremos, están en local, se quedan en el servidor, pero realmente se traen de un repositorio denominado *registro*. Un registro de imágenes es un sitio donde se guardan esas imágenes para poder utilizarlas. El repositorio más famoso es [Docker Hub](https://hub.docker.com), que es propiedad de Docker, y que contiene montones de imágenes ya creadas. Pero en definitiva, lo que debe quedar claro es que las imágenes se pueden guardar en un repositorio, tanto de Docker Hub o en un repositorio propio, un registro propio.

En la instalación lo que hemos hecho ha sido instalar el Docker Host y el Docker Client y a partir de ahora lo que vamos a aprender es a crear contenedores a partir de imágenes ya hechas y cómo poder crear imágenes y subirlas y descargarlas a nuestro repositorio.

# SECCIÓN 2: Trabajar con contenedores

## 2.1 Arrancar y parar Docker

En este capítulo vamos a ver cómo arrancar y para Docker.

Si escribimos `docker` en la terminal y tabulamos dos veces, vemos que aparecen dos entradas:

- *dockerd*: Es el demonio, el proceso background de docker, que como hemos comentado anteriormente, es uno de los componentes que forman parte de Docker.
- *docker*: Es la herramienta cliente que vamos a utilizar durante todo el curso.

En este capítulo vamos a hablar de *dockerd*, que es el demonio que permite arrancar los servicios y procesos de Docker.

En Linux, para arrancar, reiniciar, parar y activar servicios, se utiliza `systemctl`.

Para ver el estado del servicio Docker:

```console
$ sudo systemctl status docker

● docker.service - Docker Application Container Engine
   Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
   Active: active (running) since Wed 2020-09-02 20:44:05 CEST; 1s ago
     Docs: https://docs.docker.com
 Main PID: 7879 (dockerd)
    Tasks: 13
   Memory: 45.8M
   CGroup: /system.slice/docker.service
           └─7879 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

sep 02 20:44:04 debian dockerd[7879]: time="2020-09-02T20:44:04.497536335+02:00" level=warning msg="Your kernel does not support swap memory limit"
sep 02 20:44:04 debian dockerd[7879]: time="2020-09-02T20:44:04.497567139+02:00" level=warning msg="Your kernel does not support cgroup rt period"
sep 02 20:44:04 debian dockerd[7879]: time="2020-09-02T20:44:04.497582095+02:00" level=warning msg="Your kernel does not support cgroup rt runtime"
sep 02 20:44:04 debian dockerd[7879]: time="2020-09-02T20:44:04.497779946+02:00" level=info msg="Loading containers: start."
sep 02 20:44:05 debian dockerd[7879]: time="2020-09-02T20:44:05.304249702+02:00" level=info msg="Default bridge (docker0) is assigned with an IP address 172.17.0.0/16. Daemon option --bip can be used to set a pr
sep 02 20:44:05 debian dockerd[7879]: time="2020-09-02T20:44:05.493536735+02:00" level=info msg="Loading containers: done."
sep 02 20:44:05 debian dockerd[7879]: time="2020-09-02T20:44:05.570044003+02:00" level=info msg="Docker daemon" commit=48a66213fe graphdriver(s)=overlay2 version=19.03.12
sep 02 20:44:05 debian dockerd[7879]: time="2020-09-02T20:44:05.570243248+02:00" level=info msg="Daemon has completed initialization"
sep 02 20:44:05 debian dockerd[7879]: time="2020-09-02T20:44:05.628974130+02:00" level=info msg="API listen on /var/run/docker.sock"
sep 02 20:44:05 debian systemd[1]: Started Docker Application Container Engine.

```

En este caso se está ejecutando, pero si queremos arrancar el servicio de Docker:

```console
$ sudo systemctl start docker
```

Para pararlo:

```console
$ sudo systemctl stop docker
```

Además de arrancarlo, tenemos que activar el servicio de Docker. Eso es para que se inicie el servicio al iniciar el servidor (o computadora). Si sólo arrancamos el servicio, pero no lo activamos, al reiniciar el servidor, si comprobamos el estado del servicio veremos que está parado. Por eso hay que activarlo.

```console
$ sudo systemctl enable docker
```

**Ejercicio Práctico:**
> Práctica 01 - Arrancar y parar los servicios Docker.pdf

## 2.2 Crear nuestro primer contenedor

Primero vamos a ver algunos comandos generales.

> **Importante** Docker hay que ejecutarlo como root o con el usuario sudo.

Para saber qué versión de Docker tenemos instalada:

```console
$ sudo docker --version

Docker version 19.03.12, build 48a66213fe
```

Con `docker info` obtenemos información mucho más amplia de lo que es toda la estructura de Docker


```console
$ sudo docker info

Client:
 Debug Mode: false

Server:
 Containers: 2
  Running: 0
  Paused: 0
  Stopped: 2
 Images: 11
 Server Version: 19.03.12
 Storage Driver: overlay2
  Backing Filesystem: extfs
  Supports d_type: true
  Native Overlay Diff: true
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 7ad184331fa3e55e52b890ea95e65ba581ae3429
 runc version: dc9208a3303feef5b3839f4323d9beb36df0a9dd
 init version: fec3683
 Security Options:
  apparmor
  seccomp
   Profile: default
 Kernel Version: 4.19.0-10-amd64
 Operating System: Debian GNU/Linux 10 (buster)
 OSType: linux
 Architecture: x86_64
 CPUs: 4
 Total Memory: 7.5GiB
 Name: debian
 ID: 3OYJ:6JUF:2KAY:B5AI:MTQ7:HM47:2ISE:YR7M:AZFE:NIQ4:DUHH:44MG
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false

WARNING: No swap limit support
```

Nos muestra información detallada sobre la estructura de nuestro docker, por ejemplo, cuántos contenedores tenemos, si se están ejecutando, si están parados, número de imagenes, información del driver de almacenamiento que está utilizando, espacio libre que le queda o necesita, etc.

Si ejecutamos `docker` a secas, sin indicar ninguna opción o comando, muestra la manera de usarlo, las opciones que tenemos disponibles y sus descripciones y los comandos disponibles que están divididos en comandos de gestión y comandos asociados y sus descripciones.

```console
$ docker

Usage:	docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Options:
      --config string      Location of client config files (default
                           "/root/.docker")
  -c, --context string     Name of the context to use to connect to the
                           daemon (overrides DOCKER_HOST env var and
                           default context set with "docker context use")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket(s) to connect to
  -l, --log-level string   Set the logging level
                           ("debug"|"info"|"warn"|"error"|"fatal")
                           (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA (default
                           "/root/.docker/ca.pem")
      --tlscert string     Path to TLS certificate file (default
                           "/root/.docker/cert.pem")
      --tlskey string      Path to TLS key file (default
                           "/root/.docker/key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit

Management Commands:
  builder     Manage builds
  config      Manage Docker configs
  container   Manage containers
  context     Manage contexts
  engine      Manage the docker engine
  image       Manage images
  network     Manage networks
  node        Manage Swarm nodes
  plugin      Manage plugins
  secret      Manage Docker secrets
  service     Manage services
  stack       Manage Docker stacks
  swarm       Manage Swarm
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes

Commands:
  attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes

Run 'docker COMMAND --help' for more information on a command.
```




Para obtener ayuda sobre un comando determinado:

```console
$ docker COMMAND --help
```






Ahora vamos a crear nuestro primer contenedor. Y para crear un contenedor hay que crearlo a partir de una imagen

 ```console
$ docker run IMAGE
```

En realidad, este comando lo primero que va a hacer es buscar esa imagen, primero en nuestro repositorio local, privado, que se crea automáticamente en el servidor al instalar Docker y, si no la encuentra, va a buscarla a [DockerHub](https://hub.docker.com) y la va a descargar a nuestro repositorio privado local (pull).

Y después va a crear un nuevo contenedor a partir de esa imagen y lo ejecuta.

De todo esto se va a encargar el demonio *dockerd*

**Ejemplo**

Vamos a crear un nuevo contenedor a partir de la imagen *hello-world*. Este contenedor es muy sencillo, y lo único que va a hacer es arrancar, escribir *Hola Mundo* y parar.

```console
$ sudo docker run hello-world

Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
0e03bdcc26d7: Pull complete 
Digest: sha256:7f0a9f93b4aa3022c3a4c147a449bf11e0941a1fd0bf4a8e6c9408b2600777c5
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

Si observamos la salida del comando, vemos que no encuentra la imagen localmente, por lo que la descarga (pull) de Docker Hub. Lo de latest es la etiqueta o última versión de la imagen (lo veremos más adelante)

También vemos que le asigna un ID a la imagen, porque en docker casi todos los objetos de Docker tienen un ID, los contenedores, las imágenes, ...

Luego crea el contenedor y lo ejecuta mostrando el *Hello from Docker!*

Y nos dice el proceso que ha seguido dockerd desde que se ejecuta el comando