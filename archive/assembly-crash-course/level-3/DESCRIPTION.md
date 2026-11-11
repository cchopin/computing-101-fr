Ce niveau porte sur les registres : il faut modifier leur contenu ou le lire.

Les valeurs sont placées dynamiquement en mémoire avant chaque exécution, donc elles changent d'une fois à l'autre. Il faut effectuer un calcul sur les registres. Le niveau indique quels registres sont préremplis et où placer le résultat. Dans la plupart des cas, c'est `rax`.

En s'appuyant sur ce qui précède, il faut calculer :
- `f(x) = mx + b`, où :
  - `m = rdi`
  - `x = rsi`
  - `b = rdx`

Placer le résultat dans `rax`.

Remarque : il existe une différence importante entre `mul` (multiplication non signée) et `imul` (multiplication signée), notamment sur les registres utilisés. Il faut consulter la documentation de ces instructions pour la voir.

Dans ce cas, on utilise `imul`.
