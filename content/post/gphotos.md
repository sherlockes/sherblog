sincronizar gphotos raspberry
Como sincronizar las imágenes de Google Photos desde la Raspberry
Hasta ahora había sincronizado mi galería de Google Fotos con el NAS desde el pc de sobremesa con Linux Mint, voy a intentar hacerlo desde la Raspberry para no tener que encender el pc. Estos sonlos paso que he seguido hasta hacer la sincronización.
sudo apt-get install python3-pip
https://pypi.org/project/gphotos-sync/
pip3 install --user pipenv
ghotos-sync en Raspberry
mkdir gphotos-sync
cd gphotos-sync
pip install gphotos-sync
/usr/bin/python3 -m pip install --upgrade pip
pip3 install --user pipenv
pipenv isntall gphotos-sync
generar las credenciales 
https://docs.google.com/document/d/1ck1679H8ifmZ_4eVbDeD_-jezIcZ-j6MlaNaeQiz7y0/edit?usp=sharing
copiar oauth en .config/gphotos/sync
cd <installed directory>
pipenv run gphotos-sync TARGET_DIRECTORY
------
4/1AY0e-g7mO0YmbBvGwXqRWZkdkMDDSyZXr7eNqT1_i0mTxExQNv2ckZX88d8
------
fusermount -u ~/gphotos
