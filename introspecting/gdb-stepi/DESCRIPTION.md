Jusqu'ici, le secret a été lu dans le désassemblage du programme.
Mais que faire quand le secret est caché ?

Dans ce défi, le désassemblage est **censuré** : la valeur secrète est remplacée par `CENSORED`.
On ne peut donc pas *lire* la valeur dans le code, mais on peut encore *exécuter* ce code !
Quand le processeur exécute `mov rdi, CENSORED`, il charge la valeur secrète réelle dans le registre `rdi`.

Pour exécuter une seule instruction dans gdb, utiliser la commande `stepi` (**step** one **i**nstruction, c'est-à-dire « avancer d'une **i**nstruction », abrégée `si`) :

```gdb
(gdb) stepi
```

Une fois passée l'instruction `mov`, nous lisons le registre `rdi` à la place et affichons la valeur secrète.
La soumettre avec `/challenge/submit-number` !
