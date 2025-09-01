# Documentation Projet SAÉ 12

Projet réalisé par ANTOINE Maxime, BRETONNIERE Martin, COEURET Tristan, DAIRIN Come et SCHER Florian.

## Rapports de tests

**Test configuration Switchs**

### Test n°1

Après avoir appliquer toutes les configurations des Switchs et du routeur et appliquer tous les paramètres, nous pouvons tester notre montage.

D'après les photos ci-dessous, nous voyons que tous les ports sont actifs, grâce à la couleur verte de l'ampoule LED :


<img src="tests2.jpg" width=80%>
<img src="tests3.jpg" width=80%>

Cependant, le routage inter-Vlan n'a pas fonctionné. En regardant de nouveau dans nos configurations, il s'avère que nos routes ne permettaient pas d'accéder à toutes les adresses IP. Cela concluait en un "ping" qui n'atteignait pas sa cible.

En effet, nous avions essayé des "pings" entre nos postes qui étaient dans des mêmes VLan et les tests étaient positifs. Mais, en nous plaçant dans différents VLan, le "ping" n'aboutissait plus.

<img src="erreur.jpg" width=80%>