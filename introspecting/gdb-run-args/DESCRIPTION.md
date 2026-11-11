Dans le niveau précédent, on a utilisé la commande `run` de gdb pour la première fois : `run` lance le programme et le laisse s'exécuter librement.
Mais que faire quand le programme a *besoin d'arguments* en ligne de commande pour fonctionner ?

En dehors de gdb, on passe des arguments en les tapant après le nom du programme :

```console
hacker@dojo:~$ /challenge/debug-me hello
```

Dans gdb, l'équivalent consiste à les passer à `run` :

```text
(gdb) run hello
```

Ce qui suit `run` devient `argv[1]`, `argv[2]`, et ainsi de suite pour le programme débogué, exactement comme si ces arguments avaient été tapés sur la ligne de commande.
gdb accepte aussi la forme courte `r` :

```text
(gdb) r hello
```

(Partout où la documentation de gdb écrit `run`, `r` fonctionne aussi.)

Dans ce défi, `/challenge/debug-me` n'affiche le flag que si on lui donne la chaîne `pwn` comme `argv[1]`.
Faites-le !
