---
title: "Acceso a NAS Synology mediante ssh-key"
date: "2021-04-06"
creation: "2021-04-06"
description: "Hay cientos de tutoriales en la web para acceder a un NAS de Synology mediante llave público-privada, esta es la forma que yo empleo."
thumbnail: "/images/20210406_sshkey_a_synology_00.jpg"
disable_comments: true
authorbox: false
toc: false
mathjax: false
categories:
- "computing"
tags:
- "synology"
draft: true
weight: 5
---
El acceso mediante llave público-privada es la forma más sencilla de acceder mediante ssh a nuestro NAS, por la red hay cientos de tutoriales de como hay que hacerlo. Esta es sólo la forma que a mi me ha funcionado sin problemas.
<!--more-->

1. Habilitar el servicio ssh (Panel de control - Terminal y SNMP)
1. Habilitar el servicio de inicio de usuario (Panel de control - usuario - avanzado)
1. Añadir el repositorio de [SynoCommunity] (Paquetes - Configuración - Orígenes del paquete - Agregar)
1. Instalar el paquete "SynoCli File Tools" para diponer de nano y otras utilidades de consola
1. Acceder al NAS mediante consola "ssh usuario@ip_del_nas"
1. Editar el archivo de configuración ssh `sudo nano /etc/ssh/sshd_config`
   1. Borrar la "#" de la línea "PubkeyAuthentication"
   2. Borrar la "#" de la línea "AuthorizedKeysFile"
1. Deshabilitar y volver a habilitar el servicio ssh (Panel de control - Terminal y SNMP)
1. Desde consola de la máquina desde la que queremos acceder al nas creamos la llave ssh `ssh-keygen -t rsa`
1. Copiamos la llave al NAS `ssh-copy-id usuario@ip_del_nas`
1. Nos conectamos al NAS mediante shh y cambiamos los permisos de las siguientes carpetas
```
chmod 700 ~
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

Y ya está.


Links de referencia
https://www.spacerex.co/use-ssh-keys-with-synology-nas/
https://brendonmatheson.com/2020/02/23/ssh-on-synology-with-key-pairs.html

![image_01]

[SynoCommunity]: https://synocommunity.com

[image-01]: /images/20210406_sshkey_a_synology_01.jpg
