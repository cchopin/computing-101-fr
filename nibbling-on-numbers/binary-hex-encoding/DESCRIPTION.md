Il est temps de mettre en pratique ce qui précède.

Point clé : un chiffre hexadécimal vaut exactement 4 bits, donc un octet, qui compte 8 bits, n'est rien d'autre que deux chiffres hexadécimaux.
Pour convertir un octet du binaire vers l'hexadécimal, on découpe ses 8 bits en deux groupes de 4, puis on cherche la valeur de chaque groupe :

```
11100011     l'octet, en binaire
1110 0011    l'octet, en binaire, découpé en deux groupes de 4 bits
 e    3      chaque groupe de 4 bits donne un chiffre hexadécimal
->  0xe3     l'hexadécimal !
```

Lancer `/challenge/convert` pour obtenir le flag !
