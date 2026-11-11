Les ordinateurs stockent les données sous forme d'octets, eux-mêmes formés de bits.
Les registres connus, comme `rax`, ont une largeur de 64 bits : ils peuvent contenir des valeurs de 0 (tous les bits à 0) à `2^64-1` (tous les bits à 1).
Mais comment stocker des nombres *négatifs* ?

Une première méthode consistait à utiliser un *bit de signe* : un `5` décimal positif pouvait être stocké sous la forme de l'octet `00000101`, et un `5` négatif sous la forme `10000101`.
Cela paraît logique, mais pose un problème important : les calculs.

Au cœur du processeur se trouve un sous-composant appelé *unité arithmétique et logique* (ALU, Arithmetic Logical Unit), chargé des calculs.
Ce composant doit être très optimisé : moins il est complexe pour additionner, mieux c'est.
En même temps, les calculs doivent être justes.
Cela pose deux problèmes :

1. Avec un bit de signe, il existe *deux* représentations de zéro : `00000000` (zéro positif) et `10000000` (zéro négatif). C'est gênant à bien des égards, notamment parce que l'ALU doit alors être plus compliquée.
2. Avec un bit de signe, il faut utiliser des algorithmes différents pour les nombres signés et non signés. On peut additionner `00000010` (décimal `2`) et `00000001` (décimal `1`) avec une addition binaire ordinaire, mais `10000010` (décimal `-2`) et `00000001` (décimal `1`) doivent être traités en partie comme une soustraction.

Ce n'est pas idéal.
Heureusement, des chercheurs ont trouvé une méthode astucieuse, le [complément à deux](https://fr.wikipedia.org/wiki/Compl%C3%A9ment_%C3%A0_deux) (*Two's Complement*).
Le complément à deux résout les deux problèmes en représentant la moitié de l'espace des bits comme négative, d'une façon compatible avec l'arithmétique non signée.
Considérons cette soustraction :

```text
  00000001    un 1 positif
- 00000010    un 2 positif (qu'on soustrait)
```

Il n'y a pas assez de bits pour effectuer cette soustraction, on ajoute donc un bit de retenue (*borrow*) :

```text
  1 00000001    un 1 positif (avec le bit de retenue)
- 0 00000010    un 2 positif (qu'on soustrait)
  ----------
  0 11111111    le résultat de la soustraction ordinaire, indépendante du signe
```

Ainsi, `11111111` *est* `-1` : si on l'additionne à `1`, les deux s'annulent, et le bit en trop sort de l'octet et disparaît.
Cette méthode a plusieurs avantages :

1. Le bit de poids fort reste le *bit de signe* : s'il vaut 1, la valeur est négative. C'est très facile à tester.
2. Il n'existe qu'un seul zéro : `00000000`.
3. L'arithmétique fonctionne exactement de la même façon pour les nombres signés et non signés.

L'inconvénient est une légère complexité pour l'humain : la valeur absolue d'un nombre négatif est la valeur non signée de l'octet *moins 256*.
Dans l'exemple ci-dessus, `11111111` vaut `255 - 256 = -1`.
C'est un peu compliqué, mais on finit par s'y habituer.
Par ailleurs, si on considère la valeur comme non signée, on peut la traiter comme `255` dans les calculs voulus, et tout fonctionne quand même !

Passons à la pratique.
Ce défi oblige à comprendre le complément à deux sur des octets.
Lancer `/challenge/decode` pour obtenir le flag !
