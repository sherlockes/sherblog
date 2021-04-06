---
title: "Montar carpeta del NAS mediante sshfs"
date: "2021-04-06"
creation: "2021-04-06"
description: "Descripción"
thumbnail: "/images/20210406_sshfs_a_synology_00.jpg"
disable_comments: true
authorbox: false
toc: false
mathjax: false
categories:
- "computing"
tags:
- "bash"
- "synology"
draft: true
weight: 5
---
Resumen de introducción
<!--more-->

> Imprescindible para que funcione el comando sobre el NAS que esté habilitado el servidor sftp desde "Panel de control - Servicios de archivos - FTP - Habilitar servicio SFTP"

sudo sshfs -o allow_other,default_permissions usuario@ip_del_nas:/carpeta

> La ruta a montar comienza en la carpeta "Volume1" del NAS de modo que para la carpeta "/Volume1/fotos/2021/" en el comando de sshfs deberemos poner "/fotos/2021".

![image_01]

[link]: https://www.google.es

[image-01]: /images/20210406_sshfs_a_synology_01.jpg
