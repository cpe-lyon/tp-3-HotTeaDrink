# Exercice 1

1. Il suffit d'utiliser 
```BASH
 sudo groupadd dev; sudo groupadd infra
 ```
2. Il suffit d'utiliser pour chaque utilasteur le commande
```BASH
sudo useradd -m -d /home/username <username>
```
3. Afin d'ajouter les utilisateurs, il suffit d'utiliser la commande suivante:
```BASH
sudo usermod -a -G <groupname> <username>
```
4. La première methode est d'afficher le contenu de **/etc/group**, on pourra y retrouver les groupes et leur membre sous ce format:
***infra:x:1003:dave,bob,charlie***  
La deuxième methode est d'afficher le contenu de **/etc/passwd**, on pourra y retrouver les utilisateurs et leur different gid permettant d'identifier leur appartenance.

5. Il suffit d'effectuer la commande suivante avec les privilèges administrateur:
```BASH
chgrp <groupname> /home/path/
```
6. Pour remplacer le groupe primaire d'un utilisateur il suffit de suivre le paterne suivant:
```BASH
usermod <username> -g <groupname>
```
7. Pour ajouter faire cela il faut creer une ACL. Apres avoir installé les ACL (si pas déja installées), il suffit d'executer differente commande suivant le model suivant:
```BASH
setfacl -m group:<groupname>:rw path/
```
8. Pour se faire il suffit de donner uniquement les droits d'écriture du dossier au propriétaire garce à la commande suivante:
```BASH
setfacl -m user::rw path/
```
9. Non car son mot de passe n'a pas été défini.

10. Après avoir mis un mot de passe en place et l'avoir activé, on peux désormé s'y connecter.

11. Il suffit de tapper la commande **id alice**, celle-ci va nous donner l'ensemble de ses uid et gid

12. Pour retrouver l'utilisateur en question, il suffit d'ajouter des paramètres a la commande id:
```BASH
id -nu <uid>
```
13. L'id du groupe **dev** est **1002**

14. Il s'agit encore du groupe **dev**

15. Il suffit d'entrer la commande suivante:
```BASH
sudo gpasswd -d <username> <groupname>
```
16. Il suffit d'entrer la commande suivante:
```BASH
sudo chage -E 2021-6-01 -m 5 -M 90 -W 14 -I 30 dave
```
17. On peut voir que l'utilisateur root a un shell **BASH** par defaut. On peut allez le vérifier en regardant le dernier élément de la ligne de l'utilisateur dans le fichier **/etc/passwd**.
```BASH
cat /etc/passwd | grep "root"
root:x:0:0:root:/root:/bin/bash
```
18. Dans de nombreuses variantes d'Unix, "nobody" est le nom conventionnel d'un identifiant d'utilisateur qui ne possède aucun fichier, ne fait partie d'aucun groupe privilégié et ne dispose d'aucune capacité à l'exception de celles de tous les autres utilisateurs. Il n'est normalement pas activé en tant que compte utilisateur, c'est-à-dire qu'il n'a pas de répertoire d'accueil ou d'identifiants de connexion assignés. Certains systèmes définissent également un groupe équivalent "nogroup". 

19. Par défaut, le mot de passe est garder en memoire pendant 15 minutes. Pour que sudo oublie le mot de passe il suffit de taper la commande **sudo -k** pour reset le timer effaçant ainsi le mot de passe en mémoire.

# Exercice 2

1. Suite a la commande suivante, tout les éléments sont crée:
```
cd $HOME; mkdir test; cd test; touch fichier
```
Le fichier **test** a les droits suivants: **drwxrwxr-x**
Le fichier **fichier** a quand à lui ceux-la: **-rw-rw-r--**

2. Pour supprimer tous les droits on peut utiliser la commande suivante:
```BASH
sudo chmod ugoa-rwx fichier
```
Même en tant qu'user **root**, la permission nous est refusé pour l'ouverture ou la modification. Ainsi nous pouvons définir des autorisation pour l'user root.

3. En ajoutant le droit d'écriture et d'execution, nous pouvons ajouter du contenu dans le fichier sans lire.

4. Nous pouvons executé le fichier cependant nous en avont la permission uniquement en root. Car seul root à les droits de lecture.

5. Quand on s'enleve les droits sur un dossier dans lequel on est, on ne peux plus savoir ce que contient le dossier ou executer les fichiers dont nous avons les droits. Ainsi nous pouvons déduire que nous pouvons stocker des fichiers ou tous le monde a les droits dans un fichier que seul un utilisateur ou un group a acces pour limité artificiellement l'acces de ceux-ci.

6. Nous pouvons déduire que le droit de modification d'un fichier depends de celui-ci mais qu'en revanche, le droit de suppression depends du dossier parent.

7. Les droits du dossier parent défini en parti les droits de modification, d'execution et de suppretion des fichiers dans celui-ci.

8. Tous les droit que l'ont posseide sur le répertoire courant depends des droits d'execution. Il est possible de retourner dans le dossier parent avec "**cd ..**", donc nous pouvons toujours faire appel a des fichiers dans un dossier ou nous n'avons pas les permissions. De plus la commande cd est une commande qui fait appel a la commande **chdir** vers un autre dossier. Ainsi la demande de permission d'execution de la commande se fait dans un autre dossier.

9. Il suffit d'executé la commande suivante:
```BASH
sudo chmod 720 fichier
```

10. Il suffit d'executé la commande suivante:
```BASH
umask 077
```
11. Il suffit d'executé la commande suivante:
```BASH
umask 022
```

12. Il suffit d'executé la commande suivante:
```BASH
umask 037
```
13. Voici les transcriptions:
```BASH
chmod u=rx,g=wx,o=r fic --> chmod 534
chmod uo+w,g-rx fic (r--r-x---) --> chmod 602
chmod 653 fic (711) -->  chmod u=rw,g=rx,o=wx fic
chmod u+x,g=w,o-r fic (r--r-x---) --> chmod 520
```

14. Nous pouvons remarquer que tous le monde peux l'executé et le lire, de plus le créateur du fichier a tout les droits et a mis en place le droit setuid ce qui indique que ce programme est exécuté avec les droits de son propriétaire. Le fichier /etc/passwd autorise tous le monde a le lire et seulement root a écrire dedans. Ainsi grace au droit setuid du programme passwd, tous le monde peux modifier son mot de passe.