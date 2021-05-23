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
Aquí dejo mis experiencias con Home assistant sobre una Raspberry Pi 4 de 4Gb.
<!--more-->


### Instalando Home assistant ###
El primer paso es acceder a la web de [Home Assistant] y sigo los pasos de [instalación en raspberry] para novatos. De esta forma se instalará la distribución completa de Home Assistant

#### Instalación de la distribución completa ####
1. En el artículo de [instalación en raspberry] buscamos el enlace para la versión que necesitamos
1. Grabar la imagen en el stick usk con [Balena Etcher]
1. Montar el stick en la raspberry, conectar cable de red y alimentación y encender
1. Esperamos unos minutos y ya se puede acceder a http://IP_raspberry:8123

> Si no sabes como descubrir la IP con la que la raspberry se ha conectado a la red puedes visitar mi [post] para obtener alguna respuesta.

##### Acceso ssh (Terminal) #####
1. Habilitar el modo avanzado desde la pestaña de usuario
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

#### Instalación sobre Raspberry OS ####
https://github.com/Kanga-Who/home-assistant/blob/master/Supervised%20Install%20on%20Raspberry%20Pi%20OS.md

### Importando la configuración ###
Si tenemos una inatantánea (snapshot) de una instalacion anterior resultará muy sencillo recuperar toda la configuración de home assistant. Para ello y tras instalar Home Assistant seguiremos los siguientes pasos:

1. Instalar "FTP" desde la "Tienda de complementos" del Supervisor
1. Iniciar el add-on desde la pestaña "Información"
1. En "opciones" de la pestaña "Configuración" cambiar el usuario y contraseña
1. Cambiar a "true" el permiso de subida ("allow_upload") y el acceso a la carpeta "backup"
1. Guardar los cambios y reiniciar el add-on
1. Acceder mediante ftp al servidor y copiar la instantánea en la carpeta "backup"
1. En "Supervisor-Instantáneas" seleccionar la subida y restaurarla
1. Espera unos minutos y accese de nuevo a Home Assistant

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

#### Sensor de temperatura con un ESP-WROOM-32 ####
Seguramente será matar moscas a cañonazos utilizar este microcontrolador como sólo sensor de temperatura. El ESP-WROOM-32 irá colocado en el salón y en un futuro es posible que haga más cosas.

![image_02]

Voy a usar una sonda DHT22 con una resistencia de 4.7 kOhm entre el pin de datos y el de 5V. El pin de datos lo coloco en el pin 13 del ESP y creo un nuevo nodo dentro del módulo ESPHome al que nombraré "salon" y será del tipo "nodeMCU-32S".

Al código que se genera automáticamente con la información de la wifi añado lo siguiente.

```
# Sensor de Temperatura AM2302
sensor:
  - platform: dht
    pin: 13
    temperature:
      name: "Temperatura Salón"
    humidity:
      name: "Humedad Salón"
    model: AM2302
    update_interval: 60s
```

### Termostato de la caldera ###
Tomaŕe como sensor la DHT22 conectada al ESP32 del salón y como relé el conectado al ESP01 de la caldera. Muy buenos resultados con unas pocas líneas de configuración que añadiremos al archivo "configuration.yaml"

```
climate:
  - platform: generic_thermostat
    name: Caldera
    heater: switch.rele
    target_sensor: sensor.temperatura_salon
    min_temp: 16
    max_temp: 23
    ac_mode: false
    target_temp: 19
    cold_tolerance: 0.2
    hot_tolerance: 0
    min_cycle_duration:
      seconds: 300
    keep_alive:
      minutes: 3
    initial_hvac_mode: "off"
    away_temp: 16
    precision: 0.1
```
Todos los parámetros están definidos en la [configuración del termostato genérico].


### Encendido remoto del NAS ###
La forma que hasta ahora he utilizado para el encendido del Nas cuando no tengo el dedo delante del mismo es mediante el comando "wakeonlan" a la MAC del NAS pero en este caso nos encontramos con un problema y es que debemos lanzarlos desde el sistema operativo del host para que funcione, no desde el docker sobre el que está corriendo Home Asistant. Gracias a este post de [siytek] he conseguido ver la luz al final del tunel siguiendo estos pasos.

1. 



Links:


[Balena Etcher]: https://www.balena.io/etcher/
[Debian para Raspberry]: https://raspi.debian.net/tested-images/
[Home Assistant]: https://www.home-assistant.io
[instalación en raspberry]: https://www.home-assistant.io/installation/raspberrypi
[post de Tecnosanvaras]: https://tecnosanvaras.es/instalacion-de-ha-supervisded-en-raspberry-pi-con-debian-10/
[post]: https://sherblog.pro/raspberry-montaje-y-ssh/
[siytek]: https://siytek.com/home-assistant-shell/
[termostato genérico]: https://www.home-assistant.io/integrations/generic_thermostat/

[image-01]: /images/20210428_home_assistant_01.jpg
[image-02]: /images/20210428_home_assistant_02.jpg


