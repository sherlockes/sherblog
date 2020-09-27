---
title: "Script de configuración para Raspberry"
date: "2018-12-04"
creation: "2018-11-06"
description: "La batalla por corregir una fuga de agua en una cisterna Roca de doble pulsador."
thumbnail: "images/20181106_raspberry_config_script_00.jpg"
disable_comments: true
authorbox: false
toc: false
mathjax: false
categories:
  - "raspberry"
tags:
  - "linux"
  - "shell"
draft: false
weight: 5
---
Está claro que de las partes más tediosas del cacharreo con la Raspberry es el tener que de nuevo comenzar la instalación desde cero y realizar siempre la misma configuración inicial.  Por esto he creado un Script que automatiza este proceso y realiza la siguiente configuración.

* Actualiza el firmware de la placa y los paquetes de Raspbian
* Cambia la contraseña para el usuario Pi
* Configura la IP y la puerta de enlace al router
* configura el huso horario y el idioma
* Instala Git
* Instala Pi-Hole
* Instala Hugo
* Instala Rclone
* Instala el servidor pivpn
* Programa un reinicio diario

Para comenzar descargamos el archivo de configuración y lo dotamos de permisos de ejecución.

```
wget https://sherblog.pro/files/raspi-config.sh
chmod +x raspi-config.sh
```

Con el comando `nano raspi-config.sh` editamos los siguientes parámetros
* Contraseña para el usuario "pi"
* Ip fija de nuestra red local que queremos asignar a la raspberry
* Ip del router o puerta de enlace

Ahora sólo resta ejecutar el script de configuración

```
./raspi-config.sh
```

La instalación no es completamente desatendida y requiere que estemos intesactuando con la misma en varios puntos.

- Seleccionar el área geográfica (Europa y Madrid)
- Configurar la codificación local (en-GB.UTF-8 y es-ES.UTF-8)
- Seleccionar eth0 como interface de Pi-hole
- Seleccionar Google como el proveedor DNS
- En la IP estática de Pi-Hole sustituir la existente por la que hemos configurado en el instalador
- Modificar la puerta de acceso si es necesario
- Habilitar la interface administrativa, el servidor web y las consultas de Pi-Hole

Con todo lo anterior el script de configuración queda de la siguiente forma

```
#!/bin/bash
# -*- ENCODING: UTF-8 -*-

# Raspberry custom init config script
# Updated on 20181120
# Created on 20181106
# Developed by Sherlockes
# www.sherblog.pro/files/raspiconfig.sh
# Description
#	- Firmware & Packages update
#	- Change Pi password
#	- Configure local network
#	- Configure timezone & locale
#	- Install Git
#	- Install Pi-Hole
#	- Install Hugo
#	- Install Rclone
# - Installing pivpn server
#	- Schedule daily restart

#Definiciones
password1="YOURPASS" # New Pi user pass
ipadress=192.168.1.202 # New IP Direction
gateway=192.168.1.1 # Router IP

# Modificamos el password para el usuario pi
sudo echo -e "raspberry\n$password1\n$password1" | passwd pi

### Updating...
sudo apt-get update
sudo apt-get upgrade -y
sudo apt-get install rpi-update
sudo rpi-update

### Network Config
sudo echo 'interface eth0' >> /etc/dhcpcd.conf
sudo echo 'static ip_address=$ipadress/24' >> /etc/dhcpcd.conf
sudo echo 'static routers=$gateway' >> /etc/dhcpcd.conf
sudo echo 'static domain_name_servers=$gateway' >> /etc/dhcpcd.conf

### Change Timezone & Locale
sudo dpkg-reconfigure tzdata
sudo dpkg-reconfigure locales

### Installing Git
sudo apt-get install git -y
sudo apt-get install jq -y

### Installing Pi-hole Web interface http://ip/admin
curl -sSL https://install.pi-hole.net | bash
sudo echo -e "$password1\n$password1" | pihole -a -p

### Installing latest version of Hugo
curl -s https://api.github.com/repos/gohugoio/hugo/releases/latest \
| grep "browser_download_url.*hugo_[^extended].*_Linux-ARM\.deb" \
| cut -d ":" -f 2,3 \
| tr -d \" \
| wget -qi -
installer="$(find . -name "*Linux-ARM.deb")"
sudo dpkg -i $installer
rm $installer

### Installing Rclone
curl -s https://api.github.com/repos/ncw/rclone/releases/latest \
| grep "browser_download_url.*rclone-[^extended].*-linux-arm\.deb" \
| cut -d ":" -f 2,3 \
| tr -d \" \
| wget -qi -
installer="$(find . -name "*linux-arm.deb")"
sudo dpkg -i $installer
rm $installer

### Installing vpn server
curl -L https://install.pivpn.io | bash

### Schedule Raspberry reboot at 4 A.M.
echo "00 04 * * * /sbin/reboot" | cat > cron
sudo crontab cron
rm cron

### Raspberry Reboot
sudo reboot
```


[11]: https://www.systutorials.com/39549/changing-linux-users-password-in-one-command-line/
[12]: https://pi-hole.net/
[13]: https://gist.github.com/steinwaywhw/a4cd19cda655b8249d908261a62687f8