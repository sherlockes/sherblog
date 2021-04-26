---
title: "Renombrado de lotes de archivos en Emacs"
date: "2021-04-26"
creation: "2021-04-26"
description: "Como renombrar un conjunto de archivos al mismo tiempo gracias a Emacs"
thumbnail: "/images/20210426_emacs_renombrado_lotes_00.jpg"
disable_comments: true
authorbox: false
toc: false
mathjax: false
categories:
- "computing"
tags:
- "emacs"
draft: true
weight: 5
---
Cuando era usuario de windows, usaba [Renamer] para cambiar el nombre de varios archivos en lote, en linux todavía no he encontrado una aplicación que me guste así que lo hago gracias a Emacs de una forma rápida.
<!--more-->

Tenemos que reemplazar o eliminar una cadena de texto dentro del nombre de un conjunto de archivos ubicados en un directorio. Seguimos los siguiente pasos.

1. Abrir en modo "Dired" el directorio -> Alt+x dired (C-c D)
1. Entrar en modo renombrado -> Alt+x dired-toggle-read-only (C-x C-q)
1. Buscar la cadena a reemplazar -> Alt-x query-replace (M-%)
1. Introducir el valor con que reemplazar
1. Reemplazar todos -> !
1. Finalizar el modo de renombrado -> Alt+x wdired-finish-edit (C-c C-c)

Hay que reconocer que no es nada intuitivo pero, na vez conocido el método es rápido y sencillo.




![image_01]

[Renamer]: https://portableapps.com/apps/utilities/renamer-portable

[image-01]: /images/20210426_emacs_renombrado_lotes_01.jpg
