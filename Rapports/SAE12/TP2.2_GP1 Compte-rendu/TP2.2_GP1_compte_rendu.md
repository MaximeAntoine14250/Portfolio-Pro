# Compte rendu du Projet SAÉ 12 - Mise en place d'une Infrastructure Réseau

Date de rédaction : 09/01/2025

### Membre du projet :

ANTOINE Maxime;
BRETONNIERE Martin;
COEURET Tristan;
DAIRIN Come et SCHER Florian.

### Contexte du projet :

La société Fibre&Company, spécialisée dans le déploiement de la fibre optique, nécessite la mise à niveau et la sécurisation de son infrastructure réseau en raison de l'augmentation de son effectif et de l'évolution des besoins internes. Ce projet a pour objectif de concevoir, déployer et tester une nouvelle architecture réseau répondant aux exigences de sécurité, de gestion des VLANs et de services informatiques internes.

### Sommaire

1. Plans IPV4
2. Configurations Switchs et du Routeur
3. Connexion des différents équipements
4. Configuration du Proxmox et des machines virtuelles (VM)
5. Configuration de l'accès à Internet et à la Box
6. Schémas Logique/physique

### Équipements nécessaires

- 2 ordinateurs et leurs périphériques ;
- 2 switch Cisco L2 2960X, nommés ici respectivement SW1 et SW2 ;
- 1 switch Cisco L3 3750, nommé ici SWR1 ;
- 1 routeur Cisco 2911, nommé ici R1 ;
- 2 câbles d'alimentation classiques ;
- 1 câble d'alimentation Schuko ;
- 2 câbles console ;
- Des câbles Ethernet RJ45 ;

## 1/ Plans IPV4

### Attendus :

- 60 machines par VLAN
- 1 VLAN = 1 sous-réseau
- 1 VLAN natif

### Adressage global :

- Adresse réseau globale : 192.168.0.0/16
- Sous-réseau utilisé : /26 (64 adresses par sous-réseau, 62 utilisables)
- (Magic number : 64)

### Table des sous-réseaux :

| VLAN | Description        | Adresse Réseau | Masque | Plage IP Utilisables          | Broadcast     |
|------|--------------------|----------------|--------|-------------------------------|---------------|
| 100  | Services logiciels | 192\.168.0.0    | /26    | 192\.168.0.1 - 192.168.0.62    | 192\.168.0.63  |
| 120  | Comptabilité       | 192\.168.0.64   | /26    | 192\.168.0.65 - 192.168.0.126  | 192\.168.0.127 |
| 140  | Administratif      | 192\.168.0.128  | /26    | 192\.168.0.129 - 192.168.0.190 | 192\.168.0.191 |
| 160  | Vente              | 192\.168.0.192  | /26    | 192\.168.0.193 - 192.168.0.254 | 192\.168.0.255 |
| 180  | Supervision        | 192\.168.1.0    | /26    | 192\.168.1.1 - 192.168.1.62    | 192\.168.1.63  |
| 190  | Admin. Info.       | 192\.168.1.64   | /26    | 192\.168.1.65 - 192.168.1.126  | 192\.168.1.127 |
| 999  | VLAN Natif         | 192\.168.1.128  | /26    | 192\.168.1.129 - 192.168.1.190 | 192\.168.1.191 |

### Notes :

- Chaque sous-réseau utilise un masque /26, ce qui fournit 64 adresses IP par VLAN, dont 62 utilisables.
- Le VLAN natif a été replacé dans la plage de sous-réseaux pour simplifier la gestion.

## 2/ Configurations des Switchs et du Routeur

Pour configurer tout switch du montage, il faut connecter ces derniers aux postes pour pouvoir les configurer à l'aide des câbles console ; en connectant le câble console du port console du poste, au port console du switch.

Puis, en ouvrant le logiciel Gtkterm sur les postes, il suffit de se connecter au switch en mode configuration. Enfin, il faut copier coller les différentes configurations des équipement ci-dessous dans le logiciel Gtkterm.

<https://unicloud.unicaen.fr/index.php/s/ebe8TwXHTmCzRC5>

## 3/ Connexion des différents équipements

### 3\.1 Connexion des Switchs  :

#### Liens :

Les Switchs SW1 et SW2 doivent être alimentés par des câbles d'alimentation classiques, puis doivent être reliés par des câbles console aux ordinateurs. Pour le SWR1, il doit être alimenté par un câble d'alimentation spécial (câble d'alimentation Schuko).

Le PC1 doit être relié au SW1 et le PC2 doit être relié au SW2.

Par la suite, le SWR1 doit être relié par des câbles Ethernet de débit minimum de 100Mb/s aux SW1 et SW2. Il ne doit être connecté à aucun poste.

Le câblage doit permettre l'agrégation de liens selon ce modèle :

| Équipement \[Lien\] 1 | Équipement \[Lien\] 2 | Groupe |
|---------------------|---------------------|--------|
| SWR1 \[Fa1/0/1-2\]    | SW1 \[Gi1/0/1-2\]     | 1      |
| SWR1 \[Fa1/0/3-4\]    | SW2 \[Gi1/0/1-2\]     | 2      |
| SW1 \[Gi1/0/3-4\]     | SW2 \[Gi1/0/5-6\]     | 3      |

**Voir la notice de Connexion des Switchs pour plus de précisions.**

#### SW1 & SW2 :

Les liens agrégés sont configurés en assignant les ports tagués à des channel-groups.

### 3\.2 Connexion du Routeur :

Le routeur R1 doit être alimenté par un câble d'alimentation du même type que le SWR1, un câble. Veuillez à bien mettre en marche le routeur à l'aide de l'interrupteur avant, sans lequel il ne fonctionnera pas.

Puis, il doit être relié au SWR1, pour fournir aux Switchs et aux postes un accès à la Box puis à Internet.

## 4/ Configuration du Proxmox et des machines virtuelles

### 4\.1 Configuration des VM

Les machines virtuelles ont été configurées à l'aide des paramètres ci-dessous :

• RAM => 8Gb

• Bios => OVMF UEFI

• Carte graphique => VirtioGPU

• Flash => 3 disques de 50GB ; 30GB ; 30GB :

```
◦ Disque 1 (etx4fs, sauf la partition "boot EFI" et swap)

    ▪ "boot EFI" => environ 500MB

    ▪ "racine" => / 40GB

    ▪ "partition d'échange" => "swap" => 6GB

    ▪ "fichiers temporaires" => /tmp => 4GB

◦ Disque 2

    ▪ "Données variables" => /var

◦ Disque 3

    ▪ "Données utilisateurs" /home
```

Ces paramètres ainsi instaurés, nous obtenons le partitionnement Proxmox suivant : 

![1029-771-max (2).png](.attachments.48709263/1029-771-max%20%282%29.png)

### 4\.2 Configuration du Proxmox

Pour configurer le Proxmox, il suffit de réaliser un câblage correct au niveau des Box Internet.

Pour la première Box, il faut tout d'abord se connecter de cette façon avec le port tagué et la baie : 

![porttagué.jpg](.attachments.48709263/porttagu%C3%A9.jpg)

![](.attachments.48454750/connexion.jpg)

Puis, il suffit de suffit de brancher nos postes sur les ports 0 à 8 de la baie (câbles branchés en bas à gauche de la première image ci-dessous) et sur les différents ports de la salle (le 4e groupe de ports en partant du haut) : 

![postes.jpg](.attachments.48709263/postes.jpg)

Après avoir branché nos postes de cette façon, nous gagnons accès à la baie "serveurs Proxmox" ainsi qu'à la Box 1 en se reliant directement sur un port tagué : 

![port_tagué2.jpg](.attachments.48709263/port_tagu%C3%A92.jpg)

Après s'être branché sur le port tagué, il faut nous relier à la baie "serveurs Proxmox" : 

![baie.jpg](.attachments.48709263/1000009042.jpg)

Enfin, il suffit de se connecter à la salle Brattain via la baie pour connecter notre montage.