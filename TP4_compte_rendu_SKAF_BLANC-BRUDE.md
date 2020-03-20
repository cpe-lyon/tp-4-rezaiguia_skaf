# TP 4 Utilisateurs, groupes et permissions
## Julien Blanc-Brude & Paul Skaf

### Exercice 1
1)  ``addgroup groupe1``et ``addgroup groupe2``

2) ```useradd -m u1 & useradd -m u2 & useradd -m u3 & useradd -m u4``

-m permet de créer le dossier utilisateur

3) ```bash
   usermod -a -G groupe1 u1 
   usermod -a -G groupe1 u2
   usermod -a -G groupe1 u4
   usermod -a -G groupe2 u2 
   usermod -a -G groupe2 u3 
   usermod -a -G groupe2 u4
   ``` 
4) On peut utiliser la commande suivante : ``grep groupe2 /etc/group | cut -d: -f4`` ou bien installer le paquet members qui permet de faire ça automatiquement : ``members groupe2``
 
 
5) 
``` bash
 chgrp groupe1 /home/u1
 chgrp groupe1 /home/u2
 chgrp groupe2 /home/u3
 chgrp groupe2 /home/u4
 ```
 
 6) 
 ```bash
usermod u1 -g groupe1
usermod u2 -g groupe1
usermod u3 -g groupe2
usermod u4 -g groupe2

 ```
 
 On peut vérifier dans quels groupes se trouvent nos utilisateurs avec la commande : ``groups utilisateurs``
 
 7) 
 ```
mkdir /home/groupe1
chgrp groupe1 /home/groupe1
chmod g+w /home/groupe1
mkdir /home/groupe2
chgrp groupe2 /home/groupe2
chmod g+w /home/groupe2

 ```
 
 8) Source pour cette question: https://stackoverflow.com/questions/1163294/changing-chmod-for-files-but-not-directories
 
 On doit faire la commande ``find . -type f -print0 | xargs -0 chmod 644``
 
 9) On ne peut pas se connecter en tant que U1 car il n'a pas encore de mot de passe.
 
 10) On set le password avec la commande ``passwd u1`` puis on switch d'utilisateur avec ``su u1``
 
 11) L'uid de u1 est 1001 et son guid est 1003, obtenus avec ``id u1``
 
 12) La commande ``cat /etc/passwd | cut -d: -f1,3 | grep 1003 | cut -d: -f1`` renvoie ``u3``
 
 cut -d: -f1,3 récupère les colonnes 1 (utilisateur) et 3 (uid) 
grep 1003 récupère les lignes contenant 1003 
cut -d: -f1 récupèrer la 1ère colonne et ainsi le nom d'utilisateur : ici u3)

13) ``cat /etc/group | cut -d: -f1,3 | grep groupe1 | cut -d: -f2`` renvoie 1001

14) ``cat /etc/group | cut -d: -f3 | grep -n 1002 | cut -d: -f1`` renvoie groupe2

15) 
```
groups u3
u3 : groupe2

gpasswd -d u3 groupe2
Removing user u3 from group groupe2

groups u3
u3 : groupe2

```

U3 est toujours dans le groupe 2. Le groupe n'a pas changé, cela peut s’expliquer par le fait que comme u3 ne possède pas qu’un seul groupe, il sera remis dans le groupe portant son nom. 

16) 
```
# chage -E 2020-06-1 u4
# chage -M 90 u4
# chage -m 5 u4
# chage -W 14 u4
# chage -I 30 u4

```

 -E permet de changer la date d'expiration du compte.
-M change la durée maximale de validité du mot de passe. 
-m change le délai avant de pouvoir modifier le mdp.
-W change le nombre de jour pour lequel l'utilisateur est averti avant expiration de son mdp. 
-I change le nombre de jours après lequel, suite à l'expiration de son mdp, l'utilisateur est bloqué.


Vérification : 


```
# chage -l u4
Last password change                                    : Mar 13, 2020
Password expires                                        : Jun 11, 2020
Password inactive                                       : Jul 11, 2020
Account expires                                         : Jun 01, 2020
Minimum number of days between password change          : 5
Maximum number of days between password change          : 90
Number of days of warning before password expires       : 14

```

17) 
 ```
 root@server:~# printenv SHELL
/bin/bash

```

18) C'est un utilisateur existant par défaut, ayant les droits minimum possibles sur le système. Avec cet utilisateur on peut se permettre de tester ce qu’on veut sans prendre de risques (services, outils).


 19) Par défaut sudo garde en mémoire le mdp pendant 15 minutes. 
 
 
 

### Exercice 2 
1) Le dossier "test" a les droits 755 et le fichier "fichier" a les droits 644.
Ce sont les droits de base quand on crée des fichiers et des dossiers sous linux 

2) On peut retirer tout les droits en faisant ``chmod 000``. Malgrès ça, on peut quand même accéder à ce fichier et le modifier en tant que root car les permissions n'affectent pas l'utilsiateur Root.

3) Comme on a de nouveaux les droits (écriture et éxéc) sur le fichier, on a remplacé le contenu de fichier par "Hello". Mais on ne peut pas le lire avec ``cat`` car nous n'avons pas les droits de lecture.

4) On a les droits d'éxécution, mais l'éxécution du fichier renverra ``Permission Denied`` car on ne peut pas éxécuter un fichier si on ne peut pas le lire ! Comme avant, Sudo permet de l'éxécuter car c'est le super-utilisateur.

5) ``chmod u-r ../test``. La commande ``ls`` ne fonctionne plus car nous ne pouvons plus lire donc lister les fichiers dans Test. On ne peut plus non plus lire le fichier "fichier".

6) Le comportement des différentes manipulations est finalement assez logique.

Si on possède le droit de lecture sur un dossier, on peut lister ses éléments, même si ses fichiers à l'intérieur ne nous donnent pas le droit de lecture. Par contre, on ne pourra pas accéder individuellement à chaque contenu des fichiers présents dans le dossier si nous n'avons pas le droit de lecture pour chacun de ses fichiers.

7) ``chmod u-x test`` : On ne peut plus se déplacer, le modifier, supprimer, etc.. 

8) Les répertoires fils et les fichiers contenus dans Test héritent de ses permissions. Une fois dans test, si on on supprime les droits d'éxécution, on se retrouve bloquer en éxécution dans ce dossier et nous ne pouvons plus rien faire, sauf remonter au répertoire parent avec cd.. car test se trouve dans ``/home/nom/test`` et nous avons le droit de nous déplacer dans ``/home/nom`` donc on peut revenir en arrière.

9) ``chmod 750 fichier`` enlève le droit en écriture mais autorise la lecture d'un autre membre de notre groupe

10) le ``umask 077`` empêche tout le monde d'écrire ou lire, à part nous.

11) le ``umask 022`` autorise la lecture la traversée des réprtoires à tout le monde mais permet l'écriture seulement à l'utilisateur.

12) le ``umask 037`` permet à l'utilisateur d'écrire, lire et éxécuter et n'autorise au groupe que la lecture.

13) ``chmod u=rx,g=wx,o=r fic`` = chmod 534 fic

``chmod uo+w,g-rx fic``= chmod 602 fic

``chmod 653 fic`` = chmod u-x,g+r,o+w fic

``chmod u+x,g=w,o-r fic`` = chmod 520 fic

14) Seul le superutilisateur root peut lire le fichier /etc/passwd.Les utilisateurs peuvent écrire leurs mots de passes dans le fichier mais sans le consulter. 
