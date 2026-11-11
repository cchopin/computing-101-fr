Maintenant, apprenons à la calculatrice à multiplier !

La multiplication a sa propre instruction : `imul`.
De même qu'on a utilisé `add` pour `'+'` et `sub` pour `'-'`, on utilisera `imul` pour `'*'`.
Elle a déjà été rencontrée dans `atoi-two-digits`, où `imul rax, 10` multipliait le total courant par dix ; ici, elle multiplie les deux opérandes de la même façon.

```
"6"   ->  atoi  ->   6
"7"   ->  atoi  ->   7
6  *  7  =  42
42   ->  itoa  ->  "42"
```

Ajouter une troisième branche sur l'octet de l'opérateur : s'il vaut `'*'`, effectuer `imul` sur les opérandes ; `'+'` et `'-'` fonctionnent comme avant, et tout autre opérateur fait toujours quitter avec un code de sortie non nul.

Une difficulté du shell : `*` a un sens particulier pour le shell (il se développe en noms de fichiers). Il faut donc le mettre entre guillemets lorsqu'on lance le programme à la main : `./prog 6 '*' 7` ou `./prog 6 "*" 7`.
Le caractère reçu par le programme reste un simple `'*'`.

Construire et soumettre comme précédemment :

```console
hacker@dojo:~$ as -o prog.o prog.s
hacker@dojo:~$ ld -o prog prog.o
hacker@dojo:~$ ./prog 6 '*' 7
42
hacker@dojo:~$ /challenge/check prog
```

Ajouter la branche de multiplication et afficher le produit.
