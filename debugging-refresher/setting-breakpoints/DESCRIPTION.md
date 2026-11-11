Une part essentielle de l'analyse dynamique consiste à amener le programme dans l'état qui vous intéresse.
Jusqu'ici, les défis posaient automatiquement les points d'arrêt à votre place, pour suspendre l'exécution aux états intéressants.
Il est important de savoir le faire soi-même.

Il existe plusieurs façons de faire avancer l'exécution du programme.
La commande `stepi <n>`, ou `si <n>` en abrégé, avance d'une instruction.
La commande `nexti <n>`, ou `ni <n>` en abrégé, avance d'une instruction en sautant les appels de fonctions.
Le paramètre `<n>` est facultatif, et permet de faire plusieurs pas à la fois.
La commande `finish` termine la fonction en cours d'exécution.
La commande paramétrée `break *<address>` pose un point d'arrêt à l'adresse indiquée.
Vous avez déjà utilisé la commande `continue`, qui poursuit l'exécution jusqu'au prochain point d'arrêt.

Pendant le pas à pas, il peut être utile d'afficher en permanence certaines valeurs.
Plusieurs méthodes existent.
La plus simple est la commande paramétrée `display/<n><u><f>`, dont le format est exactement celui de `x/<n><u><f>`.
Par exemple, `display/8i $rip` affiche toujours les 8 instructions suivantes.
De même, `display/4gx $rsp` affiche toujours les 4 premières valeurs de la pile.
Autre option : la commande `layout regs`. Elle fait passer gdb en mode TUI et affiche le contenu de tous les registres, ainsi que les instructions voisines.

Pour résoudre ce niveau, il faut déterminer une suite de valeurs aléatoires placées sur la pile.
Comme précédemment, `run` lance le programme, mais il l'interrompt : il faut alors poursuivre son exécution avec précaution.

Entraînez-vous à combiner `stepi`, `nexti`, `break`, `continue` et `finish`, pour bien comprendre ces commandes.
Elles sont toutes indispensables pour naviguer dans l'exécution d'un programme.

----
**DOCUMENTATION UTILE :**
- La commande [run](https://sourceware.org/gdb/current/onlinedocs/gdb#Starting) de gdb
- La commande [continue](https://sourceware.org/gdb/current/onlinedocs/gdb#Continuing-and-Stepping) de gdb
- La commande [info](https://sourceware.org/gdb/current/onlinedocs/gdb#Registers) de gdb
- La commande [print](https://sourceware.org/gdb/current/onlinedocs/gdb#Data) de gdb
- La commande [examine](https://sourceware.org/gdb/current/onlinedocs/gdb#Memory) de gdb
- La commande [break](https://sourceware.org/gdb/current/onlinedocs/gdb#Set-Breaks) de gdb
- La commande [display](https://sourceware.org/gdb/current/onlinedocs/gdb#Auto-Display) de gdb
- Les [différentes commandes de pas à pas](https://sourceware.org/gdb/current/onlinedocs/gdb#Continuing-and-Stepping) de gdb (toute cette section)

**NOTE :**
Ce défi vous demande de *lire* et de *comprendre* de l'assembleur !
Pas d'inquiétude : cette compétence vous servira beaucoup plus tard dans pwn.college.
