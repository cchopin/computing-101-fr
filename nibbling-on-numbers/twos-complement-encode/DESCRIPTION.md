Les trois premiers niveaux vous ont fait *lire* des bits comme un nombre signé. Faisons maintenant le chemin inverse : à partir d'un nombre, **écrire** ses bits en complément à deux.

Pour un nombre positif ou nul, il suffit de sa représentation binaire ordinaire, complétée par des zéros de tête pour remplir l'octet. Pour un nombre négatif, rappelez-vous la règle du tout premier niveau : le motif de `n` bits en complément à deux d'une valeur négative est celui du nombre non signé `(valeur + 2ⁿ)`. Sur un octet (8 bits), cela revient à ajouter 256 :

```
  -5   ->   -5 + 256 = 251   ->   11111011
  42   ->                         00101010
```

(De façon équivalente : inverser les bits de `+5`, puis ajouter un. On obtient le même résultat, `11111011`. Les deux façons de raisonner conviennent.)

Lancez `/challenge/encode` et obtenez le flag !
