Le complément à deux a été vu un octet à la fois, mais rien n'est propre aux 8 bits.
Ça fonctionne quelle que soit la largeur !

Dans ce niveau, nous pratiquons le complément à deux sur des *valeurs de 16 bits*.
Se rappeler que `rax` et ses semblables ont 64 bits et utilisent aussi le complément à deux : la marge est donc grande !

Une valeur de 16 bits peut aller jusqu'à `2^16-1` en non signé (65535). Mais si l'on interprète la valeur binaire `1111111111111111` comme un entier signé, on obtient `-1` !
Pour interpréter `1000000000000000` (32768 en non signé) comme un nombre signé, il faut lui soustraire 65536, ce qui donne -32768 : la plus petite valeur signée représentable sur 16 bits (la plus grande, `0111111111111111`, vaut 32767).

Cela peut devenir un peu confus, et les différences entre les valeurs maximales des nombres signés et non signés sont à l'origine de nombreux bogues et failles de sécurité !

La taille de ces nombres rend le calcul à la main pénible, et il n'est pas demandé de faire ces conversions de tête.
Notre conseil : convertir d'abord la valeur en non signé à l'aide d'un outil, puis faire la soustraction du complément à deux si la valeur est signée.
L'un de ces outils est le langage `Python`.
Inutile d'y écrire un programme pour l'instant (nous y viendrons), il suffit d'utiliser son mode interactif comme une calculatrice.
Par exemple, pour convertir le binaire `1001111101011100`, on peut écrire :

```console
hacker@dojo$ ipython
In [1]: 0b1001111101011100
Out[1]: 40796

In [2]: 40796 - 65536
Out[2]: -24740

In [3]: exit
hacker@dojo$
```

Quelques remarques :

- `In [1]` est l'invite de saisie de l'interpréteur interactif `ipython`, et `Out [1]` est le résultat de l'expression saisie en `In [1]`.
- `0b` est le préfixe Python qui distingue les nombres écrits en binaire des nombres décimaux (par exemple, `0b101` vaut 5, et `101` vaut 101).

Lancer `/challenge/decode` pour obtenir le flag !
