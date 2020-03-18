REZAIGUIA Sarah

# TP 4 - Utilisateurs, groupes et permissions

Exercice 1: Gestion des utilisateurs et des groupes

1) On commence par créer deux groupe 1 et 2 grâce a la commande : sudo addgroup groupe1 et sudo addgroup groupe2.
Sudo permet d'executer en mode superutilisateur la commande, le mot de passe est donc requis.

2) On crée ensuite 4 utilisateurs u1 u2 u3 et u4 grâce a la commande: *sudo useradd u1*. On fait de même pour tous les utilisateurs.
Afin de demander la création de leur dossier personnel nous devons nous trouver dans le dossier home: *cd ~* puis nous créons chaque dossier grâce a la commande mkdir.
On modifie par la suite le propriétaire de chaque dossier:
> chown [-R] u1:u1

> chown [-R] u2:u2

> chown [-R] u3:u3

> chown [-R] u4:u4

3) Afin d'ajouter les utilisateurs dans les groupes existants nous utilisons la commande *usermod* afin d'ajouter un utilisateur existant à un groupe (secondaire) existant :
> usermod -a -G groupe1 u1

> usermod -a -G groupe1 u2

> usermod -a -G groupe1 u4

> usermod -a -G groupe2 u2

> usermod -a -G groupe2 u3

> usermod -a -G groupe2 u4

4) Afin d'afficher les membres des groupes on peut utiliser la commande :
> cat etc/group
Cette commande permet la lecture du fichier des groupes. 
Une deuxième possibilité est:
> grep groupe_à_vérifier /etc/group
groupe_à_vérifier peut donc être groupe1 ou groupe2 dans notre cas.
