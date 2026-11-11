Avant chaque exécution, le niveau place dynamiquement des valeurs en mémoire, et ces valeurs changent à chaque fois. Il faut donc effectuer un calcul, sous une forme ou une autre, avec les registres. Le niveau indique quels registres sont préremplis et où placer le résultat. Dans la plupart des cas, c'est `rax`.

Dans ce niveau, on travaille avec le contrôle du flot d'exécution. Il s'agit d'utiliser des instructions qui contrôlent, directement ou indirectement, le registre spécial `rip`, le pointeur d'instruction. On utilisera des instructions comme `jmp`, `call`, `cmp` et leurs variantes pour obtenir le comportement demandé.

Jusqu'ici, on a manipulé le flot d'exécution de façon indirecte, en modifiant des données. Mais x86 fournit des instructions qui agissent directement sur le flot de contrôle.

Il existe deux grandes façons de modifier le flot de contrôle :
- par un saut (*jump*)
- par un appel de fonction (*call*)

Dans ce niveau, on travaille avec les sauts.

Il existe deux types de sauts :
- les sauts inconditionnels
- les sauts conditionnels

Un saut inconditionnel est toujours exécuté, et ne dépend pas du résultat des instructions précédentes.

On sait que les emplacements mémoire peuvent contenir des données et des instructions. Votre code est stocké à l'adresse `0x400042` (cette adresse change à chaque exécution).

Pour tous les sauts, il existe trois types :
- Les sauts relatifs : on saute en avant ou en arrière de un certain nombre d'instructions ou d'octets.
- Les sauts absolus : on saute vers une adresse précise.
- Les sauts indirects : on saute vers l'adresse contenue dans un registre.

En x86, un saut absolu se fait en chargeant d'abord l'adresse cible dans un registre à usage général (appelons-le `reg`), puis en exécutant `jmp reg`.

Dans ce niveau, on demande un saut absolu. Il faut faire ceci : sauter à l'adresse absolue `0x403000`.
