# Processus

## Affichage des processus

Affichez les processus de l'utilisateur courant avec `ps`
**Que constatez-vous ? D'après vous, à quoi sert chaque process ?**

Affichez tous les processus du système avec `ps -ef`
**Que signifie chaque colonne ? Quel est le PID du processus bash en cours ?**

Utilisez `top` puis `htop` pour observer les processus en temps réel
**Quelles sont les différences entre les deux outils ?**

## Hiérarchie des processus

Utilisez la commande `pstree` pour afficher l’arborescence des processus.
**Quel est le processus "racine" initial ?**
**Quel est le processus parent (PPID) du shell courant ?**

## Manipulation des processus

### Création de processus

Ouvrez un terminal et lancez un bash réalisant une boucle sur un `sleep 60`.
Lancez un second terminal et affichez son PID avec `ps` ou `pgrep`.
Vérifiez que le programme reste actif avec `htop`.

### Suspension et reprise

Suspendez le programme via un `Ctrl+Z`.
**Que constatez-vous dans htop ? Quel est le statut du processus ?**
Listez les jobs via la commande `jobs`.
Reprenez la tâche en tâche de fond via `bg`.
**Que constatez-vous dans htop et dans jobs ?**
Reprenez la tâche en avant-plan via `fg`.

### Terminaison de processus

Utilisez `kill` pour arrêter le processus bash lancé précédemment (`kill <PID>`).
Relancez-le, puis envoyez un signal différent (ex : `SIGSTOP`, `SIGCONT`).
Obtenez la liste des signaux avec `kill -l`.
**Quelle est, d'après-vous, leur signification ?**
Confrontez vos hypothèses avec la documentation sur les signaux.
Tentez de kill le process `init`.
**Que se passe-t-il ?**

## Analyse des ressources

### Processus et charge système

Lancez plusieurs instances de `yes > /dev/null &`.
**D'après-vous, que fait cette commande ? Quels en sont les différents éléments ?**
Observez l’effet sur l’utilisation CPU via `top` ou `htop`.
Arrêtez-les proprement avec `kill` ou `pkill`
