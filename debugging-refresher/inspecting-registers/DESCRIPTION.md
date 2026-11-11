Dans ce défi, on apprend à afficher la valeur des registres.

On peut voir les valeurs de tous les registres avec `info registers`. On peut aussi afficher la valeur d'un registre précis avec la commande `print`, ou `p` en abrégé. Par exemple, `p $rdi` affiche la valeur de $rdi en décimal. On peut afficher cette valeur en hexadécimal avec `p/x $rdi`.

Pour résoudre ce niveau, il faut déterminer la valeur aléatoire actuelle du registre r12, en hexadécimal.

Comme précédemment, lancez le défi, exécutez la commande `run` de gdb, puis suivez les consignes.
Une fois la valeur affichée, pensez à `continue` pour passer à l'étape suivante du défi !

----
**DOCUMENTATION UTILE :**
- La commande [run](https://sourceware.org/gdb/current/onlinedocs/gdb#Starting) de gdb
- La commande [continue](https://sourceware.org/gdb/current/onlinedocs/gdb#Continuing-and-Stepping) de gdb
- La commande [info](https://sourceware.org/gdb/current/onlinedocs/gdb#Registers) de gdb
- La commande [print](https://sourceware.org/gdb/current/onlinedocs/gdb#Data) de gdb
