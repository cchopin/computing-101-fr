Avant chaque exécution, le niveau place dynamiquement des valeurs en mémoire, et ces valeurs changent à chaque fois. Il faut donc effectuer un calcul, sous une forme ou une autre, avec les registres. Le niveau indique quels registres sont préremplis et où placer le résultat. Dans la plupart des cas, c'est `rax`.

Dans ce niveau, on travaille avec la pile, la zone de mémoire qui grandit et rétrécit dynamiquement. Il faut lire et écrire dans la pile, ce qui peut demander les instructions `pop` et `push`. On peut aussi avoir besoin du registre de pointeur de pile (`rsp`) pour savoir où pointe la pile.

Dans les niveaux précédents, on a utilisé `push` et `pop` pour stocker et charger des données sur la pile. On peut aussi accéder directement à la pile en passant par le pointeur de pile.

Sur x86, le pointeur de pile est stocké dans le registre spécial `rsp`. `rsp` contient toujours l'adresse mémoire du sommet de la pile, c'est-à-dire l'adresse de la dernière valeur empilée.

Comme dans les niveaux de mémoire, on peut utiliser `[rsp]` pour accéder à la valeur située à l'adresse contenue dans `rsp`.

Sans utiliser `pop`, il faut calculer la moyenne de 4 mots quadruples consécutifs stockés sur la pile. Empiler la moyenne sur la pile.

Indice :
- `RSP+0x??` mot quadruple A
- `RSP+0x??` mot quadruple B
- `RSP+0x??` mot quadruple C
- `RSP`      mot quadruple D
