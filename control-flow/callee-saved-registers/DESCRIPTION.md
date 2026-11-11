Les registres sauvegardés par l'appelant ont été pratiqués ; voyons maintenant ceux *préservés par l'appelé*.
Les registres préservés par l'appelé (`rbx`, `rbp`, `r12`, `r13`, `r14`, `r15`) doivent être restitués intacts quand la fonction appelée revient.
Quand une fonction utilise l'un d'eux, elle l'*emprunte* à celui qui l'a appelée, et elle doit le rendre exactement dans l'état où elle l'a trouvé.
C'est ce qui permet à un appelant de garder des valeurs durables dans `rbx`, `rbp` et `r12` à `r15` pendant un appel, en sachant qu'elles seront toujours là après.

La règle est la suivante : quand la fonction veut utiliser ces registres, *les sauvegarder (push) à l'entrée de la fonction, puis les restaurer (pop) avant le `ret`*.

Ce niveau place du côté de l'appelé.
La fonction `solve` est appelée comme `solve(check_callee_clobbered)` (le pointeur est dans `rdi`), et l'appelant conserve des valeurs vivantes dans `rbx`, `rbp`, `r12`, `r13`, `r14` et `r15`, qu'il s'attend à retrouver intactes.

Travail à faire :

1. Sauvegarder tous les registres préservés par l'appelé.
2. Les écraser tous en les mettant à `0x1337`.
3. Appeler `check_callee_clobbered`, qui vérifie qu'ils ont bien été écrasés.
4. Restaurer les registres préservés par l'appelé.
5. `ret`.

Le défi vérifie ensuite que `rbx`, `rbp` et `r12` à `r15` ont été restitués exactement comme l'appelant les avait laissés.

Construire la bibliothèque partagée et la soumettre au correcteur :

```console
hacker@dojo:~$ as -o your-solve.o your-solve.s
hacker@dojo:~$ ld -shared -o your-solve.so your-solve.o
hacker@dojo:~$ /challenge/check your-solve.so
```

Les emprunter, les rendre, et obtenir le flag.
