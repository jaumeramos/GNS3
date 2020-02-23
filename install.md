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

### Cisco Dynamips

* Router Cisco 1700: [gns3-docs](https://docs.gns3.com/appliances/cisco-1700.html) 
    * c1700-adventerprisek9-mz.124-25d.image

* Router Cisco 3620: [gns3-docs](https://docs.gns3.com/appliances/cisco-3620.html) 
    * c3620-a3jk8s-mz.122-26c.image

* Router Cisco 3640: [gns3-docs](https://docs.gns3.com/appliances/cisco-3640.html) 
    * c3640-a3js-mz.124-25d.image

* Router Cisco 3660: [gns3-docs](https://docs.gns3.com/appliances/cisco-3660.html) 
    * c3660-a3jk9s-mz.124-15.T14.image

* Router Cisco 3725: [gns3-docs](https://docs.gns3.com/appliances/cisco-3725.html) 
    * c3725-adventerprisek9-mz.124-15.T14.image

* Router Cisco 3745: [gns3-docs](https://docs.gns3.com/appliances/cisco-3745.html) 
    * c3745-adventerprisek9-mz.124-25d.image

* Router Cisco 7200: [gns3-docs](https://docs.gns3.com/appliances/cisco-7200.html) 
    * c7200-adventerprisek9-mz.124-24.T5.image


### Cisco VIRL

* Switch Cisco IOSvL2: [gns3-doc](https://docs.gns3.com/appliances/cisco-iosvl2.html) 
    * IOSvL2 15.2: vios_l2-adventerprisek9-m.vmdk.SSA.152-4.0.55.E

* Switch Cisco NX-OSv: [gns3-doc](https://docs.gns3.com/appliances/cisco-nxosv.html) 
    * NX-OSv: titanium-final.7.3.0.D1.1.qcow2


* Router Cisco IOSv: [gns3-doc](https://docs.gns3.com/appliances/cisco-iosv.html) 
    * IOSv 15.6: vios-adventerprisek9-m.vmdk.SPA.156-2.T

* Router Cisco CSR1000v: [gns3-doc](https://docs.gns3.com/appliances/cisco-csr1000v.html) 
    * csr1000v-universalk9.16.07.01-serial.qcow2

* Router Cisco IOS XRv: [gns3-doc](https://docs.gns3.com/appliances/cisco-iosxrv.html) 
    * iosxrv-k9-demo-6.0.1.qcow2

    
### Mikrotik

* Router Mikrotik CHR: [gns3-doc](https://docs.gns3.com/appliances/mikrotik-chr.html) 
    * chr-6.45.6



### pfSense

* Firewall pfSense: [gns3-doc](https://docs.gns3.com/appliances/pfsense.html) 
    * pfSense-CE-2.4.4-RELEASE-p3-amd64


### Guests

* Guest: ipterm [gns3-doc](https://docs.gns3.com/appliances/ipterm.html) 

* Guest: Ubuntu Docker Guest [gns3-doc](https://docs.gns3.com/appliances/ubuntu-docker.html) 

