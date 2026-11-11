Avant chaque exécution, le niveau place dynamiquement des valeurs en mémoire, et ces valeurs changent à chaque fois. Il faut donc effectuer un calcul, sous une forme ou une autre, avec les registres. Le niveau indique quels registres sont préremplis et où placer le résultat. Dans la plupart des cas, c'est `rax`.

Dans ce niveau, on travaille avec la pile, la zone de mémoire qui grandit et rétrécit dynamiquement. Il faut lire et écrire dans la pile, ce qui peut demander les instructions `pop` et `push`. On peut aussi avoir besoin du registre de pointeur de pile (`rsp`) pour savoir où pointe la pile.

Dans ces niveaux, on découvre la pile.

La pile est une zone de mémoire qui permet de conserver des valeurs pour plus tard.

Pour stocker une valeur sur la pile, on utilise l'instruction `push`, et pour la récupérer, on utilise `pop`.

La pile fonctionne en LIFO (*last in, first out*, dernier entré, premier sorti) : la dernière valeur empilée est la première à être dépilée.

Imaginons qu'on range des assiettes au retour du lave-vaisselle. Il y a une assiette rouge, une verte et une bleue. On pose d'abord la rouge dans le placard, puis la verte sur la rouge, puis la bleue.

La pile d'assiettes ressemble alors à ceci :

```
Top ----> Blue
          Green
Bottom -> Red
```

Si l'on veut une assiette pour faire un sandwich, on prend celle du dessus, la bleue, qui a été rangée en dernier, donc sortie en premier.

Sur x86, l'instruction `pop` prend la valeur du sommet de la pile et la place dans un registre.

De même, l'instruction `push` prend la valeur d'un registre et la place au sommet de la pile.

En utilisant ces instructions, il faut prendre la valeur du sommet de la pile, lui soustraire `rdi`, puis la remettre sur la pile.
