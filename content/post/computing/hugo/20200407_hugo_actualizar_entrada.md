---
title: "Insertar comentarios en Hugo"
date: "2020-04-08"
creation: "2020-04-07"
description: "Está es la forma que utilizo para actualizar entradas y evitar que se muestren los cambios hasta que esté terminada la actualización."
thumbnail: "images/20200407_hugo_actualizar_entrada_00.jpg"
disable_comments: true
authorbox: false
toc: false
mathjax: false
categories:
  - "hugo"
tags:
  - "hugo"
draft: false
weight: 5
---
Cuando no se tiene mucho tiempo libre, la publicación de un post puede demorarse a lo largo de varios días. Para eso Se mantiene la entrada en modo "borrador". Pero... ¿Como lo hago al actualizar una entrada?
<!--more-->
Está claro que no es factible pasar una entrada a modo borrador mientras la estamos actualizando ya que la volvemos invisible y durante todo el tiempo que la estemos editando los potenciales visitantes no podrán acceder al contenido original.

Cuando añado contenido a alguna entrada uso un concepto extraído de [andreasbergstrom.com] en el que explica cómo es posible "comentar" parte del contenido del archivo Markdown para que [Hugo] no lo interprete como contenido a mostrar.

Para ello es necesario crear un archivo "borrador.html" dentro de la carpeta "layouts/shortcodes/" de nuestra web. El contenido de este archivo simplemente será

```
{{ if .Inner }}{{ end }}
```

A partir de aquí, mientras estemos añadiendo contenido a una entrada lo haremos dentro de una zona comentada de forma que Hugo no la muestre.

```
{{</* borrador */>}}
Aquí iremos añadiendo el contenido con el que queremos actualizar la entrada...
{{</* / borrador */>}}
```

Y así de sencillo, cuando queramos que el contenido actualizado se muestre tan solo tenemos que eliminar las primera y última linea.

[Hugo]: https://gohugo.io/
[andreasbergstrom.com]: https://andreasbergstrom.com/posts/comments-in-hugo/
