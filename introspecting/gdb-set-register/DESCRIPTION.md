Dans le niveau précédent, on a utilisé `print` pour lire la valeur d'un registre.
gdb peut aussi modifier un registre pendant que le programme est arrêté.

La commande `set` affecte une nouvelle valeur à un registre :

```gdb
(gdb) set $rax = 42
```

Comme avec `print`, faites précéder le nom du registre de `$`.

Dans ce défi, il faut :

1. lancer le programme avec `starti`, puis avancer d'une instruction ;
2. placer `1337` dans `rdi`, puis avancer encore d'une instruction ;
3. afficher le nombre secret résultant, puis le soumettre avec `/challenge/submit-number`.

Lancer `gdb /challenge/debug-me` et modifier ce registre !
