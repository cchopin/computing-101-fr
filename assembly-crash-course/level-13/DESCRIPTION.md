Avant chaque exécution, le niveau place dynamiquement des valeurs en mémoire, et ces valeurs changent à chaque fois. Il faut donc effectuer un calcul, sous une forme ou une autre, avec les registres. Le niveau indique quels registres sont préremplis et où placer le résultat. Dans la plupart des cas, c'est `rax`.

Dans ce niveau, on travaille avec la mémoire. Il faut lire ou écrire des données stockées de façon linéaire en mémoire. Si ce point n'est pas clair, on peut revoir le module sur l'adressage linéaire de *Ike* (en anglais). On peut aussi devoir déréférencer des adresses, parfois plusieurs fois, vers des données placées dynamiquement en mémoire pour le niveau.

Rappel : la mémoire est stockée de façon linéaire.

Qu'est-ce que cela signifie ?

Prenons le mot quadruple situé à `0x1337` :
```
[0x1337] = 0x00000000deadbeef
```

En réalité, la mémoire est organisée octet par octet, en little endian :
```
[0x1337] = 0xef
[0x1337 + 1] = 0xbe
[0x1337 + 2] = 0xad
...
[0x1337 + 7] = 0x00
```

Qu'est-ce que cela change ?

Cela signifie qu'on peut accéder à des données voisines à l'aide de décalages, comme dans l'exemple précédent.

Par exemple, pour accéder au 5e *octet* à partir d'une adresse, on écrit :
```
mov al, [address+4]
```

Rappel : les décalages commencent à 0.

Il faut faire ceci :
- Charger deux mots quadruples consécutifs à partir de l'adresse contenue dans `rdi`.
- Calculer la somme des deux mots quadruples de l'étape précédente.
- Stocker cette somme à l'adresse contenue dans `rsi`.
