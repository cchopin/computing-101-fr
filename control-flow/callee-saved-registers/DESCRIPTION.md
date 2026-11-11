Vous avez pratiqué les registres sauvegardés par l'appelant, voyons maintenant ceux *préservés par l'appelé*.
Les registres préservés par l'appelé (`rbx`, `rbp`, `r12`, `r13`, `r14`, `r15`) doivent être restitués intacts quand la fonction appelée revient.
Quand une fonction utilise l'un d'eux, elle l'*emprunte* à celui qui l'a appelée, et elle doit le rendre exactement dans l'état où elle l'a trouvé.
C'est ce qui permet à un appelant de garder des valeurs durables dans `rbx`, `rbp` et `r12` à `r15` pendant un appel, en sachant qu'elles seront toujours là après.

La règle est la suivante : quand votre fonction veut utiliser ces registres, *sauvegardez-les (push) à l'entrée de la fonction, et restaurez-les (pop) avant le `ret`*.

Ce niveau vous place du côté de l'appelé.
Votre `solve` est appelé comme `solve(check_callee_clobbered)` (le pointeur est dans `rdi`), et votre appelant conserve des valeurs vivantes dans `rbx`, `rbp`, `r12`, `r13`, `r14` et `r15`, qu'il s'attend à retrouver intactes.

Votre travail :

1. Sauvegarder tous les registres préservés par l'appelé.
2. Les écraser tous en les mettant à `0x1337`.
3. Appeler `check_callee_clobbered`, qui vérifie que vous les avez bien écrasés.
4. Restaurer les registres préservés par l'appelé.
5. `ret`.

Le défi vérifie ensuite que `rbx`, `rbp` et `r12` à `r15` ont été restitués exactement comme l'appelant les avait laissés.

Construisez votre bibliothèque partagée et soumettez-la au correcteur :

```console
hacker@dojo:~$ as -o your-solve.o your-solve.s
hacker@dojo:~$ ld -shared -o your-solve.so your-solve.o
hacker@dojo:~$ /challenge/check your-solve.so
```

Empruntez-les, rendez-les, et obtenez le flag.
