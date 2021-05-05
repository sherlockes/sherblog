---
title: "Primera experiencia con Home Assistant"
date: "2021-04-28"
creation: "2021-04-28"
description: "Descripción"
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

![image_02]

[Home Assistant]: https://www.home-assistant.io
[instalación en raspberry]: https://www.home-assistant.io/installation/raspberrypi 

[image-01]: /images/20210428_home_assistant_01.jpg
[image-02]: /images/20210428_home_assistant_02.jpg


