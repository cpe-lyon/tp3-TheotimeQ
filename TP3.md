**Quere Theotime 3ICS**

# Table des matières

[Exercice 1.Gestion des utilisateurs et des groupes](#Anch1)

[Exercice 2.Gestion des permissions](#Anch2)

# Exercice 1. Gestion des utilisateurs et des groupes <a id='Anch1'></a>

## 1. Utilisez la commande groupadd pour créer deux groupes dev et infra

```console
User@localhost:~$ sudo groupadd dev
User@localhost:~$ sudo groupadd infra
```

```console
User@localhost:~$ cat /etc/group
```

![](/TP2_IMG/TP3_1.png)

## 2. Créez ensuite 4 utilisateurs alice, bob, charlie, dave avec la commande useradd, en demandant la création de leur dossier personnel et avec bash pour shell

```console
User@localhost:~$ sudo useradd -m alice
User@localhost:~$ usermod --shell /bin/bash alice
User@localhost:~$ grep alice /etc/passwd
alice:x:1002:1004::/home/alice:/bin/bash
```

```console
User@localhost:~$ sudo useradd -m bob
User@localhost:~$ usermod --shell /bin/bash bob
User@localhost:~$ grep bob /etc/passwd
bob:x:1002:1004::/home/bob:/bin/bash
```

```console
User@localhost:~$ sudo useradd -m charlie
User@localhost:~$ usermod --shell /bin/bash charlie
User@localhost:~$ grep charlie /etc/passwd
charlie:x:1002:1004::/home/charlie:/bin/bash
```

```console
User@localhost:~$ sudo useradd -m dave
User@localhost:~$ usermod --shell /bin/bash dave
User@localhost:~$ grep dave /etc/passwd
dave:x:1002:1004::/home/dave:/bin/bash
```

## 3. Ajoutez les utilisateurs dans les groupes créés :

- alice, bob, dave dans dev
```console
User@localhost:~$ usermod -a -G dev alice
User@localhost:~$ usermod -a -G dev bob 
User@localhost:~$ usermod -a -G dev dave
User@localhost:~$ grep dev /etc/group
dev:x:1002:alice,bob,dave
```
- bob, charlie, dave dans infra
```console
User@localhost:~$ usermod -a -G infre bob
User@localhost:~$ usermod -a -G infra charlie
User@localhost:~$ usermod -a -G infra dave
User@localhost:~$ grep infra /etc/group
infra:x:1003:bob,dave,charlie
```

## 4. Donnez deux moyens d’afficher les membres de infra

```console
User@localhost:~$ grep infra /etc/group
infra:x:1003:bob,dave,charlie
```
```console
User@localhost:~$ getent group dinfra
infra:x:1003:bob,dave,charlie
```

## 5. Faites de dev le groupe propriétaire des répertoires /home/alice et /home/bob et de infra le groupe propriétaire de /home/charlie et /home/dave

```console
User@localhost:~$ sudo chgrp -R dev /home/alice
User@localhost:~$ sudo chgrp -R dev /home/charlie
User@localhost:~$ sudo chgrp -R dev /home/dave
```

On peut verifier avec : 

```console
User@localhost:~$ ls -l /home/
total 4
drwxr-x--- 2 alice   dev       57 Sep 15 08:20 alice
drwxr-x--- 3 ansible ansible   69 May 27 08:41 ansible
drwxr-x--- 2 bob     bob       57 Sep 15 08:30 bob
drwxr-x--- 2 charlie infra     57 Sep 15 08:33 charlie
drwxr-x--- 2 dave    infra     57 Sep 15 08:30 dave
drwxr-x--- 5 User    User    4096 Sep 15 08:23 User
```

## 6. Remplacez le groupe primaire des utilisateurs :

- dev pour alice et bob
```console
User@localhost:~$ sudo usermod alice -g dev
User@localhost:~$ sudo usermod bob -g dev
```

- infra pour charlie et dave
```console
User@localhost:~$ sudo usermod charlie -g infra
User@localhost:~$ sudo usermod dave -g infra
```

## 7. Créez deux répertoires /home/dev et /home/infra pour le contenu commun aux membres de chaque groupe, et mettez en place les permissions leur permettant d’écrire dans ces dossiers.

On creer les dossiers : 
```console
User@localhost:~$ sudo mkdir /home/dev
User@localhost:~$ sudo mkdir /home/infra
```

On donne les permissions : 
```console
User@localhost:~$ sudo chgrp -R infra /home/infra
User@localhost:~$ sudo chgrp -R dev /home/dev
User@localhost:~$ sudo chmod g+w /home/infra
User@localhost:~$ sudo chmod g+w /home/dev
```

## 8. Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer ou supprimer ce fichier ?

Ils faut enelever les permission à tout les autres. Pour cela :
```console
User@localhost:~$ sudo chmod 700 /home/alice
```

On peut verifier :
```console
User@localhost:~$ ls -l /home/
total 4
drwx------ 2 alice   dev       57 Sep 15 08:20 alice
...
```

## 9. Pouvez-vous ouvrir une session en tant que alice ? Pourquoi ?

```console
User@localhost:~$ su alice
Password:
```

On ne peut pas car on a pas le mot de passe d'alice

Mais on peut forcer avec : 
```console
User@localhost:~$ sudo su alice
alice@localhost:/home/User$
```

Etant donné qu'on a pas setup de mot de passe pour sa session la session devrait etre inactive..

## 10. Activez le compte de l’utilisateur alice et vérifiez que vous pouvez désormais vous connecter avec son compte.

On setup un mot de passe :
```console
User@localhost:~$ sudo passwd alice
```

Maintenant , on peut se connecter:
```console
User@localhost:~$ su alice
Password:
alice@localhost:/home/User$
```

## 11. Comment obtenir l’uid et le gid de alice ?

On peut le voir en faisant :
```console
User@localhost:~$ id alice
uid=1002(alice) gid=1004(alice) groups=1004(alice),1002(dev)
```

## 12. Quelle commande permet de retrouver l’utilisateur dont l’uid est 1003 ?

On peut regarder dans `/etc/passwd`
```console
User@localhost:~$ grep 1003 /etc/passwd
bob:x:1003:1005::/home/bob:/bin/sh
dave:x:1004:1003::/home/dave:/bin/sh
charlie:x:1005:1003::/home/charlie:/bin/sh
```

On peut aussi utiliser : 
```console
User@localhost:~$ id 1003
uid=1003(bob) gid=1005(bob) groups=1005(bob),1002(dev),1003(infra)
```
On voit dans les deux cas que bob à l'uid 1003

## 13. Quel est l’id du groupe dev ?

On voit avec la commande precedente  que l'ID de dev est 1002

On peut le faire aussi avec :
```console
User@localhost:~$ grep 1002 /etc/group
dev:x:1002:alice,bob,dave
```

## 14. Quel groupe a pour gid 1002 ? ( Rien n’empêche d’avoir un groupe dont le nom serait 1002...)

Le group `dev`

## 15. Retirez l’utilisateur charlie du groupe infra. Que se passe-t-il ? Expliquez.

```console
User@localhost:~$ gpasswd -d charlie infra
gpasswd: Permission denied.
```

Avec sudo :
```console
User@localhost:~$ sudo gpasswd -d charlie infra
Removing user charlie from group infra
```

## 16. Modifiez le compte de dave de sorte que :

— il expire au 1er juin 2021 :
```console
User@localhost:~$ sudo usermod -e 2021-06-01 dave
```

— il faut changer de mot de passe avant 90 jours
```console
User@localhost:~$ sudo usermod -e 2021-06-01 dave
```

— il faut attendre 5 jours pour modifier un mot de passe
```console
User@localhost:~$ sudo usermod -m dave
```

— l’utilisateur est averti 14 jours avant l’expiration de son mot de passe
```console
User@localhost:~$ sudo usermod -W dave
```

— le compte sera bloqué 30 jours après expiration du mot de passe
```console
User@localhost:~$ sudo chage -I 30 dave
```

On peut tout verifier avec :
```console
User@localhost:~$ sudo chage -l dave
Last password change                                    : Sep 15, 2022
Password expires                                        : never
Password inactive                                       : never
Account expires                                         : Jan 06, 2021
Minimum number of days between password change          : 0
Maximum number of days between password change          : 99999
Number of days of warning before password expires       : 7
```

## 17. Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?
 
```console
User@localhost:~$ grep root /etc/passwd
root:x:0:0:root:/root:/bin/bash
```
L'interpreteur de commandes de root est `bash`

## 18. Si vous regardez la liste des comptes présents sur la machine, vous verrez qu’il en existe un nommé nobody. A quoi correspond-il ?

```console
User@localhost:~$ grep nobody /etc/passwd
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
```

L'user nobody est le nom conventionnel d'un compte d'utilisateur à qui aucun fichier n'appartient, qui n'est dans aucun groupe qui a des privilèges et dont les seules possibilités sont celles que tous les "autres utilisateurs

## 19. Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ? Quelle commande permet de forcer sudo à oublier votre mot de passe ?

La commande sudo enregistre le mot de passe pendant 15mins. 

Pour forcer l'oubli , on peut : 
```console
User@localhost:~$ sudo -k
```

# Exercice 2. Gestion des permissions <a id='Anch2'></a>

## 1. Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier fichier contenant quelques lignes de texte. Quels sont les droits sur test et fichier ?

Pour creer les fichiers demandé , on fait : 

```console
User@localhost:~$ mkdir ~/test
User@localhost:~$ cd ~/test
User@localhost:~$ echo "Coucou maman" > fichier
User@localhost:~$ ls -l
total 4
-rw-rw-r-- 1 tquere tquere 13 sept. 20 20:44 fichier
User@localhost:~$ cd ..
User@localhost:~$ ls -l
total 40
..
drwxrwxr-x 2 tquere tquere 4096 sept. 20 20:44 '~/test'
..
```

Les droits sur le dossier sont : -rw-rw-r--
Les droits sur le fichier sont : drwxrwxr-x

## 2. Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’afficher en tant que root. Conclusion ?

Pour retirer les droits : 
```console
User@localhost:~$ chmod 000 ~/test
User@localhost:~$ chmod 000 ~/test/fichier
```

On peut quand meme modifier le fichier avec les droits de root.
Root à toujours tout les droits.

## 3. Redonnez vous les droits en écriture et exécution sur fichier puis exécutez la commande echo "echoHello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’un fichier s’il existe déjà. Que peut-on dire au sujet des droits ?

On redonne tout les droits : 
```console
User@localhost:~$ chmod 777 ~/test
User@localhost:~$ chmod 777 ~/test/fichier
```

On peut de nouveau modifier le fichier sans les droits root.

## 4. Essayez d’exécuter le fichier. Est-ce que cela fonctionne ? Et avec sudo ? Expliquez.

Le fichier est executable avec ou sans root , car on a donnée tout les droits sur le fichier.

## 5. Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le contenu du répertoire, puis exécutez ou affichez le contenu du fichier fichier. Qu’en déduisez-vous ? Rétablissez le droit en lecture sur test.

Il est impossible de lister ou d'affichier le contenu du repertoire si on enleve les droits dessus

```console
User@localhost:~$ ls test
ls: cannot open directory 'test': Permission denied
```

## 6. Créez dans test un fichier nouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au répertoire test le droit en écriture. Tentez de modifier le fichier nouveau. Rétablissez ensuite le droit en écriture au répertoire test. Tentez de modifier le fichier nouveau, puis de le supprimer. Que pouvezvous déduire de toutes ces manipulations ?

```console
User@localhost:~$ mkdir sstest
User@localhost:~$ touch nouveau
User@localhost:~$ chmod u-w sstest
User@localhost:~$ chmod u-w nouveau
```

Si on retire les droits en ecriture , on ne peut plus ecrire dans le fichier

## 7. Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test. Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’en lister le contenu, etc…Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires ?

On creer le dossier :
```console
User@localhost:~$ mkdir test
User@localhost:~$ chmod -x test
```
Creer :
```console
User@localhost:~$ touch test/truc
touch: cannot touch 'test/truc': Permission denied
```

Supprimer :
```console
User@localhost:~$ rm -rf test/truc
rm: cannot remove 'test/truc': Permission denied
```

Modifier :
```console
User@localhost:~$ touch test/truc
touch: cannot touch 'test/truc': Permission denied
```

Deplacer :
```console
User@localhost:~$ cd test
ls: cannot open directory 'test': Permission denied
```

On en conclu que les droits d'execution bloque toutes les action sur un directory

## 8. Rétablissez le droit en exécution du répertoire test. Positionnez vous dans ce répertoire et retirez lui à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des droits que l’on possède sur le répertoire courant ? Peut-on retourner dans le répertoire parent avec ”cd..” ? Pouvez-vous donner une explication ?

Modifier ou supprimer le fichier dépend des droits du fichier et non des droits du répertoire courant du fichier.

Si on enlève le droit d'execution d'un répertoire, il sera impossible de voir son contenu et donc non modifiable.







## 9. Rétablissez le droit en exécution du répertoire test. Attribuez au fichier fichier les droits suffisants pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.

## 10. Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.

## 11. Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos répertoires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.

## 12. Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture aux membres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.

## 13. Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vous
pourrez vous aider de la commande stat pour valider vos réponses) :

- chmod u=rx,g=wx,o=r fic

- chmod uo+w,g-rx fic en sachant que les droits initiaux de fic sont r--r-x---

- chmod 653 fic en sachant que les droits initiaux de fic sont 711

- chmod u+x,g=w,o-r fic en sachant que les droits initiaux de fic sont r--r-x---


## 14. Affichez les droits sur le programme passwd. Que remarquez-vous ? En affichant les droits du fichier
/etc/passwd, pouvez-vous justifier les permissions sur le programme passwd ?




Pour les plus rapides :

## 15. Access Control Lists (ACL) : suivez le tutoriel de cette page : https://doc.ubuntu-fr.org/acl.

## 16. Quotas disques : suivez le tutoriel de cette page : https://doc.ubuntu-fr.org/quota.
