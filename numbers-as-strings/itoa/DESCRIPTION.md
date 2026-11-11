Maintenant, des nombres de n'importe quelle longueur.
C'est la même étape de division par 10 que dans le niveau précédent, simplement répétée : chaque `div` retire le chiffre le plus bas (le reste) et réduit le nombre (le quotient), et l'on continue jusqu'à ce que le quotient atteigne `0`, quel que soit le nombre de chiffres.

```
123:  123 % 10 = 3,  123 / 10 = 12
       12 % 10 = 2,   12 / 10 = 1
        1 % 10 = 1,    1 / 10 = 0   (stop)
```

Attention au piège : les chiffres sortent à l'**envers**, les unités d'abord (`3`, `2`, `1`), alors que nous les écrivons dans l'autre sens (`1`, `2`, `3`).
On ne peut donc pas simplement les ajouter au fur et à mesure.
Les solutions habituelles : mémoriser chaque chiffre au fur et à mesure et les écrire à l'envers (la pile est parfaite pour cela : `push` les chiffres au fur et à mesure qu'ils sortent, puis `pop` pour les écrire, et la logique LIFO les inverse gratuitement), ou les écrire dans le tampon en partant de la fin vers le début.

Et `0` est le même cas particulier que celui traité au niveau précédent : la boucle s'exécute zéro fois pour lui, il faut donc écrire simplement `"0"` à la main.

Écrire `itoa(value, buf)` pour toute `value` positive ou nulle (dans `rdi`, le tampon dans `rsi`) : écrire ses chiffres décimaux dans le tampon, et renvoyer dans `rax` le nombre de chiffres écrits.
Penser à `.global itoa`.

Compiler et soumettre comme précédemment :

```console
hacker@dojo:~$ as -o your-solve.o your-solve.s
hacker@dojo:~$ ld -shared -o your-solve.so your-solve.o
hacker@dojo:~$ /challenge/check your-solve.so
```

Inverser les chiffres et renvoyer la longueur.
