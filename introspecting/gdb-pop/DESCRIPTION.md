Dans les niveaux précédents, le secret était caché dans le code du programme, sous la forme d'une instruction `mov` écrite en dur.
Cette fois, le secret vient de l'état du programme *à l'exécution* : c'est le nombre d'arguments (`argc`), qui se trouve sur la pile.

Le programme dépile cette valeur avec `pop rdi`, puis remet aussitôt `rdi` à zéro avant de quitter :

```text
pop    rdi          <- reads argc from the stack into rdi
mov    rdi,0x0      <- overwrites rdi with 0!
mov    rax,0x3c
syscall             <- exit(0) --- the secret is gone!
```

Le code est entièrement visible et rien n'est censuré, mais on ne peut pas déterminer le secret en lisant seulement le désassemblage, car `argc` dépend du nombre d'arguments avec lequel le programme est lancé.
Dans ce défi, gdb s'en charge à votre place. Plus tard, nous vous montrerons aussi comment définir les arguments du programme dans gdb !

Pour l'instant, il faut :
1. lancer le programme ;
2. avancer d'une instruction pour exécuter uniquement `pop rdi` ;
3. afficher avec `print` la valeur de `rdi` avant qu'elle ne soit écrasée ;
4. quitter gdb, puis soumettre la valeur avec `/challenge/submit-number`.
