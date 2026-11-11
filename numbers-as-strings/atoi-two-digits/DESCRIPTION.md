On sait décoder un chiffre avec `atoi_digit`.
Un nombre de deux chiffres se compose simplement de deux de ces chiffres, combinés selon leur *position* : dans `"42"`, le `4` occupe la position des dizaines et le `2` celle des unités, donc la valeur vaut `4 * 10 + 2 = 42`.
C'est l'algorithme que nous utiliserons dans ce niveau.

Ici, il faut écrire **deux** fonctions :

- `atoi_digit(s)`, exactement comme précédemment : la valeur du chiffre unique situé en `s`. On peut (et on doit !) réutiliser la solution du défi précédent.
- `atoi(s)`, qui reçoit un pointeur vers un nombre de *deux caractères* et renvoie sa valeur, en décodant chaque caractère avec `atoi_digit` puis en les combinant sous la forme `first * 10 + second`.

Ces deux fonctions sont appelées par le correcteur : elles doivent donc respecter la convention d'appel.
Si l'on utilise des *registres préservés par l'appelé* (callee-saved), il faut les restaurer correctement avant de retourner.
Et comme `atoi` *appelle* `atoi_digit`, il faut aussi gérer correctement les *registres non préservés* (caller-saved).

Comme précédemment, chaque fonction reçoit son argument dans `rdi` et renvoie son résultat dans `rax`.

Comment multiplie-t-on ?
L'instruction de multiplication de x86 est `imul`.
Elle a plusieurs formes, mais nous l'utiliserons comme `add` : `imul rax, 10` multiplie `rax` par `10` sur place (`rax = rax * 10`), ce qui permet de mettre le chiffre des dizaines à la bonne position en une seule instruction.
Bien sûr, `imul` peut utiliser d'autres registres que `rax` : par exemple, `imul rbx, 10` multiplie `rbx` par `10`.

Un dernier point !
Dans le code assembleur, il faut à la fois un `.global atoi_digit` *et* un `.global atoi` (ainsi que les fonctions correspondantes à ces étiquettes) pour que le correcteur puisse les trouver.
Construire et soumettre comme précédemment, avec les deux fonctions `atoi_digit` et `atoi` :

```console
hacker@dojo:~$ /challenge/check your-solve.so
```

Multiplier par dix, ajouter les unités, et le flag est obtenu.
