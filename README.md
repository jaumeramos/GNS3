# Virtualització de Xarxes amb GNS3

## Alguns conceptes bàsics sobre virtualització
 
 Hi ha diferents tipus de virtualització, natros quan fem servir el terme pensem en virtualització de hardware, però n'hi ha altres tipus:
 
 * Virtualització d'aplicacions: Les aplicacions s'executen de forma remota.
 * Virtualització d'escriptoris: L'entorn de treball és accessible des de qualsevol lloc.
 * Virtualització de xarxes: Es defineixen les xarxes per software, i es pot modificar ample de banda i molts paràmetres en temps real.
 * Virtualització d'emmagatzematge: Les dades s'emmagatzemen en clusters remots (núvol)
 * Virtualització a nivell de SO: **Contenidors**. Un contenidor permet crear un entorn aïllat en una màquina, però utilitzant els mateixos recursos de hardware. Ofereix un molt bon rendiment i facilitat de configuració, però limita a que utilitzem el mateix SO en tots els contenidors. Un exemple de contenidors és Docker.
 
 
 Respecte a la virtualització de harware, n'hi ha de 2 tipus principals
 
 * Emulació: Simular altres màquines traduïnt el codi binari. Les instruccions de la arquitectura a emular son convertides en temps real al crides del sistema host. Aquesta traducció adapta també el hardware de la màquina a emular al del sistema host. És una aproximació molt flexible, ja que permet "virtualitzar" quasi tot, ja que executa dirèctament el codi binari original, però resulta molt més lenta.
 
 * Virtualització total (Hypervisor Type 2): El hipervisor funciona com una aplicació dins un SO.  Es simula tot el hardware de la màquina (VirtualBox, VMWare...). 
 
 * Paravirtualització (Hypervisor Type 1): El hipervisor és només una capa lleugera que distribueix recursos entre les diferents VM (Xen, Proxmox, ESXi, Hyper-V). 
 
 * KVM: Kernel-based Virtual Machine és una barreja dels dos tipus. Corre sobre un SO, però actua com un hipervisor de tipus 1, ja que pot poroporcionar accés dirècte al hardware a les VM. KVM s'acostuma a fer servir juntament amb QUEMU(Quick Emulation) que pot actuar com un hipervisor complert de tipus 2. KVM representa una solució molt millor que VB o VMWare, pero... només per línux ;)
 

Cada cop el hardware suporta millor la virtualització, i es pot fer servir dirèctament des de les VM, amb molt poca pèrdua de rendiment. Fins i tot els processadors suporten virtualització aniuada ("Nested") que permet treballar amb VM dins de altres VM amb un rendiment adient.



### Virtualbox

Recordar que a VirtualBox, per poder instal·lar Guest Additions calen els següents paquets:
```
apt install build-essential dkms
```

Virtualbox suporta nested virtualization en processadors Intel des de la versió 6. Abans només ho suportava en processadors AMD.



##Instal·lar GNS3 a ubuntu 18.04

```
sudo add-apt-repository ppa:gns3/ppa
sudo apt update
sudo apt install gns3-gui gns3-server
```

Un cop instal·lat hem d'executar el programa `gns3` i seguir el assistent de configuració.


## Docker

Sistema de contenidors que permet executar codi "aïllat" dins un contenidor i compartint el mateix Kernel, això fa que sigui molt més lleuger que altres alternatives de virtualització.


Instal·lar:

```
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository \
"deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) stable"

sudo apt update

sudo apt install docker-ce
```

Cal afegir el vostre usuari a aquests grups i després tancar la sessió o reiniciar l'equip.
```
usermod -a -G ubridge,libvirt,kvm,wireshark,docker alumne
```


## IOU

Per poder utilitzar les imatges IOU cal afegir suport per 32 bits, ja que les imatges existents són de 32 bits. Cal recordar que IOU no està suportat en absolut per Cisco, és una eina per ús intern únicament.

```
sudo dpkg --add-architecture i386
sudo apt-get update
sudo apt-get install libssl1.0.0:i386
sudo ln -s libcrypto.so.1.0.0 /usr/lib/i386-linux-gnu/libcrypto.so.4
```


cal instal·lar un arxiu de llicències amb un codi de llicència que es pot obtenir amb el programa `CiscoIOUKeygen3f.py`

```
python3 CiscoIOUKeygen3f.py
``` 
Cal posar el text que mostra al fitxer `~\.iourc` i per evitar que envii paquets als servidors Cisco hem de canviar l'adreça de xml.cisco.com a l'arxiu `/etc/hosts`

```
echo '127.0.0.127 xml.cisco.com' >> /etc/hosts
```

## Simulació d'equips Cisco

### Dynamips

Emula hardware Cisco de forma que permet fer servir les imatges originals de Cisco, de fet es pot descarregar el software d'una màquina física i utilitzar-lo amb Dynamips. Cisco no suporta utilitzar el seu software fora dels seus equips.
Els darrers equips Cisco fan servir hardware molt diferent que no es pot emular amb Dynamips.


### IOU

Sistema intern de Cisco per executar IOS a Unix/Linux. Resulta molt eficient, però Cisco restringeix el seu us als empleats. Hi ha pocs sistemes que es puguin virtualitzar d'aquesta manera. Tot i això és una altenativa que pot funcionar amb pocs recursos.


### VIRL

La única forma "legal" de virtualitzar sistemes Cisco (sempre que es pagui per la llicència, clar). GNS3 fa servir QUEMU per executar les imatges VIRL dels equips Cisco.




## Components a Instal·lar

### Guests
ipterm

### Dynamips
Cisco 1700: c1700-adventerprisek9-mz.124-25d.image
Cisco 2691: c2691-adventerprisek9-mz.124-15.T14.image
Cisco 3620: c3620-a3jk8s-mz.122-26c.image
Cisco 3640: c3640-a3js-mz.124-25d.image
Cisco 3660: c3660-a3jk9s-mz.124-15.T14.image
Cisco 3725: c3725-adventerprisek9-mz.124-15.T14.image
Cisco 3745: c3745-adventerprisek9-mz.124-25d.image
Cisco 7200: c7200-adventerprisek9-mz.124-24.T5.image


###VIRL
Cisco IOSv 15.6: vios-adventerprisek9-m.vmdk.SPA.156-2.T
Cisco IOSvL2 15.2: vios_l2-adventerprisek9-m.vmdk.SSA.152-4.0.55.E
Cisco CSR1000v: csr1000v-universalk9.16.07.01-serial.qcow2
Cisco IOS xrv: iosxrv-k9-demo-6.0.1.qcow2
Cisco NX-OSv: titanium-final.7.3.0.D1.1.qcow2


### Mikrotik
MikroTik Cloud Hosted Router. 


# Laboratoris GNS3

## Enrutar VLANs amb switch Cisco i router Mikrotik

## Accedir a Internet fent NAT amb un router Cisco





Pàgines de descàrregues: https://upw.io
