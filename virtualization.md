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


## Docker

Sistema de contenidors que permet executar codi "aïllat" dins un contenidor i compartint el mateix Kernel, això fa que sigui molt més lleuger que altres alternatives de virtualització.


### Dynamips

Emula hardware Cisco de forma que permet fer servir les imatges originals de Cisco, de fet es pot descarregar el software d'una màquina física i utilitzar-lo amb Dynamips. Cisco no suporta utilitzar el seu software fora dels seus equips.
Els darrers equips Cisco fan servir hardware molt diferent que no es pot emular amb Dynamips.


### IOU

Sistema intern de Cisco per executar IOS a Unix/Linux. Resulta molt eficient, però Cisco restringeix el seu us als empleats. Hi ha pocs sistemes que es puguin virtualitzar d'aquesta manera. Tot i això és una altenativa que pot funcionar amb pocs recursos.

Per poder utilitzar les imatges IOU cal afegir suport per 32 bits, ja que les imatges existents són de 32 bits. 



### VIRL

La única forma "legal" de virtualitzar sistemes Cisco (sempre que es pagui per la llicència, clar). GNS3 fa servir QUEMU per executar les imatges VIRL dels equips Cisco. 

Les imatges VIRL requereixen molts recursos de hardware, RAM i processador, no són les més recomanables per fer proves "senzilles".



