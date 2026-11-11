Dans ce défi, on apprend à utiliser gdb pour regarder dans la mémoire d'un processus !

On peut examiner le contenu de la mémoire avec la commande paramétrée `x/<n><u><f> <address>` (*e***x**amine), où l'adresse est donnée par <address>. Dans ce format, `<u>` est la taille unitaire à afficher, `<f>` le format d'affichage, et `<n>` le nombre d'éléments à afficher. Les tailles valides sont `b` (1 octet), `h` (2 octets), `w` (4 octets) et `g` (8 octets). Les formats valides sont `d` (décimal), `x` (hexadécimal), `s` (chaîne de caractères) et `i` (instruction). L'adresse peut être donnée par un nom de registre, un nom de symbole ou une adresse absolue. On peut aussi fournir des expressions mathématiques pour désigner l'adresse.

Par exemple, `x/8i $rip` affiche les 8 instructions suivantes à partir de l'instruction courante. `x/16i main` affiche les 16 premières instructions de main. On peut aussi utiliser `disassemble main`, ou `disas main` en abrégé, pour afficher toutes les instructions de main. Autre exemple : `x/16gx $rsp` affiche les 16 premières valeurs de la pile. `x/gx $rbp-0x32` affiche la variable locale stockée à cet endroit de la pile.

Pour afficher les instructions avec la syntaxe assembleur correcte, utiliser la commande `set disassembly-flavor intel`.

Pour résoudre ce niveau, il faut déterminer la valeur aléatoire placée sur la pile (la valeur lue depuis `/dev/urandom`).
Réfléchir aux arguments de l'appel système read.

----
**DOCUMENTATION UTILE :**
- La commande [run](https://sourceware.org/gdb/current/onlinedocs/gdb#Starting) de gdb
- La commande [continue](https://sourceware.org/gdb/current/onlinedocs/gdb#Continuing-and-Stepping) de gdb
- La commande [info](https://sourceware.org/gdb/current/onlinedocs/gdb#Registers) de gdb
- La commande [print](https://sourceware.org/gdb/current/onlinedocs/gdb#Data) de gdb
- La commande [examine](https://sourceware.org/gdb/current/onlinedocs/gdb#Memory) de gdb
