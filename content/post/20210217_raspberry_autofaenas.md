---
title: "Que hace la raspberry sin mi"
date: "2021-02-17"
creation: "2021-02-17"
description: "Este es un pequeño resumen de lo que la Raspberry hace por mi cuando yo no estoy delante de ella"
thumbnail: "/images/20210217_raspberry_autofaenas_00.jpg"
disable_comments: true
authorbox: false
toc: false
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

@reboot ~/SherloScripts/python/termoflask/init.sh > ~/flask.log 2>&1
@reboot sleep 15 && ~/SherloScripts/bash/teledown.sh > ~/teledown/init.log 2> ~/teledown/error.log
@reboot sleep 15 && ~/SherloScripts/python/termoflask/mqtt.py > ~/mqtt.log 2>&1
@daily ~/SherloScripts/bash/sherloscripts_push.sh
@daily ~/SherloScripts/bash/radares.sh
@daily ~/SherloScripts/bash/hugo.sh
@daily ~/SherloScripts/bash/gphotos-sync.sh
@hourly ~/SherloScripts/bash/publish.sh
40 8-15/6 * * * ~/SherloScripts/python/tiempo.py
*/5 * * * * ~/SherloScripts/python/termoflask/termo.py > /home/pi/termostato.log 2>&1


![image_01]

[link]: https://www.google.es

[image-01]: /images/20210217_raspberry_autofaenas_01.jpg
