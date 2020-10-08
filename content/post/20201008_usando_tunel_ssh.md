---
title: "Usando un tunel ssh para aceder a un servidor remoto"
date: "2020-10-08"
creation: "2020-10-08"
descrption: "Usando un tunel ssh para aceder a un servidor remoto"
thumbnail: "/images/20201008_usando_tunel_ssh_00.jpg"
disable_comments: true
authorbox: false
toc: true
mathjax: false
categories:
  - "uncategorized"
tags:
  - "untagged"
draft: true
weight: 5
---
Esta vez me encuentro con que tengo que acceder a la configuración de un router remoto n la red del cual tengo "infiltrada" una Raspberry con Zerotier.
<!--more-->
Me encuentro en la necesidad de modificar la configuración de un router remoto del que me separan unos cuantos kilómetros y el único as que tengo en la manga es una raspberry que tengo que tengo conectada a el y agregada a mi red privada de zerotier. sudo ssh -L 80:192.168.1.1:80 pi@192.168.191.204
[Image_01]: /images/20201008_usando_tunel_ssh_01.jpg
