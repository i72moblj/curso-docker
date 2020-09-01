# Curso de Docker <!-- omit in toc -->

![Docker Logo](img/logo-docker.png)

# Tabla de contenidos <!-- omit in toc -->

- [SECCIÓN 1: Introducción al curso](#sección-1-introducción-al-curso)
  - [1.1 Introducción al curso](#11-introducción-al-curso)
  - [1.2 Introducción a Docker](#12-introducción-a-docker)
  - [1.3 Diferencia con máquinas virtuales](#13-diferencia-con-máquinas-virtuales)
  - [1.4 Versiones de Docker](#14-versiones-de-docker)
  - [1.5 Instalación de Docker en Debian 10 Buster](#15-instalación-de-docker-en-debian-10-buster)

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
