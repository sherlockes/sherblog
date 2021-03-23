---
title: "Instalar rclone en Synology Ds920+"
date: "2021-03-22"
creation: "2021-03-22"
description: "Como instalar rclone en Synology Ds920+"
thumbnail: "/images/20210322_rclone_synology_00.jpg"
disable_comments: true
authorbox: false
toc: false
mathjax: false
categories:
- "computing"
tags:
- "bash"
- "rclone"
- "synology"
draft: false
weight: 5
---
En la migración de los discos duros desde el Ds216+II al Ds920+ uno de los problemas con que me he encontrado es que ha desaparecido la instalación de Rclone, toca volver a instalarlo.
<!--more-->

Sinceramente, no me acuerdo del metodo que tuve que seguir para instalar rclone en mi anterior synology, sólo dejo aquí las dos que hacen falta para instalarlo en el Ds920+ accediendo por ssh a la terminal.

```
sudo -i
curl https://rclone.org/install.sh | bash
```

> Aunque en la migración se ha perdido el programa no ha pasado lo mismo con los datos de configuración que siguen estando en "/var/services/home/.config/rclone/rclone.conf"

[Rclone]: https://rclone.org
