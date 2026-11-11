Dans le niveau précédent, nous lisions automatiquement la valeur du registre après l'exécution pas à pas.
Cette fois, c'est à faire soi-même !

Le désassemblage reste censuré, il faut donc :
1. lancer le programme avec `starti` ;
2. exécuter une instruction avec `stepi` (ou `si`) ;
3. lire soi-même le registre avec `print $rdi`.

La commande `print` affiche la valeur d'une expression.
Dans gdb, les noms de registres sont précédés de `$`, ce qui permet de lire `rdi` ainsi :

```gdb
(gdb) print $rdi
$1 = 1337
```

Soumettre ensuite la valeur avec `/challenge/submit-number`.
