Dans ce défi, on entre dans le monde du réseau en créant un socket avec l'appel système [socket](https://man7.org/linux/man-pages/man2/socket.2.html).
Un socket est la brique de base de la communication réseau : c'est un point d'accès pour envoyer et recevoir des données.
Quand on appelle [socket](https://man7.org/linux/man-pages/man2/socket.2.html), on fournit trois arguments : le domaine (par exemple `AF_INET` pour IPv4), le type (par exemple `SOCK_STREAM` pour TCP) et le protocole (en général `0`, pour choisir la valeur par défaut).
Maîtriser cet appel est important, car il pose les bases de toutes les interactions réseau suivantes.

----
**NOTE :**
Dans la documentation, les arguments des appels système sont donnés sous forme de noms en majuscules.
Par exemple, on peut vouloir appeler `socket(AF_INET, SOCK_STREAM, 0)`, mais on ne peut pas simplement écrire `mov rdi, AF_INET` : `AF_INET` n'est pas une notion au niveau de l'assembleur.
Il faut trouver l'entier qui correspond à `AF_INET`.
Ces nombres ne figurent même pas dans les pages de manuel, mais ils existent sur la machine.
Il faut regarder dans le répertoire `/usr/include`.
Tous les fichiers d'en-tête généraux pour la programmation en C y sont placés. (Pour qui a écrit du C, il s'agit des fichiers inclus avec `#include <stdio.h>` : toutes les fonctions et constantes qu'ils définissent se trouvent quelque part dans ce répertoire.)
Comme le C est compilé en assembleur, ces nombres sont présents quelque part dans ce répertoire.
Plutôt que de les chercher à la main, on peut utiliser [grep](https://pwn.college/linux-luminarium/commands/) pour les trouver.
