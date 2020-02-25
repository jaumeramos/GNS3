# Taller GNS3


## Instal·lar el suport per virtualització en Una VM Ubuntu 18.04


## Utilitzar GNS3 amb un servidor Remot (VM)


## Utilitzar GNS3 amb la VM GNS3


## Enrutar VLANs amb switch Cisco i router Mikrotik

Es tracta de fer una configuració mixta amb switch Cisco i un router Mikrotik CHR.

![JJTT-1](https://i.imgur.com/iunq4Lv.png) 


Pel switch Cisco agafarem un dispositiu IOU on configurarem dos VLAN i un enllaç troncal cap al router.

El router Mikrotik CHR (Cloud Hosted Router) és una versió de RouterOS que podem instal·lar sense límit de temps en qualsevol equip, però sense pagar una llicència només ofereix un ample de banda limitat. Resulta perfecte per fer proves i no té la limitació temporal de 24h que té la versió per x86 del RouterOS.

Al switch Cisco configurarem 2 vlans, i verificarem el seu funcionament amb 2 terminals linux ipterm. Afegirem un port troncal cap al router que permeti les 2 vlan.

```
interface Ethernet0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 10,20

interface Ethernet0/1
 switchport mode access
 switchport access vlan 10

interface Ethernet0/2
 switchport mode access
 switchport access vlan 20
```

Configurarem el router Mikrotik per afegir les vlan en el interface troncal i verificarem el funcionament de l'enrutament entre vlans.

```
/interface vlan
add interface=ether2 name=vlan10 vlan-id=10
add interface=ether2 name=vlan20 vlan-id=20
/ip address
add address=10.10.10.1/24 interface=vlan10 network=10.10.10.0
add address=20.20.20.1/24 interface=vlan20 network=20.20.20.0
```

Un cop verificat el funcionament configurarem el router com a servidor DHCP per les dos vlan i habilitarem NAT per permetre que les vlan puguin accedir a Internet.

```
/ip dhcp-client
add disabled=no interface=ether1

/ip pool
add name=vlan10 ranges=10.10.10.100-10.10.10.110
add name=vlan20 ranges=20.20.20.100-20.20.20.110

/ip dhcp-server
add address-pool=vlan10 disabled=no interface=vlan10 name=dhcp-vlan10
add address-pool=vlan20 disabled=no interface=vlan20 name=dhcp-vlan20

/ip dhcp-server network
add address=10.10.10.0/24 dns-server=8.8.8.8 gateway=10.10.10.1 netmask=24
add address=20.20.20.0/24 dns-server=8.8.8.8 gateway=20.20.20.1 netmask=24

/ip firewall nat
add action=masquerade chain=srcnat out-interface=ether1

```

Un cop configurat verificarem que els terminals de cada VLAN poden obtenir adreça IP per DHCP, i que totes dos VLAN poden accedir a Internet.



## Accedir a Internet fent NAT amb un router Cisco

Es tracta de configurar un router Cisco per permetre a una xarxa interna accedir a Internet fent NAT. També configurarem el router Cisco com a servidor DHCP.

![JJTT-2](https://i.imgur.com/vLNhkpy.png)

Com a switch farem servir un IOU i com a router farem servir el model 7200, però afegint 2 interficies de xarxa ethernet, un  per la xarxa interna i un altre per Internet.

L'accés a Internet el farem utilitzant un template "cloud" que ens permet accedir al nostre interface de xarxa real.

Per la part de DHCP cal configurar un pool d'adreces i configurar les dades de la xarxa.

Per la part de NAT cal definir una ACL per permetre NAT a la xarxa interna i després aplicar NAT amb overloading (també es coneix com a PAT).


```
ip dhcp excluded-address 10.10.10.1 10.10.10.200

ip dhcp pool LAN
   network 10.10.10.0 255.255.255.0
   default-router 10.10.10.1 
   dns-server 8.8.8.8 

interface FastEthernet0/0
 ip address 10.10.10.1 255.255.255.0
 ip nat inside

interface FastEthernet0/1
 ip address dhcp
 ip nat outside

ip route 0.0.0.0 0.0.0.0 192.168.2.2

access-list 1 permit 10.10.10.0 0.0.0.255

ip nat inside source list 1 interface FastEthernet0/1 overload

```

Podem veure les adreces "prestades" amb:

```
 show ip dhcp binding 
 show ip dhcp pool

``` 

Un cop configurat comprovarem que el node amb Ipterm pot obtenir adreça IP per DHCP i que pot accedir a Internet.

