Avant chaque exécution, le niveau place dynamiquement des valeurs en mémoire, et ces valeurs changent à chaque fois. Il faut donc effectuer un calcul, sous une forme ou une autre, avec les registres. Le niveau indique quels registres sont préremplis et où placer le résultat. Dans la plupart des cas, c'est `rax`.

Dans ce niveau, on travaille avec le contrôle du flot d'exécution. Il s'agit d'utiliser des instructions qui contrôlent, directement ou indirectement, le registre spécial `rip`, le pointeur d'instruction. On utilisera des instructions comme `jmp`, `call`, `cmp` et leurs variantes pour obtenir le comportement demandé.

On combine maintenant les deux niveaux précédents. Il faut faire ceci :

- Créer un trampoline à deux sauts :
  - Faire en sorte que la première instruction du code soit un `jmp`.
  - Faire de ce `jmp` un saut relatif de 0x51 octets à partir de sa position actuelle.
  - À l'adresse 0x51, écrire le code suivant :
    - Placer la valeur au sommet de la pile dans le registre `rdi`.
    - `jmp` vers l'adresse absolue 0x403000.
