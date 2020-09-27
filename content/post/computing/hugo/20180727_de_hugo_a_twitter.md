---
title: "Insertar en Twitter enlaces a Hugo con imagen"
date: "2018-07-27"
description: "Si te encuentras con que los enlaces"
thumbnail: "images/20180726_hugo_twitter_card_00.jpg"
disable_comments: true
authorbox: false
toc: false
mathjax: false
categories:
  - "hugo"
  - "sherver"
draft: false
weight: 5
---
### El problema
Ya tenemos nuestra web en [Hugo][3] y resulta que cuando queremos compartir un enlace al mismo en [Twitter][1] este aparece como un miserable hipervínculo, ni imagen, ni resumen, ni título, ni na¡¡¡.  No se si el problema es común para todos los temas u ocurre simplemente con el que yo he utilizado.

No tardo mucho en encontrar información para darme cuenta de que todo pasa por el uso de metadatos por parte de [Twitter][1] para generar las tarjetas (Cards) tal y como se comenta en su sección de [desarrolladores][2] donde puedes invertir varias horas para documentarte o seguir leyendo durante unos minutos para solucionar tu problema.

Es decir, [Twitter][1] necesita unos metadatos que [Hugo][3] no está enviando.

### La solución

En todos los encabezados de los artículos tengo, entre otros, insertados los siguiente campos

```
title: "Enlaces con imagen en Twitter a Hugo"
description: "Te interesa leer si los links que compartes o comparten en Twitter de tu web en Hugo Salen sin imagen"
thumbnail: "images/20180712_wordpress_hugo.jpg"
```

Pues bien, el reto pasa por reaprovechar estos campos para generar los metadatos que [Twitter][1] está buscando para generar su tarjeta de enlace.  Hay varias opciones, pero la más práctica que he encontrado es editar el archivo "header.html" dentro de la carpeta "layouts/partials" e incluir en el "head" a continuación de los metas de [Hugo][3] estas líneas:

```
<meta name="twitter:card" content="{{ .Params.description }}" />
<meta name="twitter:title" content="{{ .Params.title }}" />
{{ if (.Params.thumbnail) }}<meta name="twitter:image" content="http://sherblog.pro/{{ .Params.thumbnail }}" />
{{else}}<meta name="twitter:image" content="http://sherblog.pro/images/logo.png" />
{{end}}
<meta name="twitter:creator" content="@sherblogpro">
<meta name="twitter:site" content="sherblog.pro" />
```

Bastante sencillo, cada metadato corresponde a un parámetro de nuestro archivo con una particularidad para el caso de las imágenes y es que hace distinción para el caso en que no encuentre el parámetro "thumbnail" que utilice en su lugar una imagen fija.

### Comprobando la solución

Podrías dedicarte a enviar y borrar tweets para comprobar que funciona la solución que hemos adoptado pero [Twitter][1] nos proporciona una herramienta más sencilla mediante su [validador de tarjetas][4].

![CardsGen][5]

Esta es una web en la que pegas el enlace que quieres comprobar y te muestra la tarjeta generada y los posibles problemas que encuentra en su generación.  Tras las modificaciones en el "header.html" la tarjeta generada aparece así.

![CardsGen][6]

[1]: https://twitter.com
[2]: https://developer.twitter.com/en/docs/tweets/optimize-with-cards/guides/getting-started.html
[3]: https://gohugo.io/
[4]: https://cards-dev.twitter.com/validator
[5]: /images/20180726_hugo_twitter_card_01.jpg
[6]: /images/20180726_hugo_twitter_card_02.jpg
