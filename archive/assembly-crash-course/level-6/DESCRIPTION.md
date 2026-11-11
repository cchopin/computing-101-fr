Ce niveau porte sur les registres : il faut modifier leur contenu ou le lire.

Les valeurs sont placées dynamiquement en mémoire avant chaque exécution, donc elles changent d'une fois à l'autre. Il faut effectuer un calcul sur les registres. Le niveau indique quels registres sont préremplis et où placer le résultat. Dans la plupart des cas, c'est `rax`.

Utiliser `div` pour calculer un modulo est lent !

On peut employer une astuce mathématique pour optimiser le modulo (`%`). Les compilateurs l'utilisent beaucoup.

Si l'on a `x % y`, et que `y` est une puissance de 2, soit `2^n`, le résultat correspond aux `n` bits de poids faible de `x`.

On peut donc utiliser les accès aux octets de poids faible des registres pour calculer le modulo efficacement.

En utilisant uniquement l'instruction (ou les instructions) suivante :
- `mov`

Il faut calculer :
- `rax = rdi % 256`
- `rbx = rsi % 65536`
