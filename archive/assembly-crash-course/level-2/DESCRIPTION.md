Ce niveau porte sur les registres : il faut modifier leur contenu ou le lire.

Les valeurs sont placées dynamiquement en mémoire avant chaque exécution, donc elles changent d'une fois à l'autre. Il faut effectuer un calcul sur les registres. Le niveau indique quels registres sont préremplis et où placer le résultat. Dans la plupart des cas, c'est `rax`.

Il existe de nombreuses instructions qui permettent d'effectuer les opérations arithmétiques usuelles sur les registres et la mémoire.

Par raccourci, `A += B` signifie `A = A + B`.

Voici quelques instructions utiles :
- `add reg1, reg2` <=> `reg1 += reg2`
- `sub reg1, reg2` <=> `reg1 -= reg2`
- `imul reg1, reg2` <=> `reg1 *= reg2`

La division (`div`) est plus compliquée, on la verra plus tard. Remarque : chaque `regX` peut être remplacé par une constante ou par un emplacement mémoire.

Il faut faire ceci :
- Ajouter `0x331337` à `rdi`
