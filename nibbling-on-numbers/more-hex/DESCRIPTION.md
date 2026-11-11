Comme pour les nombres décimaux, on peut en accoler autant qu'on veut pour représenter davantage d'octets.
Chaque paire de chiffres hexadécimaux représente un octet supplémentaire.
Un seul chiffre hexadécimal s'appelle un _quartet_ (nibble), ce qui fait sourire, mais ce mot n'est pas utilisé pour désigner des données.
On travaille presque toujours au niveau de l'_octet_, jamais en dessous.

Nous nous entraînons dans ce défi.
Découper les 8 bits de chaque octet en deux groupes de 4, transformer chaque groupe en chiffre hexadécimal, puis écrire les octets de gauche à droite. Par exemple :

```
1110 0011   0101 1010   1001 0000
 e    3      5    a      9    0
-> 0xe35a90
```

À jouer : lancer `/challenge/convert` pour obtenir le flag !
