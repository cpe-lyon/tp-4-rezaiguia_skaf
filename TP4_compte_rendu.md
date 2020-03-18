REZAIGUIA Sarah

# TP 4 - Utilisateurs, groupes et permissions

Exercice 1: Gestion des utilisateurs et des groupes

1) On commence par créer deux groupe 1 et 2 grâce a la commande : sudo addgroup groupe1 et sudo addgroup groupe2.
Sudo permet d'executer en mode superutilisateur la commande, le mot de passe est donc requis.

2) On crée ensuite 4 utilisateurs u1 u2 u3 et u4 grâce a la commande: *sudo useradd u1*. On fait de même pour tous les utilisateurs.
Afin de demander la création de leur dossier personnel nous devons nous trouver dans le dossier home: *cd ~* puis nous créons chaque dossiers grâce a la commande mkdir.
On modifie par la suite le propriétaire de chaque dossier:
> chown [-R] u1:u1
> chown [-R] u2:u2
> chown [-R] u3:u3
> chown [-R] u4:u4
