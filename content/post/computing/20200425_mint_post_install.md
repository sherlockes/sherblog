---
title: "Mi Post-instalación de Linux Mint"
date: "2020-04-27"
creation: "2020-04-25"
description: "Aquí están todos los pasos que doy después de la instalación de Linux Mint, perfectamente documentado para que no se me olvide..."
thumbnail: "images/20200425_mint_post_install_00.jpg"
disable_comments: true
authorbox: false
toc: true
mathjax: false
categories:
  - "linux"
tags:
  - "linux mint"
draft: false
weight: 5
---
Hay cientos de artículos sobre post-instalaciones de sistemas operativos. Este sólo pretendo que me sirva de guía para mi en las futuras instalaciones de linux Mint, aunque si le puede ayudar a alguien...
<!--more-->
## Keeweb (Gestor de contraseñas)
Descargamos el paquete .deb desde la web de [Keeweb] y lo instalamos manualmente. Con esto ya puedo acceder a todos los nombres de usuario y contraseñas que tengo guardadas en una archivo de [Google Drive].

## Brave (Navegador web)
En la actualidad es el navegador que más confianza y fluidez me proporciona. Aunque no está incluido en el gestor de software de Linux Mint, la instalación de [Brave] está perfectamente documentada en su web.

## Insync (Sincronizador de archivos)
La mayor parte de los archivos con los que trabajo a diario están en [Google Drive]. La forma más sencilla que conozco para trabajar con estos archivos es mediante el uso de [Insync].

Es importante que las carpetas sincronizadas de Google Drive se ubiquen dentro de la ruta "/home/sherlockes/Google_Drive/" para que el resto de aplicaciones puedan acceder a los archivos de configuración

## Touchpad Indicator (Gestor táctil portatil)
Para los que somos un poco manazas es fácil que se nos vaya algún pulgar al táctil del portatil mientras escribimos, de forma que nos mueve el puntero a otro punto del documento de forma que, como agaches un momento la cabeza... ya no estás escribiendo en el punto que lo estabas haciendo.

Gracias a [Atareao] por haber creado y mantenido [Touchpad Indicator], una sencilla utilidad para linux con la que gestionar el panel táctil de los portátiles con múltiples funciones.

La instalación es tan sencilla como añadir el repositorio, actualizarlo e instalar la aplicación.
```
sudo add-apt-repository ppa:atareao/atareao
sudo apt-get update
sudo apt-get install touchpad-indicator
```
## Emacs (Editor de texto)
Desde el repositorio de paquetes de Linux Mint aunque no sea la última versión. El archivo de configuración de Emacs llamado ".emacs" se creará en "/home/usuario/" y tendrá el siguiente contenido.
```
(setq user-init-file "/home/sherlockes/Google_Drive/SherloScripts/emacs/.emacs")
(setq user-emacs-directory "/home/sherlockes/Google_Drive/SherloScripts/emacs/.emacs.d/")
(setq default-directory "/home/sherlockes/")
(setenv "HOME" "/home/sherlockes/")
(load user-init-file)
```
### Inkscape (Diseño vectorial)
Desde que empece a trabajar con el, apenas abro Gimp y me he acostumbrado a crear imágenes vectoriales mucho más ligeras y fáciles de redimensionar sin perder calidad.

En el momento de escribir esto, la ultima versión de [Inkscape] es la 0.92 y su instalación para derivados de Ubuntu es posible hacerla apartir de repositorios.

```
sudo add-apt-repository ppa:inkscape.dev/stable-0.92
sudo apt update
sudo apt install inkscape
```

### Telegram Desktop (Mensajería instantanea y...)
Mis usos de Telegram los puedes ver en este post [Whatsapp Vs Telegram] y la instalación es tan simple como descargar [Telegram], descomprimir el archivo y ejecutar la aplicación desde la carpeta descomprimida. Es posible fijar la aplicación a la barra de inicio o crear un lanzador en el escritorio si no eres de los de abrir las aplicaciones desde el teclado...


### Rclone (Sincronizar nubes desde terminal)
Hasta ahora he escrito varios [artículos sobre Rclone] en los que explico algunos de mis usos de esta fantástica utilidad para conectar con casi cualquiera de las nubes públicas desde nuestra tarminal. Tal y como podemos comprobar en la web de [Rclone] la instalación es tan simple como una línea de terminal.
```
curl https://rclone.org/install.sh | sudo bash
```
La configuración completa de todas mis nubes la realizo copiando el archivo de respaldo "rclone.conf" al directorio "usuario/.config/rclone/rclone.conf"

{{< borrador >}}
### Zerotier (Virtual VPN)
curl -s https://install.zerotier.com | sudo bash
[Zerotier]

### ImageMagick ###

```
sudo apt install imagemagick
```

{{< / borrador >}}

Y por ahora esto es todo lo que tengo instalado en mi ordenador, poco a poco iré actualizando este post con todas las utilidades y configuraciones que vaya necesitando.


[artículos sobre Rclone]: https://sherblog.pro/tags/rclone/
[Atareao]: https://www.atareao.es
[Brave]: https://brave-browser.readthedocs.io/en/latest/installing-brave.html#linux
[Google Drive]: https://drive.google.com/
[Inkscape]: https://inkscape.org/es/release/
[Insync]: https://www.insynchq.com/downloads?start=true
[Keeweb]: https://keeweb.info
[Rclone]: https://rclone.org/downloads/
[Telegram]: https://telegram.org/dl/desktop/linux
[Touchpad Indicator]: https://www.atareao.es/aplicacion/touchpad-indicator-para-ubuntu/#
[Whatsapp Vs Telegram]: https://sherblog.pro/telegram-vs-whatsapp/
[Zerotier]: https://www.zerotier.com/download/
