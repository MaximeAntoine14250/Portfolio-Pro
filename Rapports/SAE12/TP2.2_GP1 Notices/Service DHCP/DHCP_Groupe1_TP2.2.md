# Documentation Projet SAÉ 12

Projet réalisé par ANTOINE Maxime, BRETONNIERE Martin, COEURET Tristan, DAIRIN Come et SCHER Florian du 12 décembre 2024 au 10 janvier 2024.

# Notice DHCP

## Configuration du DHCP

### Matérielle utilisé :

Commutateur Cisco catalyst 3750

### Commande utilisé : 

```
ip dhcp excluded-address 192.168.0.0 192.168.0.3
ip dhcp excluded-address 192.168.0.64 192.168.0.68
ip dhcp excluded-address 192.168.0.128 192.168.0.132
ip dhcp excluded-address 192.168.0.192 192.168.0.196
ip dhcp excluded-address 192.168.1.64 192.168.1.68
ip dhcp excluded-address 192.168.1.128 192.168.1.132
!
ip dhcp pool VLAN100
   network 192.168.0.0 255.255.255.192
   default-router 192.168.0.3
!
ip dhcp pool VLAN120
   network 192.168.0.64 255.255.255.192
   default-router 192.168.0.67
!
ip dhcp pool VLAN140
   network 192.168.0.128 255.255.255.192
   default-router 192.168.0.131
!
ip dhcp pool VLAN160
   network 192.168.0.192 255.255.255.192
   default-router 192.168.0.195
!
ip dhcp pool VLAN180
   network 192.168.1.0 255.255.255.192
   default-router 192.168.1.3
!
ip dhcp pool VLAN190
   network 192.168.1.64 255.255.255.192
   default-router 192.168.1.68
!
ip dhcp pool VLAN999
   network 192.168.1.128 255.255.255.192
   default-router 192.168.1.131
!
```

### Explication des différentes commandes : 

#### ```ip dhcp excluded-address``` :

La commande ```ip dhcp excluded-address``` est utilisée pour exclure certaines adresses IP d'un pool DHCP. Ces adresses exclues ne seront pas attribuées automatiquement par le serveur DHCP, car elles peuvent être réservées pour des périphériques spécifiques, tels que des serveurs, des imprimantes ou des équipements réseau. Elle permet alors d'exclure la plage d'adresse de nos vlans dans notre cas.

```ip dhcp excluded-address <adresse_début> [adresse_fin]```

```<adresse_début>``` : Adresse IP de début de la plage à exclure. 
```[adresse_fin] (optionnel) ``` : Adresse IP de fin de la plage à exclure. 

Si une seule adresse est spécifiée, seule cette adresse sera exclue.

#### Exemple d'utilisation : 

```ip dhcp excluded-address 192.168.0.0 192.168.0.3```

Elle exclue l'adresse IP du sous réseau du vlan100 et l'adresse IP du vlan100 sur les SW1, SW2, SWR1.

```ip dhcp pool ``` :

La commande ip dhcp pool est utilisée sur un équipement réseau pour créer et configurer un pool DHCP. Un pool DHCP est une plage d'adresses IP qui sera attribuée dynamiquement aux clients du réseau par le serveur DHCP.

Cette commande est souvent combinée avec d'autres pour définir des informations supplémentaires comme la passerelle par défaut, les serveurs DNS et la durée du bail.

```ip dhcp pool <vlanX> ```

```<vlanX> ```: On indique le vlan dont on souhaite configuré le DHCP


#### Exemple d'utilisation :

```ip dhcp pool VLAN100```

Permets de créer et de configurer le pool DHCP pour le vlan100

```network```

La commande network est utilisée dans la configuration d'un pool DHCP sur un équipement réseau pour définir la plage d'adresses IP que le serveur DHCP peut attribuer aux clients.

Elle précise le réseau IP et son masque de sous-réseau, permettant au serveur DHCP de savoir quelles adresses sont disponibles pour les clients. Les adresses exclues (via la commande ip dhcp excluded-address) ne seront pas attribuées, même si elles se trouvent dans ce réseau.

```network <adresse_réseau> <masque_sous_réseau>```
```
<adresse_réseau> : L’adresse réseau qui définit le sous-réseau.
```
```
<masque_sous_réseau> : Le masque de sous-réseau.
```

#### Exemple d'utlisation :

```network 192.168.0.0 255.255.255.192```

Définie l'IP d'un appareil branché sur le Vlan100 en tenant compte des adresse utilisé pour le Vlan sur les différents Switch et des autres apparareils connecté au vlan100

```default-router```

La commande default-router est utilisée dans la configuration d'un pool DHCP sur un équipement réseau (routeur ou commutateur compatible DHCP) pour spécifier l’adresse IP de la passerelle par défaut.

Cette adresse sera attribuée aux clients DHCP, permettant à ces derniers de communiquer avec des réseaux situés en dehors de leur sous-réseau local.

```default-router <passerelle>```

```<passerelle>``` : On indique la paserelle que l'on souhaite utilisé.

#### Exemple d'utilisation : 

```default-router 192.168.0.3 ```

Definie l'IP du vlan100 sur le SWR1 comme passerelle du vlan.

Groupe 1 TP2.2
