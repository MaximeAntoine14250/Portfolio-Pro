## Plan IP SAE 1.02

### Attendus :
- 60 machines par VLAN
- 1 VLAN → 1 sous-réseau
- 1 VLAN natif

### Adressage global :
- **Adresse réseau globale** : 192.168.0.0/16
- **Sous-réseau utilisé** : /26 (64 adresses par sous-réseau, 62 utilisables)
- **Incréments magiques** : 64

### Table des sous-réseaux :

| VLAN | Description           | Adresse Réseau  | Masque  | Plage IP Utilisables       | Broadcast      |
|------|-----------------------|-----------------|---------|----------------------------|----------------|
| 100  | Services logiciels    | 192.168.0.0     | /26     | 192.168.0.1 - 192.168.0.62 | 192.168.0.63   |
| 120  | Comptabilité          | 192.168.0.64    | /26     | 192.168.0.65 - 192.168.0.126| 192.168.0.127  |
| 140  | Administratif         | 192.168.0.128   | /26     | 192.168.0.129 - 192.168.0.190| 192.168.0.191  |
| 160  | Vente                 | 192.168.0.192   | /26     | 192.168.0.193 - 192.168.0.254| 192.168.0.255  |
| 180  | Supervision           | 192.168.1.0     | /26     | 192.168.1.1 - 192.168.1.62 | 192.168.1.63   |
| 190  | Admin. Info.          | 192.168.1.64    | /26     | 192.168.1.65 - 192.168.1.126| 192.168.1.127  |
| 999  | VLAN Natif            | 192.168.1.128   | /26     | 192.168.1.129 - 192.168.1.190| 192.168.1.191  |

### Notes :
- Chaque sous-réseau utilise un masque `/26`, ce qui fournit 64 adresses IP par VLAN, dont 62 utilisables.
- Le VLAN natif a été replacé dans la plage de sous-réseaux pour simplifier la gestion.

---

