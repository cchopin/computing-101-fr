Allons plus loin !
Nous passons à 4 octets, soit 32 bits.
Reprendre la même démarche avec `ipython` que pour le niveau sur 16 bits : convertissez d'abord la valeur en non signé, puis soustrayez `2**32` si le bit de poids fort est à 1.
La valeur non signée maximale, `11111111 11111111 11111111 11111111`, vaut `2**32-1`, soit `4 294 967 295`.
La valeur signée maximale, `01111111 11111111 11111111 11111111`, vaut `2**31-1`, soit `2 147 483 647`, et la valeur signée minimale, `10000000 00000000 00000000 00000000`, vaut `-2**31`, soit `-2 147 483 648`.

Lancer `/challenge/decode` pour obtenir le flag !
