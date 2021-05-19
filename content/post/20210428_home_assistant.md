---
title: "Home assistant desde cero con Raspberry"
date: "2021-04-28"
creation: "2021-04-28"
description: "Es este post incluyo mis aprendizajes con Home Assistant"
thumbnail: "/images/20210428_home_assistant_00.jpg"
disable_comments: true
authorbox: false
toc: false
mathjax: false
categories:
- "computing"
tags:
- "home-assistant"
- "iot"
draft: true
weight: 5
---
Resumen de introducción
<!--more-->
El primer paso es acceder a la web de [Home Assistant] y sigo los pasos de [instalación en raspberry] para novatos (La versión para Docker la probé hace un tiempo y me dió algún fallo con el modo supervisor y los addons que me impidió seguir adelante).

### Instalando Home assistant ###

#### Instalación de la distribución completa ####
1. En el artículo de [instalación en raspberry] buscamos el enlace para la versión que necesitamos
1. Grabar la imagen en el stick usk con [Balena Etcher]
1. Montar el stick en la raspberry, conectar cable de red y alimentación y encender
1. Esperamos unos minutos y ya se puede acceder a http://IP_raspberry:8123

> Si no sabes como descubrir la IP con la que la raspberry se ha conectado a la red puedes visitar mi [post] para obtener alguna respuesta.

##### Acceso ssh (Terminal) #####
1. 
1. Acceder a la tiemda de complementos en el panel del supervisor
1. Instalar "Terminal & ssh"
1. En "Configuración - Opciones" introducir una contraseña en "password: "
1. En "Configuración - Red" introducir "22" como puerto y guardar
1. En "Información" iniciar el add-on
1. Ya es posible acceder desde la terminal con `ssh root@IP_HOMEASSISTANT`


#### Instalación sobre Debian 10 en Raspberry ####
He realizado este proceso sobre una Pi4 de 4Gb con un stick ubs para el arranque siguiendo el [post de Tecnosanvaras] y realizando los siguientes pasos:

1. Descargar la ultima versión de [Debian para Raspberry]
1. Descomprimir la imagen
1. Grabar la imagen en el stick usk con [Balena Etcher]
1. Conectar la Raspberry a un teclado, monitor y red cableada
1. Insertar el stick usb y arrancar la Raspberry
1. Tras el arranque, logearnos como "root"
1. Actualizar las dependencias - `apt update`
1. Instalar sudo - `apt install sudo`
1. Crear un nuevo usuario - `adduser TU_USUARIO`
1. Añadir el usuario creado al grupo sudo - `usermod -aG sudo TU_USUARIO`
1. Averiguar la IP de la Raspberry - `ip addr show eth0`
1. Reiniciar la raspberry

> Importante no realizar un "apt upgrade" de la distribución ya que, a día de hoy (20210517), presenta un bug con el aranque desde usb que hace que se cuelgue el sistema.

Con esto ya sólo resta instalar una utilidades, docker y el supervisor de home assistant mediante las siguientes líneas de código.

```
sudo -i

apt-get install -y software-properties-common apparmor-utils apt-transport-https ca-certificates curl dbus jq network-manager

systemctl disable ModemManager

systemctl stop ModemManager

curl -fsSL get.docker.com | sh

curl -sL "https://raw.githubusercontent.com/Kanga-Who/home-assistant/master/supervised-installer.sh" | bash -s -- -m raspberrypi4-64
```

Con la instalación realizada, ya estamos en condiciones de acceder a la IP que se nos muestra al terminar el script a través del puerto 8123.

```
[info] 
[info] Home Assistant supervised is now installed
[info] First setup will take some time, when it's ready you can reach it here:
[info] http://192.168.1.104:8123
[info] 
```

Este script nos monta una serie de contenedores, la mejor forma de gestionarlos es mediante portainer. Lo instalaremos con el siguiente comando y accederemos a el a través del pueto 9000 de nuestra Raspberry

```
docker run -d --name=Portainer --restart=always -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer
```





### Añadiendo microcontroladores ESP ###
Desde el apartado "supervisor" accederemos a la tienda de complementos, donde buscaremos "ESPHome" para instalar el correspondiente módulo.

#### ESP-01 con relé de un canal ####
1. Añadir un nuevo dispositivo (El + de abajo a la dcha)
2. Nombrarlo
3. Seleccionar "Generic ESP8266 (for example Sonoff)" para ESP-01
4. Introcudir esid del wifi y pass
5. Enviar la información para crear el nodo

Ahora tendremos el nuevo nodo creado con una línea roja encima

6. Actualizar el firmware del ESP-01
- Montamos el ESP en el programador y enchufamos este al dispositivo donde esté corriendo la distribución de Home Assistant.

> Muy importante pulsar el botón de booteo del programador para introducir el programador al puerto USB y no soltarlo¡¡¡

- En este momento, Home Assistant habrá reconocido un dispositivo conectado en un puerto serie y lo seleccionamos en el desplegable donde pone "OTA (Over-The-Air)"

- Pulsamos la opción (Sin soltar el botón de booteo del programador) "UPLOAD" y esperamos, cruzando los dedos, a que en la ventana llegue a mostrarse algo como lo siguiente indicando que el firmware ha sido correctamente subido al microcontrolador.

![image-01]

> En alguna ocasión me ha ocurrido que la contraseña del wifi con se ha copiado correctamente en el archivo de configuración y no es posible la conexión del dispositivo. Hay que comprobarlo desde el menú "EDIT" del nodo que no podemos conectar.

- En este punto ya podemos quitar el ESP-01 del programador, conectarlo al módulo con el relé y alimentarlo para que se conecte al wifi y lo podamos actualizar via OTA

- Desde el menú "EDIT" del nodo que hemos creado añadiremos ahora el código necesario al final para que el microcontrolador sea capaz de manejar el relé en el correspondiente puerto GPIO

El código que tenemos que introducir para que funcione este relé de un sólo canal es unas pocas líneas que se muestran a continuación.

``` yaml
switch:
  platform: gpio
  name: "rele"
  pin:
    number: GPIO0
    inverted: True
```
- Guardar y subir y ya está nuestro relé listo.

#### Regulación de temperatura ####
Tenemos un senson de temperatura y un rele ya configurados gracias a ESPHome




![image_02]

[Balena Etcher]: https://www.balena.io/etcher/
[Debian para Raspberry]: https://raspi.debian.net/tested-images/
[Home Assistant]: https://www.home-assistant.io
[instalación en raspberry]: https://www.home-assistant.io/installation/raspberrypi
[post de Tecnosanvaras]: https://tecnosanvaras.es/instalacion-de-ha-supervisded-en-raspberry-pi-con-debian-10/
[post]: https://sherblog.pro/raspberry-montaje-y-ssh/

[image-01]: /images/20210428_home_assistant_01.jpg
[image-02]: /images/20210428_home_assistant_02.jpg


