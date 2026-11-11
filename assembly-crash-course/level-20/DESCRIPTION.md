Avant chaque exécution, le niveau place dynamiquement des valeurs en mémoire, et ces valeurs changent à chaque fois. Il faut donc effectuer un calcul, sous une forme ou une autre, avec les registres. Le niveau indique quels registres sont préremplis et où placer le résultat. Dans la plupart des cas, c'est `rax`.

Dans ce niveau, on travaille avec le contrôle du flot d'exécution. Il s'agit d'utiliser des instructions qui contrôlent, directement ou indirectement, le registre spécial `rip`, le pointeur d'instruction. On utilisera des instructions comme `jmp`, `call`, `cmp` et leurs variantes pour obtenir le comportement demandé.

Dans un niveau précédent, on a calculé la moyenne de 4 mots quadruples entiers, un nombre fixe de valeurs. Mais comment traiter une taille connue seulement pendant l'exécution du programme ?

Dans la plupart des langages de programmation, il existe une structure appelée boucle for, qui permet d'exécuter un ensemble d'instructions un nombre borné de fois. Ce nombre peut être connu avant l'exécution, ou bien être fourni dynamiquement pendant l'exécution.

Par exemple, une boucle for peut calculer la somme des nombres de 1 à n :

```plaintext
sum = 0
i = 1
while i <= n:
    sum += i
    i += 1
```

Il faut calculer la moyenne de `n` mots quadruples consécutifs, où :
- `rdi` = adresse mémoire du 1er mot quadruple
- `rsi` = `n` (nombre d'itérations de la boucle)
- `rax` = moyenne calculée
