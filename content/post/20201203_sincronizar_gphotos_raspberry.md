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
Para la sincronización de una carpeta local con mi contenido de Google Photos voy a utilizar el paquete pip [gphotos-sync] y estos son los paso que he seguido hasta hacer la sincronización.

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

Con el paquete instalado, toca generar las credenciales Oauth2 desde [Google Cloud Platform]

* Creamos un nuevo proyecto y le ponemos un nombre ("Create Project")
* Accedemos a [Photos Library API]
* Seleccionamos el proyecto que hemos creado en el desplegable superior
* Habilitamos la API (Botón "Enable")
* Accedemos a [Google Cloud Platform API] con nuestro proyecto seleccionado
* En la pestaña "OAuth consent screen" añadimos el nombre "gphotos-sync" a la aplicación
* En la pestaña "Credentials" añadimos una nueva credencial del tipo "OAuth client ID"
* Elegimos la opción "Desktop app" gphoto_client" y creamos la credencial
* Descargamos la credencial creada y la renombramos como "client_secret.json"
* Copiamos el archivo a la carpeta ".config/gphotos-sync" del usuario que la vaya a ejecutar

Este proceso está descrito en [Gphotos-sync OAuth Creation]


cd <installed directory>
pipenv run gphotos-sync TARGET_DIRECTORY

------ 4/1AY0e-g7mO0YmbBvGwXqRWZkdkMDDSyZXr7eNqT1_i0mTxExQNv2ckZX88d8 ------

fusermount -u ~/gphotos

> Tras unos días funcionando correctamente me ha aparecido un fallo en el montaje de la unidad de mota del NAS "read: Connection reset by peer" que he conseguido reparar habilitando nuevamente la conexión sftp del NAS

[Google Cloud Platform]: https://console.cloud.google.com/cloud-resource-manager
[Google Cloud Platform API]: https://console.cloud.google.com/apis/dashboard
[gphotos-sync]: https://pypi.org/project/gphotos-sync/
[Gphotos-sync OAuth Creation]: https://docs.google.com/document/d/1ck1679H8ifmZ_4eVbDeD_-jezIcZ-j6MlaNaeQiz7y0/edit?usp=sharing 
[Photos Library API]: https://console.cloud.google.com/marketplace/product/google/photoslibrary.googleapis.com


[Image-01]: /images/20201203_sincronizar_gphotos_raspberry_01.jpg
