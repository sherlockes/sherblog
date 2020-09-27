---
title: "Copiar ssh key desde Synology"
date: "2018-08-09"
description: "Como copiar las ssh key desde un nas synology a otro equipo"
thumbnail: "images/20180809_ssh_key_synology.jpg"
disable_comments: true
authorbox: false
toc: false
mathjax: false
categories:
  - "Sherver"
tags:
  - "synology" 
draft: false
weight: 5
---
### El problema
Generar la llave ssh (ssh-key) de un usuario dentro de un nas Synology es tan sencillo como acceder al mismo a través de ssh (Es necesario tener activado el servicio desde el panel de control de DSM y la opción "Terminal y snmp") y posteriormente ejecutar el comando.

```
ssh-keygen
```
Esta llave se guardará dentro de la carpeta del usuario en el directorio ".ssh" de modo en entre otros archivos se genera un "id_rsa.pub" que es el que deberemos copiar al equipo que queremos acceder.

>Para el caso del usuario root dentro de Synology es tan sencillo como después de haber accedido via terminal con la cuenta de un administrador introducir el comando `sudo -i` con la misma contraseña del administrador.


La copia de las llaves ssh de un equipo a otro es un procedimiento que , aunque en un perfecto ingles, está documentado en la web oficial de [ssh][1] pero en el momento que intentamos llevarlo a cabo nos encontramos con que el comando "ssh-copy-id" no está accesible en la terminal de Synology (Al menos en DSM 6.2)

De momento no cunde el pánico pues parece sencillo instalar esta utilidad mediante el comando `sudo apt-get install ssh-copy-id` pero la cosa se complica cuando me doy cuenta de que en Synology no es posible instalar paquetes mediante apt-get o aptitude.

### La solución
Tras una breve navegación por la web he encontrado una solución usando el comando "cat" utilizado para concatenar archivos y mostrarlos como salida según se muestra en el siguiente código.  Tan solo hay que introducir el nombre de usuario e ip del equipo al que queremos copiar las llaves.

```
cat ~/.ssh/id_rsa.pub | ssh <usuario>@<ip_equipo> 'cat >> .ssh/authorized_keys && echo "Llave copiada"'
```

Y ya está, ahora nuestro nas Synology deberá poder acceder al equipo remoto via ssh sin tener que introducir la contraseña.



[1]: https://www.ssh.com/ssh/copy-id

[2]: https://www.youtube.com/watch?v=XN9SuzV08Ew
[3]: https://blog.aaronlenoir.com/2018/05/06/ssh-into-synology-nas-with-ssh-key/
