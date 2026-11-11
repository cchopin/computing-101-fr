Maintenant, apprenons à la calculatrice à multiplier !

La multiplication a sa propre instruction : `imul`.
De même que vous avez utilisé `add` pour `'+'` et `sub` pour `'-'`, vous utiliserez `imul` pour `'*'`.
Vous l'avez déjà rencontrée dans `atoi-two-digits`, où `imul rax, 10` multipliait votre total courant par dix ; ici, elle multiplie vos deux opérandes de la même façon.

```
"6"   ->  atoi  ->   6
"7"   ->  atoi  ->   7
6  *  7  =  42
42   ->  itoa  ->  "42"
```

Ajoutez une troisième branche sur l'octet de l'opérateur : s'il vaut `'*'`, effectuez `imul` sur les opérandes ; `'+'` et `'-'` fonctionnent comme avant, et tout autre opérateur fait toujours quitter avec un code de sortie non nul.

Une difficulté du shell : `*` a un sens particulier pour le shell (il se développe en noms de fichiers). Il faut donc le mettre entre guillemets lorsque vous lancez votre programme à la main : `./prog 6 '*' 7` ou `./prog 6 "*" 7`.
Le caractère reçu par votre programme reste un simple `'*'`.

Construisez et soumettez comme précédemment :

```console
hacker@dojo:~$ as -o prog.o prog.s
hacker@dojo:~$ ld -o prog prog.o
hacker@dojo:~$ ./prog 6 '*' 7
42
hacker@dojo:~$ /challenge/check prog
```

Ajoutez la branche de multiplication et affichez le produit.
