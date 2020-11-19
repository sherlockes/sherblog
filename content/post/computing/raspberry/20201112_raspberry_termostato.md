---
title: "Termostato Raspberry"
date: "2020-11-19"
creation: "2020-11-12"
description: "Proceso de la creación de un termostato mediante la raspberry en python"
thumbnail: "/images/20201112_termostato_raspberry_00.jpg"
disable_comments: true
authorbox: false
toc: true
mathjax: false
categories:
  - "raspberry"
tags:
  - "linux"
  - "raspberry"
  - "python"
  - "google sheets"
draft: false
weight: 5
---
Con la reciente muerte de termostato inteligente de NetAtmo a causa de una simple pila sulfatada me he decidido a crear el mio propio con la raspberry y un poco de Python. Iré escribiendo aquí los procesos con el mismo.
<!--more-->

### Captura de la temperatura ###
Para tomar la temperatura que tengo en casa voy a utilizar un DHT22 (sensor de humedad y temperatura) que, aunque no es lo más preciso del mundo, es suficientemente barato como para empezar a hacer pruebas. En el artículo de [atareao] se explica perfectamente como llevar a cabo esta medición. El propio sensor DHT22, una resistencia de 10k y tres cables para conectar al puerto GPIO de la Raspberry es odo lo que he necesitado.

[Image_01]: /images/20201112_termostato_raspberry_01.jpg

Necesitamos una librería para capturar los datos del sensor. Esta se instalará en la raspberry medianta el comando `sudo pip3 install Adafruit_DHT` siempre y cuando tengamos instalado Python3 y el gestos de paquetes pip.

```
import Adafruit_DHT as dht

datos_dht = dht.read_retry(dht.DHT22,4)

while datos_dht[0] > 100:
    time.sleep(5)
    datos_dht = dht.read_retry(dht.DHT22,4)
    
real_hume = round(datos_dht[0],2)
real_temp = round(datos_dht[1],2)
```

Sobre estas líneas aparece la parte de código en Python involucrada en la medición de la temperatura. Importación de la libreria [ADAfruit-DHT], captura de los datos del sensor y un redondeo a dos decimales que es más que suficiente.

> Alguna vez me ha dado un dato inválido la medición por lo que le he incrustado una condición de que siempre que la medición de humedad sea mayor del 100% (dato no válido) que espere 5 segundos y vuelva a medir.

Las variables utilizadas en la adquisición de temperatura son las siguientes:
- datos_dht - Tupla con los valores de la medida del sensor
- real_hume - Humedad medida y redondeada
- real_temp - Temperatura medida y redondeada


[atareao]: https://www.atareao.es/podcast/temperatura-con-la-raspberry/
[Adafruit-DHT]: https://pypi.org/project/Adafruit-DHT/
[jota]: https://github.com/domoticafacilconjota/capitulos/blob/master/temporada_1/S01E23/rester-ewelink

