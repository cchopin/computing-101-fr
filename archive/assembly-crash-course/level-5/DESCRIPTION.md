Ce niveau porte sur les registres : il faut modifier leur contenu ou le lire.

Les valeurs sont placées dynamiquement en mémoire avant chaque exécution, donc elles changent d'une fois à l'autre. Il faut effectuer un calcul sur les registres. Le niveau indique quels registres sont préremplis et où placer le résultat. Dans la plupart des cas, c'est `rax`.

Le modulo en assembleur est un autre concept intéressant !

x86 permet de récupérer le reste d'une opération `div`.

Par exemple : `10 / 3` donne un reste de `1`.

Le reste est la même chose que le modulo, aussi appelé opérateur « mod ».

Dans la plupart des langages de programmation, le modulo s'écrit `%`.

Il faut calculer : `rdi % rsi`

Placer la valeur dans `rax`.
