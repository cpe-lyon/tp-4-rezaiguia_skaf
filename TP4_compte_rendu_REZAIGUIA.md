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

1) Dans mon $HOME je crée un dossier test avec la commande *mkdir test* puis je crée dans ce dossier un fichier fichier avec la commande *touch fichier*.
Une fois crée nous voulons écrire des lignes de texte dans ce fichier. Pour cela nous entrons la ligne de commande suivantes: 
> cat > fichier 

On écrit ensuite des lignes de texte.
Afin de vérifier les droits sur test et sur fichier on utilise la commande *ls -l* qui permet d'afficher les droit d'un repertoire. 
Avant d'utiliser cette commande il faut s'assurer que nous nous trouvons dans le bon repertoire. Pour cela on utilise *cd*.


2) Pour retirer tous les droits sur le fichier on se place dans le repertoire test puis on utilise la ligne de commande suivante: 
> chmod 000 fichier

Ainsi on retire tous les droit rwx aux utilisateur y compris nous. Néanmoins, en tant que root il est possible de lire et d'écrire sur le fichier. Ces droits sur root ne peuvent pas être modifiés.


3) Je me redonne les droits sur le fichier grace a la commande * chmod 700 fichier*, puis on vérifie les droits sur fichier grace a la commande *ls -l* utilisée precedemment. On effectue ensuite la commande  echo "echo Hello" > fichier. 
Nous avons bien le droit d'écriture mais pas le droit d'exécution.

4) Lorsque nous exécutons la commande nous n'avons pas la permission. Lorsque nous rajoutons *sudo* la commande est bien exécutée.

5) On se place dans le repertoire test puis nous nous retirons les droits en lecture grace a la commande * sudo chmod u-r test* (la commande * chmod o-r test* retire les droits en lecture aux autres uniquement). On list le contenu du repertoire grace a la commande *ls*. 
En essayant d'affichier le contenu du fichier fichier, nous remarquons que nous n'avons pas la permission car nous nous sommes retiré les droits en lecture sur le dossier test, et le fichier fichier se trouve dans test.

6) On crée comme on a l'habitude de faire le repertoire sstest et le fichier nouveau. On retire les droits en écriture grace a la commande * sudo chmod a-w test* et *sudo chmod a-w nouveau* en s'assurant d'etre dans les bons repertoires a chaque fois. Il est maintenant impossible d'ecrire sur le fichier nouveau. On retablit le droit en ecriture du repertoire test en ecrivant *sudo chmod a+w test*. Il est impossible de modifier le fichier nouveau mais il est possible de le supprimer. 
On remarque donc que pour modifier le fichier qui se trouve dans un repertoire il est essentiels de donner les droits d'ecriture a ce meme fichier. Il n'est néanmoins pas indispensable de donner les droits en ecriture au fichier afin de le supprimer.

7) On se positionne dans le repertoire personnel grace a la commande *cd* puis on se retire les droits en execution du dossier test grace a la commande :
> sudo chmod a-x test

On tente de créer, supprimer, ou modifier un fichier dans le répertoire test, de se déplacer, d’en
lister le contenu, etc… A chaque fois nous n'avons pas la permission. 
On en deduit que pour toucher a un fichier il faut autoriser les droits sur le fichier. Lorsque nous avons les droits sur le fichier nous avons egalement les droits sur le dossier mais pas inversement.

8) Apres avoir retiré tous les droits au repertoire courant, il est impossible de modifier quoi que ce soit. Nous pouvons neanmoins nous deplacer dans le repertoire parent grace a la commande *cd ..*. Une explication possible est que, puisque nous ne pouvons pas travailler sur le repertoire courant, il est interessant de pouvoir au moins en sortir et repartir dans le repertoire parent.

9) On retablit les droits en execution du repertoire test grace a la commande * sudo chmod a+x test*. On attribut ensuite les droits en lecture pour un autre utilisateur au fichier fichier * sudo chmod g+r fichier*.

10) On definit un umask très restrictif qui interdit à quiconque à part nous l’accès en lecture ou en écriture, ainsi que la traversée de nos répertoires. On utilise la commande *umask 007* et on cree un nouveau fichier et un nouveau repertoire avec les commandes vu precedemment. On test nos droits grace a la commande *ls -l*. 
On remarque que pour le fichier le proprietaire a les droits en lecture et ecriture et les groupe et autres utilisateurs n'ont aucun droits. Pour le repertoire le proprietaire a tous les droits alors que le groupe et les autres n'ont aucun droits.


11) On definit un umask très permissif qui autorise tout le monde à lire nos fichiers et traverser nos répertoires, mais n’autorise que nous à écrire grace a la commande *umask 022* et en creant un nouveau fichier et un nouveau repertoire. On verifie les droits grace a *ls -l*.
On remarque pour le fichier: le proprietaire a les droits en lecture et ecriture, le groupe a les droits en lecture et les autres utilisateurs en lecture uniquement aussi. Pour le repertoire: le proprietaire a tous les droits, le groupe a les droits en execution et en lecture et les autres utilisateurs en lecture uniquement.


12) On definit un umask équilibré qui nous autorise un accès complet et autorise un accès en lecture aux
membres de votre groupe grace a la commande *umask 037* et en creant un nouveau fichier et un nouveau repertoire. On verifie les droits grace a *ls -l*.
On remarque pour le fichier que le proprietaire a les droits en lecture et en ecriture, le groupe a les droits en lecture uniquement et les autres utilisateurs n'ont aucun droit.
Pour le repertoire, le proprietaire a tous les droits mais le groupe et les autres utilisateurs n'en ont aucun.


13) On transcrit les commandes: 

- chmod u=rx,g=wx,o=r fic 
devient chmod 534 fic
- chmod uo+w,g-rx fic en sachant que les droits initiaux de fic sont r--r-x---
devient chmod 501 fic
- chmod 653 fic en sachant que les droits initiaux de fic sont 711
devient chmod u=rx, g=wx, o=r fic
- chmod u+x,g=w,o-r fic en sachant que les droits initiaux de fic sont r--r-x---
devient chmod u-x, g+r, o+w fic


14) On affiche les droits sur passwd * /etc/passwd*. Les droit sont lecture et ecriture pour le proprietaire et lecture uniquement pour le groupe et les autres utilisateurs. Seul le proprietaire peut modifier les mots de passe.

