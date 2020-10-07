usando tunel ssh
Usando un tunel ssh para aceder a un servidor remoto
Esta vez me encuentro con que tengo que acceder a la configuración de un router remoto n la red del cual tengo "infiltrada" una Raspberry con Zerotier. Me encuentro en la necesidad de modificar la configuración de un router remoto del que me separan unos cuantos kilómetros y el único as que tengo en la manga es una raspberry que tengo que tengo conectada a el y agregada a mi red privada de zerotier.

sudo ssh -L 80:192.168.1.1:80 pi@192.168.191.204
