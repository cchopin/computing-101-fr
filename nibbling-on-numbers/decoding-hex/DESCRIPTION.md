Jusqu'ici, on a **encodé** des données binaires en hexadécimal. Faisons maintenant le chemin inverse et **décodons** : on retrouve les bits que représente un texte hexadécimal.

C'est la même correspondance, lue dans l'autre sens.
Chaque chiffre hexadécimal devient ses 4 bits, écrits dans l'ordre.
Comme un octet correspond à deux chiffres hexadécimaux, décoder un octet revient à développer deux chiffres hexadécimaux en 8 bits :

```
0x   0      a
     0000   1010
->   00001010
```

C'est exactement ce que fait un programme qui reçoit des données encodées en hexadécimal : il transforme chaque paire de chiffres en l'octet correspondant avant de traiter les données.
Écrire les 8 bits complets, zéros de tête compris : chaque chiffre hexadécimal vaut toujours exactement 4 bits.

Lancer `/challenge/convert` pour obtenir le flag !
