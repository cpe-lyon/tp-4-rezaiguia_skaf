# TP 4 Utilisateurs, groupes et permissions
## Julien Blanc-Brude & Paul Skaf

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
