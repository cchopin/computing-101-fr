Jusqu'ici, le seul échange du programme avec le monde extérieur consistait à modifier son code de retour au moment de `exit`.
Bien sûr, d'autres échanges sont possibles !

Dans ce module, on découvre l'appel système `write`, qui sert à `write` (écrire) une sortie dans le terminal !
L'aventure promet d'être passionnante : la logique de ce programme est à la fois aussi proche que possible du matériel (on écrit directement de l'assembleur x86 que le processeur comprend tel quel) et aussi proche que possible du système d'exploitation Linux (on déclenche directement des appels système).

---

## Fiche de cours (ajout, hors traduction)

*Cette fiche n'est pas une traduction. Elle est rédigée à partir de la diapositive « System Calls » de Yan Shoshitaishvili (Arizona State University), dans la série « Assembly Crash Course » (en anglais). Elle ne contient aucune solution de défi.*

### 1. Les appels système, c'est quoi ?

Un programme ne peut pas accéder seul au matériel : l'écran, le clavier, les fichiers et le réseau sont gérés par le système d'exploitation. Pour lui demander une action, il déclenche un **appel système**, avec l'instruction `syscall`.

Le fonctionnement est toujours le même :

- le numéro de l'appel système est placé dans `rax` ;
- les arguments sont placés, dans l'ordre, dans `rdi`, `rsi`, `rdx`, `r10`, `r8` puis `r9` ;
- l'instruction `syscall` déclenche l'appel ;
- la valeur de retour est placée dans `rax`.

Attention au quatrième argument : il passe par `r10`, et non par `rcx`.

Quelques appels courants, avec leur numéro sur x86-64 :

| Numéro | Appel | Rôle |
|---|---|---|
| 0 | `read` | Lit des octets depuis un descripteur de fichier. |
| 1 | `write` | Écrit des octets dans un descripteur de fichier. |
| 2 | `open` | Ouvre un fichier et renvoie un nouveau descripteur. |
| 60 | `exit` | Termine le programme avec un code de retour. |

Les diapositives donnent d'autres exemples : `fork` (crée un processus fils identique), `execve` (remplace le programme en cours) et `wait` (attend la fin d'un fils). Il existe plus de 300 appels système sous Linux. Leur interface est très stable, ce qui permet de les utiliser de la même façon pendant des années.

### 2. Les descripteurs de fichiers

Un **descripteur de fichier** (file descriptor, FD) est un numéro qui désigne un canal d'entrée ou de sortie ouvert par le processus. Chaque processus démarre avec trois descripteurs :

- **0 : entrée standard** (Standard Input), par où le programme reçoit des données, par exemple la saisie au clavier ;
- **1 : sortie standard** (Standard Output), où le programme affiche ses résultats normaux ;
- **2 : erreur standard** (Standard Error), où le programme affiche ses messages d'erreur.

L'appel `write` reçoit donc, en premier argument, le descripteur de sortie choisi. L'appel `read` reçoit le descripteur d'entrée.

### 3. Les données et les chaînes

Pour `read` et `write`, le deuxième argument est une **adresse mémoire** : là où se trouvent les données, ou là où on veut les stocker. Le troisième argument est le **nombre d'octets** à lire ou à écrire.

L'appel `read` renvoie dans `rax` le nombre d'octets réellement lus. Ce nombre peut être inférieur à celui demandé, par exemple si l'utilisateur ne tape que quelques caractères. Le programme peut donc utiliser cette valeur pour savoir quelle partie du tampon écrire ensuite.

Une **chaîne de caractères** (string) est une suite d'octets contigus en mémoire, terminée par un octet nul, `0x00`. Pour les appels qui attendent un chemin de fichier ou une chaîne, on construit cette suite octet par octet, à partir d'une adresse choisie (par exemple sur la pile, à partir de `rsp`). Chaque caractère a son code ASCII (voir la fiche du module précédent) : par exemple, la lettre `a` vaut `0x61`.

### 4. Les constantes

Certains appels système attendent des **constantes**, des valeurs à connaître pour chaque appel. Par exemple, `open` reçoit des indicateurs (flags) qui déterminent le mode d'ouverture : lecture seule, écriture seule ou lecture et écriture. Les valeurs exactes se trouvent dans les fichiers d'en-tête du langage C, ou dans la documentation de l'appel. Les diapositives proposent de les retrouver avec un petit programme C qui affiche la valeur d'une constante.

### 5. Quitter le programme

L'appel `exit` est le seul moyen propre de terminer un programme depuis l'assembleur : son premier argument est le code de retour, et son numéro est `60`. C'est le même appel qu'au module précédent.

### Pour aller plus loin

- La table des appels système pour x86-64, citée dans les diapositives comme ressource (en anglais) : https://blog.rchapman.org/posts/Linux_System_Call_Table_for_x86_64/
- Les pages de manuel Linux, accessibles avec `man 2 read`, `man 2 write`, etc., décrivent chaque appel et ses arguments.
