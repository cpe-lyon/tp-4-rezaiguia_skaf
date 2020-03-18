REZAIGUIA Sarah

# TP 4 - Utilisateurs, groupes et permissions

## Exercice 1: Gestion des utilisateurs et des groupes

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


5) On veut changer l'appartenance d'un dossier à un groupe, on utilise dans ce cas là, la commande *chgrp*.

> sudo chgrp groupe1 /home/u1

> sudo chgrp groupe1 /home/u1

On fait de même pour /home/u3, /home/u4 pour le groupe2.


6) Pour remplacer le groupe primaire des utilisateurs on utilise la commande:

> sudo usermod groupe1 -g u1

> sudo usermod groupe1 -g u1

On fait de même dans groupe2 pour u3 et u4


7) On crée les deux répertoires /home/groupe1 et /home/groupe2 grâce à la commande mkdir *mkdir /home/groupe1* et *mkdir /home/groupe2*.
On met ensuite en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier associé grâce à la commande *chmod* qui permet de modifier les droits existants sur les fichiers et dossiers. 

> sudo chmod g+w /home/groupe1  , le g indique que c'est un groupe et le w le mode ecriture

>sudo chmod g+w /home/groupe2


8) Afin que dans ces dossiers seul le propriétaire d’un fichier ait le droit de renommer ou supprimer ce fichier on utilise la commande suivante:

> sudo chmod go-w /home/groupe1

>sudo chmod go-w /home/groupe2

Ainsi les autres utilisateurs pourront lire et executer le dossier mais pas le renomer ou le modifier.


9) Nous ne pouvons pas nous connecter en tant que u1 car nous n'avons pas attribuer à u1 un mot de passe. Le compte est inactif pour le moment.


10) Pour activer u1 il faut lui attribuer un mot de passe avec passwd par exemple de la manière suivante: 
> sudo passwd u1

On se connecte ensuite a u1 pour s'assurer qu'il est actif, on utilise la commande switch user: *su u1*.


11) Afin d'afficher l'uid et le gid de u1 on utilise la commande:
> id u1

On obtient alors : uid= 1001(u1) et gid=1001(groupe1)


12) L'utilisateur qui a pour uid 1003 est l'utilisateur u3.


13) L'id du groupe1 est donné grâce à la commande de la question 12. On obtient *groups=1001(groupe1)*.


14) Le groupe qui a pour guid 1002 est le groupe2.


15) On supprime l'utilisateur u3 grâce à la commande *userdel* ou *deluser*. Supprimer un utilisateur ne supprime pas son répertoire personnel, sa boîte aux lettres ou tout autre fichier possédé par l’utilisateur sur le système. Le groupe2 reste propriétaire de cet utilisateur.


16) Afin que u4 expire le 1er juin 2020 on exécute la commande suivante: 
> sudo chage -e 2020-06-01 u4

Afin que u4 doive changer de mot de passe avant 90 jours:
> chage -M 90 u4

Cette commande permet d'effectuer cette planification en faisant s'expirer le mot de passe tous les 90 jours avec l'option "-M".
ou

> sudo chage -d `date -d "90 days" +"%Y-%m-%d"` u4

Attendre 5 jours pour modifier un mot de passe:

> sudo chage -m 5 u4

- Utilisateur est averti 14 jours avant l’expiration de son mot de passe

> sudo chage -W 14 u4

-le compte sera bloqué 30 jours après expiration du mot de passe

> sudo chage -d `date -d "30 days" +"%Y-%m-%d"` u4


17) L'interpreteur de commandes (Shell) de l’utilisateur root est :
root:x:0:0:root:/root:/bin/bash
Cette ligne est obtenue grâce à la commande: *head -1 cat etc/passwd*.


18) L'utilisateur nobody a un shell mais il n'a aucune droits sur aucun fichier ou dossier. Il a seulement le droit d'exécuter en *su nobody* mais il n'aura aucune influence ni aucun impact sur le reste.


19) La commande *sudo* mémorise le mot de passe pendant 15 minutes. La commande pour terminer la session sudo avant la fin des 15 minutes est:
> sudo -k

## Exercice 2: gestion des permissions

