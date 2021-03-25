---
title: "Que hace la raspberry sin mi"
date: "2021-03-25"
creation: "2021-02-17"
description: "Este es un pequeño resumen de lo que la Raspberry hace por mi cuando yo no estoy delante de ella"
thumbnail: "/images/20210217_raspberry_autofaenas_00.jpg"
disable_comments: true
authorbox: false
toc: True
mathjax: false
categories:
- "raspberry"
tags:
- "bash"
draft: true
weight: 5
---
La Raspberry me sirve para hacer muchas cosas aunque aquí sólo voy a contar las que hace ella sola cuando yo no estoy delante.
<!--more-->

Como todas las automatizaciones que tengo en la Raspberry son mediante cron, sacarlas a flote es tan simple como ejecutar `crontab -l` para obtener lo siguiente.

```
@reboot ~/SherloScripts/python/termoflask/init.sh
@reboot sleep 15 && ~/SherloScripts/bash/teledown.sh
@reboot sleep 15 && ~/SherloScripts/python/termoflask/mqtt.py
@daily ~/SherloScripts/bash/sherloscripts_push.sh
@weekly ~/SherloScripts/bash/radares.sh
@daily ~/SherloScripts/bash/hugo.sh
30 0 * * * ~/SherloScripts/bash/gphotos-sync.sh
@hourly ~/SherloScripts/bash/publish.sh
@hourly ~/SherloScripts/bash/teledown_move.sh
0 8,14 * * * ~/SherloScripts/python/tiempo.py
*/5 * * * * ~/SherloScripts/python/termoflask/termo.py
```
Si no tienes claro como realizar la programación en cron, es muy recomendable visitar [crontab guru] para realizar unas pruebas con el tema de los números y asteriscos para determinar la frecuencia de realización de la tarea.

### Termostato ###
Hace ya un tiempo que cambié el comercial Netatmo que tan buen resultado me dio durante más de tres años por un [termostato en la raspberry] con el que estoy igual de contento pero mucho más orgulloso. Para funcionar necesita tres entradas en el cron.

#### init.sh ####
```
@reboot ~/SherloScripts/python/termoflask/init.sh
```
Este script se ejecuta cada vez que arranca la raspberry y tiene la única función de arrancar el servidor web que flask lleva integrado para poder visualizar la interface del termostato.

#### mqtt.py ####
```
@reboot sleep 15 && ~/SherloScripts/python/termoflask/mqtt.py
```
Además de la temperatura en la habitación en la que está ubicada la raspberry, se capturan también los datos de otra habitación mediante mqtt y el broker [mosquitto] (instalado en la raspberry) gracias a una segunda sonda de Tª y un pequeño nodemcu.

El servidor debe arrancarse cada vez que se reinicia la raspberry y tiene la peculiaridad de tener que esperar unos segundos (15) para no dar falo con la conexión de red.

#### termo.py ####
```
*/5 * * * * ~/SherloScripts/python/termoflask/termo.py
```
Este es el corazón del termostato, cada 5 minutos se ejecuta el script para calcular consignas y estado de la calefacción en función de las temperaturas y de lo establecido en el panel de control.

### Teledown ###
La raspberry es tambien la encargada de [descargar arcivos de telegram] cuando no estoy delante del ordenador y los envío desde el teléfono gracias a [telegram-download-daemon] para lo que necesito la ejecución de unos scripts.

#### teledown.sh ####
```
@reboot sleep 15 && ~/SherloScripts/bash/teledown.sh
```
Es el corazon de las descargas y encargado de vigilar el canal para descargar todos los archivos que por ahí pasen. Se ejecuta cada vez que arranca la raspberry con un retraso de 15 sg para evitar evitar los problemas con el establecimiento de las conexiones de red.

#### teledown_move.sh ####
```
@hourly ~/SherloScripts/bash/teledown_move.sh
```
La raspberry está encendida todo el día, pero el NAS no, este script seejecuta cada hora para comprobar si el NAS está encendido y mover a este los archivos descargados por la raspberry.

### sherloscripts_push.sh ###
```
@daily ~/SherloScripts/bash/sherloscripts_push.sh
```
Gracias a [Insync] y la integración de Google Drive con Android, me resulta mucho más




![image_01]

[crontab guru]: https://crontab.guru
[descargar archivos de telegram]: https://sherblog.pro/descargar-archivos-de-telegram
[insync]: https://www.insynchq.com
[mosquitto]: https://mosquitto.org
[telegram-download-daemon]: https://github.com/alfem/telegram-download-daemon
[termostato en la raspberry]: https://sherblog.pro/termostato-raspberry/

[image-01]: /images/20210217_raspberry_autofaenas_01.jpg
