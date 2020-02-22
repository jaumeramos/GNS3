# Virtualització de Xarxes amb GNS3

Podeu fer servir el vostre equip real amb Linux o podeu utilitzar una VM ubuntu 18.04. En teniu una a l'enllaç de descàrrega de la pàgina del repositori. Aquesta VM té les Guest Additions de la versió **6.1.4** de virtualbox. Versions anteriors donen errors de compilació amb els darrers kernels de Ubuntu (5.3.28)


## Virtualbox

Recordar que a VirtualBox, per poder instal·lar Guest Additions calen els següents paquets:
```
apt install build-essential dkms
```

Virtualbox suporta nested virtualization en processadors Intel des de la versió 6. Abans només ho suportava en processadors AMD.



## Instal·lar GNS3 a ubuntu 18.04

```
add-apt-repository ppa:gns3/ppa
apt update
apt install gns3-gui gns3-server
```

Un cop instal·lat hem d'executar el programa `gns3` i seguir el assistent de configuració.


## IOU

Per poder utilitzar les imatges IOU cal afegir suport per 32 bits, ja que les imatges existents són de 32 bits. Cal recordar que IOU no està suportat en absolut per Cisco, és una eina per ús intern únicament.

```
sudo dpkg --add-architecture i386
sudo apt-get update
sudo apt-get install libssl1.0.0:i386
sudo ln -s libcrypto.so.1.0.0 /usr/lib/i386-linux-gnu/libcrypto.so.4
```


cal instal·lar un arxiu de llicències amb un codi de llicència que es pot obtenir amb el programa `CiscoIOUKeygen3f.py` Aquest programa es pot descarregar des de l'adreça: http://www.ipvanquish.com/download/CiscoIOUKeygen3f.py


```
python3 CiscoIOUKeygen3f.py
``` 
Cal posar el text que mostra al fitxer `~\.iourc` i per evitar que envii paquets als servidors Cisco hem de canviar l'adreça de xml.cisco.com a l'arxiu `/etc/hosts`

```
echo '127.0.0.127 xml.cisco.com' >> /etc/hosts
```


## Docker

Sistema de contenidors que permet executar codi "aïllat" dins un contenidor i compartint el mateix Kernel, això fa que sigui molt més lleuger que altres alternatives de virtualització.


Instal·lar:

```
apt-get install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

add-apt-repository \
"deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) stable"

apt update

apt install docker-ce
```

Cal afegir el vostre usuari a aquests grups i després tancar la sessió o reiniciar l'equip.
```
usermod -a -G ubridge,libvirt,kvm,wireshark,docker ubuntu
```


## Components a Instal·lar

Només utilitzarem templates disponibles al lloc oficial de GNS3, aquestes estàn verificades i són les que ofereixen major compatibilitat. si necessiteu imatges específiques haureu de fer les vostres cerques.

### Dynamips

Cisco 1700: c1700-adventerprisek9-mz.124-25d.image

Cisco 2691: c2691-adventerprisek9-mz.124-15.T14.image

Cisco 3620: c3620-a3jk8s-mz.122-26c.image

Cisco 3640: c3640-a3js-mz.124-25d.image

Cisco 3660: c3660-a3jk9s-mz.124-15.T14.image

Cisco 3725: c3725-adventerprisek9-mz.124-15.T14.image

Cisco 3745: c3745-adventerprisek9-mz.124-25d.image

Cisco 7200: c7200-adventerprisek9-mz.124-24.T5.image


### VIRL

Cisco IOSv 15.6: vios-adventerprisek9-m.vmdk.SPA.156-2.T

Cisco IOSvL2 15.2: vios_l2-adventerprisek9-m.vmdk.SSA.152-4.0.55.E

Cisco CSR1000v: csr1000v-universalk9.16.07.01-serial.qcow2

Cisco IOS xrv: iosxrv-k9-demo-6.0.1.qcow2

Cisco NX-OSv: titanium-final.7.3.0.D1.1.qcow2


### Mikrotik

MikroTik Cloud Hosted Router: chr-6.45.6



### pfSense

pfSense-CE-2.4.4-RELEASE-p3-amd64


### Guests

ipterm

