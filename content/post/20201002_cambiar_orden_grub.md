---
title: "Cambiar el orden de los elementos del grub"
date: "2020-10-02"
creation: "2020-10-02"
descrption: "Cambiar el orden de los elementos del grub"
thumbnail: "/images/20201002_Cambiar_orden_grub_00.jpg"
disable_comments: true
authorbox: false
toc: true
mathjax: false
categories:
  - "linux"
tags:
  - "grub"
draft: true
weight: 5
---
En mi Pc de sobremesa coexisten Linux Mint y Windows 7, en el GRUB aparecen por defecto en primer lugar las opciones de Linux pero necesito que la opción en la que se produzca el arranque si el usuario no toca nada sea Windows, así lo he conseguido.
<!--more-->
Lo que puede parecer un complejo problema se resuelve con tres líneas de terminal. 

```
cd /etc/grub.d
sudo mv 30_os-prober 09_os-prober
sudo update-grub
```

La explicación es bastante simple. En el directorio "/etc/grub.d" están ubicados una serie de archivos que corresponden a cada uno de los tipos de arranque que puede tratar el grub.
_________ capture directorio______
La numeración de cada uno de los archivos corresponde a la posición en el menú de arranque. El archivo que hace referencia al arranque en Windows es el "30_os-prober", como el arranque en Linux corresponde al "10", solo hace falta cambiar 

[PcSteps]: https://www.pcsteps.com/1053-change-grub-boot-order-linux-mint-ubuntu/