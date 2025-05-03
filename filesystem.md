# Filesystem

## Exploration de l’arborescence

### Racine

Affichez le contenu de `/` avec `ls -l /`

Via des recherches, indiquez à quoi servent les différents répertoires à la racine.

**Quelle différence y a-t-il entre `/bin` et `/usr/bin` ?**

### Parcours de l’arborescence

Explorez `/home`, `/etc`, `/var/log`, et `/tmp`.

**Que contiennent ces dossiers ?**

Utilisez `tree` (si installé) ou `find` pour afficher l’arborescence de `/etc` (limite à 2 niveaux) : `find /etc -maxdepth`.

**Que pouvez-vous en déduire ?**

Via commandes, trouvez la taille totale utilisée, ainsi que la taille libre.

## Manipulations de base

### Création de structure de fichiers

Créez un répertoire `/tmp/testfs`

Dans ce répertoire, créez :
- un sous-répertoire `docs`
- un fichier texte `info.txt`
- un lien symbolique (nommé `pswd`) vers `/etc/passwd`

**Que contient ce dernier fichier ?**

Affichez les détails avec `ls -l` sur `/tmp/testfs`

**Que constatez-vous ? Analysez chaque ligne pour décrire à quoi correspondent les différents éléments.**

### Types de fichiers

Utilisez file sur les éléments suivants et notez les résultats :
- `/etc/init.d/cron`
- `/var/log/lastlog`
- `/home/pi/Images`
- `/tmp/testfs/pswd`

## Permissions et droits

### Lecture des droits

Vérifiez les permissions d’un fichier des éléments suivants :
- `/root`
- `/home/pi`
- `/var/log`
- `/tmp`
- `/home/pi/.profile`

Pour chaque résultat, donnez les informations, droits, propriétaires et groupes.

### Modification des droits

Changez les permissions de `info.txt` pour que seul l’utilisateur `pi` puisse lire et écrire.

Créez un nouveau fichier `.secret` dans le répertoire `/home/pi` et changez le propriétaire du fichier pour `root`.

Basculez en mode root puis faites un `cd`.

**Que constatez-vous ? Où êtes-vous ?**

## Fichiers spéciaux

### Fichiers de périphériques

Listez quelques fichiers dans `/dev`.

**D'après-vous, à quoi servent-ils ?**

**À quoi servent /dev/null et /dev/zero ?**
