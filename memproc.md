# Gestion de mémoire et processus

> [!Important]
> L'ensemble de cette activité se situe DANS WSL.
> Vous devez saisir vos commandes dans un WSL.

## Installation

Installez le minimum nécessaire pour l'activité, à savoir `build-essential`, `gcc` et `valgrind`.

## Ecriture du programme de base

Créez votre programme en C nommé `memtest.c` à la racine de votre répertoire utilisateur.

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
    size_t size = 10 * 1024 * 1024; // 10 MB
    char *buffer = malloc(size);

    if (buffer == NULL) {
        perror("malloc");
        return 1;
    }

    memset(buffer, 'A', size);
    printf("Mémoire allouée et remplie : %zu octets\n", size);

    getchar();
    free(buffer);
    return 0;
}
```

Ensuite, compilez : `gcc -o memtest memtest.c`.

Testez votre programme.

## Exploration de la mémoire

Via une seconde ligne de commande ouverte sur le coté, vérifiez, via top ou htop, la consommation mémoire de votre programme.

Gardez les deux lignes de commande côte à côte et faites un `top -p <pid>` en remplaçant `<pid>` par le PID de memtest.

**Que constatez-vous ?**

Effectuez un saut de ligne dans la ligne de commande de memtest pour l'arrêter. **Que se passe-t-il ?**

Lancez le programme via strace : `strace ./memtest`.

> [!Note]
> [strace](https://strace.io/) est un programme de tracé des appels mémoire effectués.
> Par défaut, il affiche tous les appels systèmes effectués par le programme passé en paramètre.

Effectuez un saut de ligne pour débloquer le programme.

Relancez `strace` avec le paramètre `-e trace=memory` afin de filtrer uniquement les appels mémoire.
En utilisant `man` sur les appels systèmes, vous pouvez obtenir davantage d'informations sur ces appels.

**Via `strace`, affichez le nombre de chaque appel système.**

L'outil `pmap` est un autre outil d'analyse mémoire.
Il affiche la "carte" mémoire affectée, qui en est l'auteur, et quels sont les droits sur ces parties mémoires.

Après avoir repéré le PID de votre memtest, réalisez un `pmap -x` sur le PID concerné.

> [!Tip]
> Sur Linux, tout est fichiers.
> Tout ce qui a trait aux processus en cours est dans le dossier /proc, avec chaque processus étant un dossier dont le nom est le PID du processus.
> Puis, dans ce dossier, se trouve de nombreux éléments propres au processus, dont le fichier `maps`.

Affichez, via `cat`, la `map` du processus de memtest.

> [!Tip]
> Chaque ligne est une région de mémoire virtuelle "contigue" dans un processus.
> Chaque ligne comporte les champs suivants:
> - address : l'adresse mémoire de début
> - perms : les permissions accordées
> - offset : si cette portion provient d'un fichier (par exemple via mmap ou autres appels / librairies), c'est l'endroit d'où démarre la mémoire
> - device : l'offset DANS le fichier concerné
> - inode : l'inode du fichier concerné
> - pathname : le fichier concerné, peut être une région spéciale comme [heap], [stack]...

## Fuite mémoire : Valgrind

Créez un nouveau fichier `oops.c` contenant le code suivant :

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

void leak() {
    char *ptr = malloc(1024);
    strcpy(ptr, "Fuite mémoire !");
    printf("%s\n", ptr);
}

int main() {
    leak();
    return 0;
}
```

Puis compilez-le en mode debug : `gcc -g -o oops oops.c`

Ce programme.. "fonctionne" ! Mais il présente une fuite mémoire. Analyons-le avec valgrind : `valgrind --leak-check=full ./oops`

