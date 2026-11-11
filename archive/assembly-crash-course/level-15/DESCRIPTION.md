Avant chaque exécution, le niveau place dynamiquement des valeurs en mémoire, et ces valeurs changent à chaque fois. Il faut donc effectuer un calcul, sous une forme ou une autre, avec les registres. Le niveau indique quels registres sont préremplis et où placer le résultat. Dans la plupart des cas, c'est `rax`.

Dans ce niveau, on travaille avec la pile, la zone de mémoire qui grandit et rétrécit dynamiquement. Il faut lire et écrire dans la pile, ce qui peut demander les instructions `pop` et `push`. On peut aussi avoir besoin du registre de pointeur de pile (`rsp`) pour savoir où pointe la pile.

Dans ce niveau, on explore la propriété LIFO (dernier entré, premier sorti) de la pile.

En utilisant uniquement les instructions suivantes :
- `push`
- `pop`

Il faut échanger les valeurs de `rdi` et `rsi`.

Exemple :
- Si au départ `rdi = 2` et `rsi = 5`
- Alors à la fin `rdi = 5` et `rsi = 2`
