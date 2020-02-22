# Taller GNS3

## Enrutar VLANs amb switch Cisco i router Mikrotik

Es tracta de fer una configuració mixta amb switch Cisco i un router Mikrotik CHR.

Pels switch Cisco agafarem un dispositiu 

El router Mikrotik CHR (Cloud Hosted Router) és una versió de RouterOS que podem instal·lar sense límit de temps en qualsevol equip, però sense pagar una llicència només ofereix un ample de banda limitat. Resulta perfecte per fer proves i no té la limitació temporal de 24h que té la versió per x86 del RouterOS.


Al switch Cisco configurarem 2 vlans, i verificarem el seu funcionament amb 2 terminals linux ipterm. Afegirem un port troncal cap al router que permeti les 2 vlan.

Configurarem el router Mikrotik per afegir les vlan en el interface troncal i verificarem el funcionament de l'enrutament entre vlans.


## Accedir a Internet fent NAT amb un router Cisco

Es tracta de configurar un router Cisco per permetre a una xarxa interna accedir a Internet fent NAT.

L'accés a Internet el farem utilitzant un template "cloud" que ens permet accedir al nostre interface de xarxa real. Cal habilitar el mode promiscu en l'interface de VirtualBox i també en l'interface de la màquina real.



```
ip link set eno1 promisc on
```
