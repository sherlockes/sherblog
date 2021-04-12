---
title: "Configurando un relé wifi mediante esp-01 y micropython"
date: "2021-04-12"
creation: "2021-04-12"
description: "Configurando un relé wifi mediante esp-01 y micropython"
thumbnail: "/images/20210412_esp01_wifi_relay_00.jpg"
disable_comments: true
authorbox: false
toc: false
mathjax: false
categories:
- "computing"
tags:
- "bash"
draft: true
weight: 5
---
Resumen de introducción
<!--more-->

### Capacidad del ESP-01 ###

Conectamos el ESP-01 al programador y lo ponemos en el puerto usb con el botón de reset pulsado, sin soltarlo, ejecutamos el siguiente comando.

```
esptool.py --port /dev/ttyUSB0 --baud 460800
```

para ver la siguente respuesta en la consola

```
esptool.py v3.0
Serial port /dev/ttyUSB0
Connecting....
Detecting chip type... ESP8266
Chip is ESP8266EX
Features: WiFi
Crystal is 26MHz
MAC: e8:db:84:96:b9:9f
Uploading stub...
Running stub...
Stub running...
Changing baud rate to 460800
Changed.
Manufacturer: 20
Device: 4014
Detected flash size: 1MB
Hard resetting via RTS pin...
```

De donde se deduce que la capacidad de la memoria flash es de 1MB. con esta información acudimos a la web de [descargas de micropython] y descargamos la última versión estable de esta capacidad.

> Una vez que el chip ha sido detectado tras la ejecución del comando ya se puede soltar el botón del progarmador.


### Borrado de memoria e instalación de Micropython ###
Nuevamente la misma jugada, sacamos el conjunto de programador y ESP-01 del usb del pc y lo volvemos a meter con el botón de reseteo del programador pulsado. Sin soltarlo, ejecutamos el siguiente comando.

```
esptool.py --port /dev/ttyUSB0 --baud 460800 erase_flash
```

Para obtener la siguiente respuesta en la consola.

```
esptool.py v3.0
Serial port /dev/ttyUSB0
Connecting....
Detecting chip type... ESP8266
Chip is ESP8266EX
Features: WiFi
Crystal is 26MHz
MAC: e8:db:84:96:b9:9f
Uploading stub...
Running stub...
Stub running...
Changing baud rate to 460800
Changed.
Erasing flash (this may take a while)...
Chip erase completed successfully in 5.4s
Hard resetting via RTS pin...
```

Ya tenemos borrada la memoria del chip, ahora vamos a instalar el firmware de Micropyton que hemos descargado ejecutando el siguiente comando desde la carpea donde tenemos descargada la imagen. Si, esta vez tambien toca soltar el conjunto de programador y ESP-01 del usb del pc y volverlo a pinchar con el botón de reset pulsado, sin soltarlo ejecutar el siguiente comando.

```
esptool.py --port /dev/ttyUSB0 --baud 460800 write_flash --flash_size=detect 0 esp8266-1m-20210202-v1.14.bin
```

Si todo ha ido bien, se obtiene una respuesta como esta en la terminal.

```
esptool.py v3.0
Serial port /dev/ttyUSB0
Connecting....
Detecting chip type... ESP8266
Chip is ESP8266EX
Features: WiFi
Crystal is 26MHz
MAC: e8:db:84:96:b9:9f
Uploading stub...
Running stub...
Stub running...
Changing baud rate to 460800
Changed.
Configuring flash size...
Auto-detected Flash size: 1MB
Flash params set to 0x0020
Compressed 588960 bytes to 389085...
Wrote 588960 bytes (389085 compressed) at 0x00000000 in 9.4 seconds (effective 498.7 kbit/s)...
Hash of data verified.

Leaving...
Hard resetting via RTS pin...
```

### Desarrollando en Thonny ###
Sacamos nuevamente el conjunto de programador y ESP-01 del slot usb del ordenador y lo volvems a introducir para que se resetee el chip (esta vez no es necesario pulsar el botón del programador para entrar en modo programación) y abrimos Thonny. Ya tenemos listo en entorno de desarrollo.

![image_01]

```
import machine
import time
led = machine.Pin(1, machine.Pin.OUT)
while True:
    led.value(1)
    time.sleep(1)
    led.value(0)
    time.sleep(1)
```

> El Pin 1 es el correspondiente al led que va integrado en el propio ESP01, para activar el relé del módulo anexo deberemos activar el Pin 0.



[descargas de micropython]: https://micropython.org/download/esp8266/

[image-01]: /images/20210412_esp01_wifi_relay_01.jpg
