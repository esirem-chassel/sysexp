# SSH et SSH-based

## Installation et configuration

Assurez-vous que votre Pi soit correctement installé, mis à jour, et que vous avez bien les identifiants de connexion.

## Activation de SSH

L'activation de SSH dans Pi se fait via une interface de configuration intégrée, dans le panneau de Configuration du Raspberry Pi, Interfaces, et activez SSH.

> [!Note]
> L'activation de ce service ne fait "que":
> - vérifier que les paquets nécessaires sont installés
> - activer le serveur SSH et le demon SSHD associé
> - ouvrir les ports concernés
> - autoriser la connexion pour le service (notemment auprès de apparmor)
>
> Dans un milieu de production "réel", ce sont autant d'éléments à vérifier !

> [!Important]
> ## SSH vs SSHD
> Il faut bien distinguer le **client** SSH du **serveur** SSH.
> Le démon de serveur ssh se nomme `sshd`. Sa configuration se situe dans `sshd_config`.
> Le client SSH n'a pas de démon associé. Sa configuration se situe dans `ssh_config`.

## Connexion locale

Testez la connexion SSH :
- depuis le PI courant localement : `ssh <user>@127.0.0.1` (remplacez `<user>` par l'utilisateur)
- depuis le PI courant à travers le réseau : `ssh <user>@<ip>` (remplacez `<ip>` par l'adresse IP de votre machine)
- depuis un autre poste vers le PI : `ssh <user>@<ip>`

Créez un fichier nommé `hello_<nom>.txt` contenant votre nom et prénom, en remplaçant `<nom>` par votre nom en minuscules sans accents, apostrophes ni espaces.

**Visualisez les permissions du dossier home. Que constatez-vous ?**

## SCP

Sur votre machine locale, créez un fichier "test.txt" contenant une chaîne quelconque.

> [!Tip]
> SCP ou SSH Copy permet de copier rapidement des fichiers d'un point A à un point B.
> Sa syntaxe est simple : `scp [<user>@<ip>:]<src> [<user>@<ip>:]<dest>`
> Ne paniquez pas, c'est plus aisé qu'il n'y paraît :
> - le premier argument est la source
> - le second argument est la destination
> - on peut préciser, devant la source et/ou la destination l'user et l'IP, comme pour une connexion SSH (auquel cas on rajoute un deux points)
> - si aucun identifiant SSH n'est précisé dans la source, la source est locale
> - si aucun identifiant SSH n'est précisé dans la destination, la destination est locale
> 
> Voyez donc SCP comme un simple `cp` mais capable de le faire à travers SSH !

**Envoyez le fichier `test.txt` depuis votre poste local vers le dossier `/tmp` de votre PI.**

**En saisissant la commande sur votre poste local, récupérez le fichier `hello_<nom>.txt` depuis le PI vers votre poste local.**

## Gestion utilisateurs

Connectez-vous en tant que superadmin.

> [!Tip]
> Pour rappel:
> - `su` permet de changer d'utilisateur. Si aucun utilisateur n'est précisé, on considère `root`.
> - `sudo` permet de demander au superadmin de réaliser une commande. Nécessite de saisir son propre mot de passe et d'être dans le groupe `sudoers`
> 
> Donc, `sudo su` permet de basculer en root !

### useradd vs adduser

Créez un nouvel utilisateur `test` via `useradd`. Vérifiez l'existence du dossier utilisateur `/home/useradd`. Que constatez-vous ?

Créez un nouvel utilisateur `trainee` via `adduser`. Que constatez-vous ?

### Opérations courantes

Supprimez l'utilisateur `test`.

Changez le mot de passe de l'utilisateur `trainee` : `passwd trainee`.

Testez la connexion SSH à cet utilisateur.

Ajoutez cet utilisateur au groupe `sudoers` : `usermod -aG sudo trainee`.

**Quels sont ces arguments dans usermod ? A quoi sert usermod ?**

Basculez sur l'utilisateur `trainee`.

### Permissions

Créez un dossier `/home/shared`. Quels sont ses droits ?

**Via `chmod` et `chown`, permettez à la fois votre utilisateur et `trainee` d'accéder en lecture, écriture (et exploration) à ce dossier, mais aucun droit pour les autres.**

Testez vos modifications !

## rsync

Rsync est l'évolution de SCP. Rsync permet une synchronisation "intelligente" de fichiers et dossiers.

Créez un répertoire `proj` contenant les fichiers de textes `a.txt`, `b.txt` et `c.txt`.

> [!Important]
> Rsync n'existe hélas pas de base sur Windows. Vous devrez testez via un Debian de l'école, votre système Linux/UNIX ou un autre PI.

Synchronisez ce répertoire avec le dossier `/home/<user>/backup` du PI : `rsync -avz proj/ <user>@<ip>:/home/<user>/backup`.

**Que constatez-vous ?**

**Modifiez un fichier et relancez le rsync. Qu'observez-vous ?**

## Clefs SSH

Rsync, SCP... sont des outils puissants qui se basent sur un protocole sûr.
Mais une faiblesse existe toujours : le mot de passe.
Ce mot de passe peut devenir un souci de sécurité, en plus d'être possiblement pénible à saisir, et de poser un souci sur différents utilitaires.

SSH permet une connexion par clefs.

> [!Caution]
> SSH fonctionne sur un principe de clefs publique/privée.
> Ne communiquez JAMAIS votre clef privée. Jamais.
> C'est toujours la clef publique que vous communiquez. La clef privée n'est utilisée que par SSH sur votre propre poste. Jamais ailleurs. Jamais.
> Si votre clef privée leak, d'une manière ou d'une autre, toute l'identité du serveur est compromise. Supprimez-la immédiatement.

Générez une paire de clefs SSH : `ssh-keygen -t ed25519`.

**L'argument -t précise l'algorithme de chiffrement utilisé. Quels autres grands algorithmes sont utilisés par SSH ? Présentez celui que vous considérez comme le plus connu.**

Copiez la clef publique vers la machine distante : `ssh-copy-id <user>@<ip>`.

> [!Note]
> La copie initie... une connexion SSH pour copier la clef.
> Ce qui signifie que vous avez bien besoin de vous connecter avec votre mot de passe à ce moment précis.

**Lancez une nouvelle connexion SSH. Si tout fonctionne comme prévu, aucun mot de passe ne vous sera demandé !**

## Cas complet

En utilisant rsync et/ou scp, créez un fichier `/home/shared/whoami.txt` dont le propriétaire est `trainee` et contenant votre nom/prénom, et récupérez-le sur un autre poste.

Vous devez utiliser des clefs SSH !
