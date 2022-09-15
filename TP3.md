**Quere Theotime 3ICS**

# Table des matières

[Exercice 1.Variables d'environnement](#Anch1)

[Exercice 2.Contrôle de mot de passe](#Anch2)

[Exercice 3.Expression rationelles](#Anch3)

[Exercice 4.Controle d'utilisateur](#Anch4)

[Exercice 5.Factorielle](#Anch5)

[Exercice 6.Le juste prix](#Anch6)

[Exercice 7.Statistique](#Anch7)

[Exercice 8.Bonus](#Anch8)

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

## 6. Remplacez le groupe primaire des utilisateurs :

- dev pour alice et bob
- infra pour charlie et dave

## 7. Créez deux répertoires /home/dev et /home/infra pour le contenu commun aux membres de chaque groupe, et mettez en place les permissions leur permettant d’écrire dans ces dossiers.

## 8. Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer ou supprimer ce fichier ?

## 9. Pouvez-vous ouvrir une session en tant que alice ? Pourquoi ?

## 10. Activez le compte de l’utilisateur alice et vérifiez que vous pouvez désormais vous connecter avec son compte.

## 11. Comment obtenir l’uid et le gid de alice ?

## 12. Quelle commande permet de retrouver l’utilisateur dont l’uid est 1003 ?

## 13. Quel est l’id du groupe dev ?

## 14. Quel groupe a pour gid 1002 ? ( Rien n’empêche d’avoir un groupe dont le nom serait 1002...)

## 15. Retirez l’utilisateur charlie du groupe infra. Que se passe-t-il ? Expliquez.

## 16. Modifiez le compte de dave de sorte que :

— il expire au 1
er juin 2021
— il faut changer de mot de passe avant 90 jours
— il faut attendre 5 jours pour modifier un mot de passe
— l’utilisateur est averti 14 jours avant l’expiration de son mot de passe
— le compte sera bloqué 30 jours après expiration du mot de passe

## 17. Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?
 
## 18. Si vous regardez la liste des comptes présents sur la machine, vous verrez qu’il en existe un nommé nobody. A quoi correspond-il ?

## 19. Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ? Quelle commande permet de forcer sudo à oublier votre mot de passe ?

# Exercice 2. Gestion des permissions <a id='Anch2'></a>

## 1. Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier fichier contenant quelques lignes de texte. Quels sont les droits sur test et fichier ?

## 2. Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’afficher en tant que root. Conclusion ?

## 3. Redonnez vous les droits en écriture et exécution sur fichier puis exécutez la commande echo "echoHello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’un fichier s’il existe déjà. Que peut-on dire au sujet des droits ?

## 4. Essayez d’exécuter le fichier. Est-ce que cela fonctionne ? Et avec sudo ? Expliquez.

## 5. Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le contenu du répertoire, puis exécutez ou affichez le contenu du fichier fichier. Qu’en déduisez-vous ? Rétablissez le droit en lecture sur test.

## 6. Créez dans test un fichier nouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au répertoire test le droit en écriture. Tentez de modifier le fichier nouveau. Rétablissez ensuite le droit en écriture au répertoire test. Tentez de modifier le fichier nouveau, puis de le supprimer. Que pouvezvous déduire de toutes ces manipulations ?

## 7. Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test. Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’en lister le contenu, etc…Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires ?

## 8. Rétablissez le droit en exécution du répertoire test. Positionnez vous dans ce répertoire et retirez lui à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des droits que l’on possède sur le répertoire courant ? Peut-on retourner dans le répertoire parent avec ”cd..” ? Pouvez-vous donner une explication ?

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
- 
## 14. Affichez les droits sur le programme passwd. Que remarquez-vous ? En affichant les droits du fichier
/etc/passwd, pouvez-vous justifier les permissions sur le programme passwd ?
Pour les plus rapides :

## 15. Access Control Lists (ACL) : suivez le tutoriel de cette page : https://doc.ubuntu-fr.org/acl.

## 16. Quotas disques : suivez le tutoriel de cette page : https://doc.ubuntu-fr.org/quota.
