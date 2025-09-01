# Documentation Projet SAÉ 12

Projet réalisé par ANTOINE Maxime, BRETONNIERE Martin, COEURET Tristan, DAIRIN Come et SCHER Florian.

## Configuration des VLAN

### Cheminement

1. Création des VLAN
2. Configuration
   - Ajout description
   - Ajout IP et masque /26
3. Assigner VLAN au ports d'accès
   - Port 5-12 -> VLAN natif
   - Port 13-24 -> autres VLAN
4. Copie de la configuration sur les autres switchs
   - Effectuer changements d'ip selon le plan ip

## Configuration sur SW1

### Création des VLAN :

```
SW1> enable
SW1# config t
SW1(config)# vlan 100,120,140,160,180,190,999
```

### Configuration :

*Se référer au plan ip*

```
SW1(config)# interface vlan 100
SW1(config-if)# description Services logiciels (hebergement des serveurs)
SW1(config-if)# ip address 192.168.0.1 255.255.255.192
```

```
SW1(config)# interface vlan 120
SW1(config-if)# description Comptabilite
SW1(config-if)# ip address 192.168.0.65 255.255.255.192
```

```
SW1(config)# interface vlan 140
SW1(config-if)# description Administratif
SW1(config-if)# ip address 192.168.0.129 255.255.255.192
```

```
SW1(config)# interface vlan 160
SW1(config-if)# description Vente
SW1(config-if)# ip address 192.168.0.193 255.255.255.192
```

```
SW1(config)# interface vlan 180
SW1(config-if)# description Supervision
SW1(config-if)# ip address 192.168.1.1 255.255.255.192
```

```
SW1(config)# interface vlan 190
SW1(config-if)# description Administration Informatique
SW1(config-if)# ip address 192.168.1.65 255.255.255.192
```

```
SW1(config)# interface vlan 999
SW1(config-if)# description Vlan natif
SW1(config-if)# ip address 192.168.1.129 255.255.255.192
```

### Assigner VLAN au ports

On assigne le VLAN natif au ports

```
SW1(config)# interface range Gi1/0/5-12
SW1(config-if)# switchport mode access
SW1(config-if)# switchport access vlan 999
```