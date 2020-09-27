---
title: "Mis vértices geodésicos en Hugo"
date: "2020-05-12"
creation: "2020-05-12"
description: "Todo lo que he tenido que aprender para poder incrustar de una forma sencilla, ágil y digna mis vértices en Hugo"
thumbnail: "images/titulo_00.jpg"
disable_comments: true
authorbox: false
toc: true
mathjax: false
categories:
  - "hugo"
  - "vertices"
tags:
  - "shortcodes"
draft: true
weight: 5
---
Hace más de dos años desde que visité mi ultimo vértice geodésico. Durante todo este tiempo el proyecto que empecé en el ya lejano 2008 lo he tenido aparcado por unos cuantos motivos...
<!--more-->
- Cierre de Panoramio (Donde tenía todas las fotos y panorámicas)
- Abandono de Wordpress (En [Hugo] no es todo tan sencillo)
- Necesidad de pago en la API de Google Maps
- Falta de tiempo

Pues bien, al fin me he liado la manta a la cabeza y he conseguido sacar el tiempo suficiente para conseguir una solución digna al problema del archivo y representación de todos los vértices geodésicos a los que, hasta ahora, he conseguido llegar.

### Creación de una plantilla ###
La documentación de [Hugo] aunque es bastante extensa, en algunas ocasiones resulta complicada de seguir. Pretendo crear una plantilla común para los vértices geodésicos en los que únicamente cambien una serie de parámetros propios de cada vértice y un pequeño artículo del mismo. De esta forma, las imágenes y mapas de cada uno de los post se generarán automáticamente resultando muy sencillo así realizar una modificación común a todos.

En la cabecera de cada archivo *.md con en que se genera el post del vértice he incluido, además de los parámetros propios del tema otros propios del vértice.

```
js: https://cdn.jsdelivr.net/gh/openlayers/openlayers.github.io@master/en/v6.3.1/build/ol.js
css: https://cdn.jsdelivr.net/gh/openlayers/openlayers.github.io@master/en/v6.3.1/css/ol.css

mapa_alto: 400

vertice_nom: "Picones"
vertice_loc: "Velilla de Ebro"
vertice_lon: "-0.3656885"
vertice_lat: "41.3790035"
vertice_alt: "354"
vertice_hoja: "0413"
vertice_id: "42"
vertice_img: "tZbCy46"
vertice_pan: "VY8ieZb"

```
Las dos primeras líneas corresponden a una hoja de estilos y una clase javascript que utilizaré para la creación del mapa, la tercera hace referencia al número de pixeles que de alto va a ocupar el mapa en el post que se genere. El resto hacen referencia a valores del vértice como el nombre, la localización, las coordenadas, la altitud, la clasificación del ign y las imágenes del vértice.

Con esto tengo material suficiente para crear un [shortcode] en forma de archivo "mapa_vertice.html" que será guardado dentro de la carpeta "/layouts/shortcodes" (No la de dentro del tema sin en el directorio raiz y que deberemos crear si no existe)

El archivo [mapa_vertice.html] de momento no voy a explicarlo aquí con todos los detalles ya que puedes verlo en mi repositorio de GitHub. La finalidad es que incluyendo en el post del vértice

```
{{ mapa_vertice }}
```
ya se genere todo el contenido de forma automática atendiendo a los parámetros del post.

### Creación del archivo gpx ###


### Creación de una imagen para el vértice ###

### Listado de los vértices conquistados ###


### Representación en un mapa ###


### Almacenamiento de las imágenes ###
Este punto también ha resultado un profundo dolor de cabeza. Hasta su cierre tenía publicadas todas las imágenes y panorámicas en Panoramio. Las sigo teniendo en Google Photos pero no he sido capaz de incrustarlas en el blog desde aquí por lo que me he visto obligado a buscar un nuevo alojamiento para todas estas imágenes.
- *Usar Github como alojamiento* - Aunque factible, presenta una limitación por el tamaño máximo del repositorio a 1 gb por lo que queda descartada la opción.
- *Usar Internet Archive como alojamiento* - Lo intenté, pero el modo de subir las fotografías, gestionarlas y organizarlas es simplemente pésimo por lo que termine desestimando está opción.
- *Usar Imgur como alojamiento* - Después de comprobar en funcionamiento y capacidad de varios servicios web he probado por escoger está opción. El espacio de almacenamiento es ilimitado con el límite de 20mb por fotografía y 50 fotografías por hora lo cual es más que suficiente para mis necesidades.

[Hugo]: https://gohugo.io/
[shortcodes]: https://gohugo.io/content-management/shortcodes/
[mapa_vertice.html]: https://github.com/sherlockes/SherloScripts/blob/master/hugo/shortcodes/mapa_vertice.html

