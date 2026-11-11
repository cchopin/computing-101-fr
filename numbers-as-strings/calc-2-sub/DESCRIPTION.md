Maintenant, apprenons à la calculatrice à soustraire !

Ajoutez une deuxième branche sur l'octet de l'opérateur : s'il vaut `'-'`, faites `sub` (soustrayez) l'opérande de droite à celui de gauche au lieu d'additionner.
Tout le reste reste le même branchement que vous avez déjà écrit : `'+'` additionne, `'-'` soustrait, et tout autre opérateur fait toujours quitter avec un code de sortie non nul.

Un seul point est à surveiller : une différence peut être **négative**.

```
"3"   ->  atoi  ->   3
"10"  ->  atoi  ->  10
3  -  10  =  -7
-7   ->  itoa  ->  "-7"
```

C'est justement le cas que votre `itoa` avec signe gère déjà : il écrit le `'-'` en tête et la grandeur à votre place.
Transmettez donc directement la différence au même `itoa` que précédemment, et le signe se gère tout seul.

Construisez et soumettez comme précédemment :

```console
hacker@dojo:~$ as -o prog.o prog.s
hacker@dojo:~$ ld -o prog prog.o
hacker@dojo:~$ ./prog 3 - 10
-7
hacker@dojo:~$ /challenge/check prog
```

Ajoutez la branche de soustraction et affichez le résultat signé.
