# Administración de Redes
- [Administración de Redes](#administración-de-redes)
  - [Linux](#linux)
  - [Distribuciones de Linux](#distribuciones-de-linux)
  - [Hipervisores y máquinas virtuales](#hipervisores-y-máquinas-virtuales)
  - [Docker y contenedores](#docker-y-contenedores)
  - [SSH, PKI, port forwarding, jump hosts](#ssh-pki-port-forwarding-jump-hosts)
    - [Ejemplos:](#ejemplos)
  - [Intro a la Administración de Linux](#intro-a-la-administración-de-linux)
    - [Automatización de la administración con Ansible](#automatización-de-la-administración-con-ansible)
    - [Instalación de software](#instalación-de-software)
    - [Creación de usuarios](#creación-de-usuarios)
    - [Permisos](#permisos)
    - [Monitoreo](#monitoreo)
    - [Un entorno de Linux en Windows: Windows Subsystem for Linux y Windows Terminal](#un-entorno-de-linux-en-windows-windows-subsystem-for-linux-y-windows-terminal)
## Linux
Linux es el nombre con el que se conoce al Kernel desarrollado por Linus Tolvards
en los años 80, cuando quería entender el procesador Intel
El kernel se empaquetó junto con un las herramientas GNU, proyecto liderado
por Richard Stallman, que permiten tener un ambiente Unix sin estar en el mismo sistema
A esta dupla se le conoce en general como GNU/Linux o Linux.


## Distribuciones de Linux
Una de las filosofías de Unix, es: "Hay más de una forma de hacerlo". Esto se ejemplifica con la gran cantidad de desarrollos alternativos para cumplir ciertas funciones. Por ejemplo, para el intérprete de comandos existen:
* Bourne Shell
* C Shell
* T-C Shell
* Bourne Again Shell
* Z Shell
* etc

Muchas veces, un empaquetamiento de Linux viene con varios shells para que el usuario elija, con uno por default. 
A las selecciones de software que se empaquetan por default se les conoce como **distribuciones** de Linux. Actualmente, en el entorno de servidores, sobresalen dos distribuciones: las basadas en Debian y las basadas en Red Hat.
Las basadas en Debian la principal es Ubuntu, que empezó en el entorno de escritorio.
Para las basadas en Red Hat, la principal es Red Hat Enterprise Linux y CentOS.

Entre los puntos más notables de diferencia de estas distribuciones están el manejador de paquetes (apt, yum, dnf) y la estructura de directorios.

##  Hipervisores y máquinas virtuales
El aumento de recursos como memoria, almacenamiento y procesador en los equipos de cómputo personales los hizo capaz de ejecutar aplicaciones que antes no se pensaban. A finales de 1990, en la Universidad de Cambridge Keir Fraser e Ian Pratt desarrollaron el proyecto Xen, un hipervisor que permitia administrar, auditar y contabilizar el acceso a estos recursos de unas equipos emulados o virtualizados.

Actualmente Xen sigue desarrollandose como parte de la compañía [Citrix](https://www.citrix.com/community/citrix-developer/citrix-hypervisor-developer/). Otros hipervisores muy utilizados son [Hyper-V](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/about/) que es una extensión disponible en Windows 10 y Windows Server y [VSphere Hypervisor](https://my.vmware.com/en/web/vmware/evalcenter?p=free-esxi7)

En Windows, además de Hyper-V es común ver ejecutar máquinas virtuales en [VirtualBox ](https://www.virtualbox.org/).

Las máquinas virtuales pueden transportarse entre hipervisores mediante un formato llamado Open Virtualization Format, en archivos con extensión ova, por ejemplo, [este sitio](https://virtualboxes.org/images/centos/) ofrece varias máquinas virtuales de CentOS.

## Docker y contenedores
Aunque las máquinas virtuales son muy útiles, recientemente se han sofisticado aún más con la introducción de contenedores, los cuales son el equivalente a máquinas virtuales mínimas que ofrecen alguna funcionalidad, la cual puede ser muy simple (por ejemplo PHP en línea de comando) o bastante más compleja como Wordpress. Debido a que son instalaciones con una funcionalidad específica, es muy fácil usarlas y acceder a funcionalidad que peude ser dificil configurar desde cero.

Ejemplos:
* ```docker info```: Información general de docker en caso de que esté funcionando
* ```docker ps```: Muestra los contenedores en ejecución
* ```docker ps -a```: Muestra los contenedores en ejecución y detenidos
* ```docker rm identificador```: Elimina el contenedor identificado por ```identificador```
* ```docker pull imagen```: Obtiene la imagen específicada
* ```docker run imagen``` Ejecuta la imagen especificada 

Docker permite ejecutar y gestionar estos contenedores de manera muy intuitiva. Para darte una idea de que funcionalidad pueden ofrecer los contenedores, visita https://hub.docker.com

## SSH, PKI, port forwarding, jump hosts
Los servidores o máquinas virtuales de Linux se instalan sin interfaz gráfica. Este formato lo está siguiendo también Windows Server con su instalación ["Core"](https://docs.microsoft.com/en-us/windows-server/administration/server-core/what-is-server-core)

El acceso a la línea de comando actualmente en Linux (y en versiones modernas de Windows Server) se realiza mediante el protocolo Secure Shell o SSH. Este protocolo permite comunicaciones encriptadas mediante Criptografía Asimétrica (o criptografía de llave pública).
El administrador del sistema a acceder entrega una llave pública y una privada o solamente una llave privada e instala la llave pública en los sistemas a acceder, con lo cual el usuario puede atenticarse aún sin passwords.

Además de sesiones de línea de comandos, SSH se puede usar para:
* Montaje de filesystems (sshfs),
* Transferir archivos (sftp, scp)
* Crear un canal encriptado para la ejecución de otros comandos (por ejemplo git sobre ssh)

Mediante SSH se puede acceder servicios en redes privadas si se tiene acceso a un host con ssh y acceso a la red púbica y a la red privada mediante redireccionamiento de puertos (port forwarding)

### Ejemplos:
  * ```ssh host```SSH a host con parámetros por omisión
  * ```ssh usuario@host``` o ```ssh -l usuario host```: SSH a host especificando usuario
  * ```ssh -i llave.pem usuario@host```: SSH a host especificando también llave
  * ```ssh -J usuario@internmedio hostdestino```: SSH a host utilizando a un host intermedio
  * ```ssh -ND 8080 usuario@intermedio```: SSH usando a host intermedio como proxy tipo SOCKS en el puerto 8080 local
  * ```sftp usuario@host```: Transferencia usando sftp
  * ```scp -r directorio_local usuario@hostremoto:/home/usuario```: Transferencia de un directorio local a equipo remoto mediante scp

## Intro a la Administración de Linux
La administración de Linux se puede explicar como las tareas que permiten ofrecer servicios de red de manera continua. Entre estas tareas están:
* Instalación, configuración, actualización, respaldo del sistema operativo
* Administración de usuarios
* Administración de aplicaciones y servicios

### Automatización de la administración con Ansible
Cuando se realiza administración a un número considerable de sistemas, es importante poder automatizar lo más posible. Existen varios frameworks de administración como Chef, Puppet, SaltStack, Ansible y Terraform.

[Ansible](https://docs.ansible.com/ansible/latest/user_guide/index.html) es un producto de Red Hat, que no requiere agentes instalados en el servidor a administrar, está basado en Python y sus archivos de entrada están escritors en YAML.

### Instalación de software
Comando Ubuntu Linux: ```apt install```
### Creación de usuarios
Comando Linux: ```useradd```
### Permisos
Comando Linux: ```chmod``` 


### Monitoreo
* Ejemplo de Nagios
* Ejemplo con Elasticsearch, Logstash y Kibana

### Un entorno de Linux en Windows: Windows Subsystem for Linux y Windows Terminal
