---
title: "Como sincronizar las imágenes de Google Photos desde la Raspberry"
date: "2020-12-03"
creation: "2020-12-03"
description: "Como sincronizar las imágenes de Google Photos desde la Raspberry"
thumbnail: "/images/20201203_sincronizar_gphotos_raspberry_00.jpg"
disable_comments: true
authorbox: false
toc: false
mathjax: false
categories:
  - "computing"
  - "productividad"
tags:
  - "bash"
  - "google photos"
draft: true
weight: 5
---
Hasta ahora había sincronizado mi galería de Google Fotos con el NAS desde el pc de sobremesa con Linux Mint, voy a intentar hacerlo desde la Raspberry para no tener que encender el pc.
<!--more-->
Para la sincronización de una carpeta local con mi contenido de Google Photos voy a utilizar el paquete pip [gphotos-syns] y estos son los paso que he seguido hasta hacer la sincronización.

* Instalamos el gestor de paquetes pip en python 3
* Actualizamos (Si es necesario)
* Instalamos la funcion de entornos virtuales
* Creamos el directorio "gphotos-sync" y nos ubicamos en el
* Instalamos el paquete pip gphotos-sync y el entorno virtual (Creo que con lo segundo valdría)

```
sudo apt-get install python3-pip
/usr/bin/python3 -m pip install --upgrade pip
pip3 install --user pipenv 
mkdir gphotos-sync 
cd gphotos-sync
pip3 install --user pipenv ghotos-sync 
pip install gphotos-sync
pipenv install gphotos-sync
```



generar las credenciales https://docs.google.com/document/d/1ck1679H8ifmZ_4eVbDeD_-jezIcZ-j6MlaNaeQiz7y0/edit?usp=sharing copiar oauth en .config/gphotos-sync 
cd <installed directory>
pipenv run gphotos-sync TARGET_DIRECTORY

------ 4/1AY0e-g7mO0YmbBvGwXqRWZkdkMDDSyZXr7eNqT1_i0mTxExQNv2ckZX88d8 ------

fusermount -u ~/gphotos

[gphotos-sync]: ttps://pypi.org/project/gphotos-sync/
[Image-01]: /images/20201203_sincronizar_gphotos_raspberry_01.jpg
