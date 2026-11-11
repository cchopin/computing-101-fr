Ce niveau porte sur les registres : il faut modifier leur contenu ou le lire.

Les valeurs sont placées dynamiquement en mémoire avant chaque exécution, donc elles changent d'une fois à l'autre. Il faut effectuer un calcul sur les registres. Le niveau indique quels registres sont préremplis et où placer le résultat. Dans la plupart des cas, c'est `rax`.

Ce niveau porte sur les opérations au niveau des bits. Il faut manipuler directement les bits d'un registre ou d'un emplacement mémoire. On aura probablement besoin des instructions logiques de x86 : `and`, `or`, `not`, `xor`.

Décaler les bits en assembleur est un autre concept intéressant !

x86 permet de « décaler » les bits d'un registre.

Prenons `al`, les 8 bits de poids faible de `rax`.

La valeur de `al`, en bits, est :

```
al = 10001010
```

Si l'on décale une fois vers la gauche avec l'instruction `shl` :

```
shl al, 1
```

La nouvelle valeur est :

```
al = 00010100
```

Tous les bits se sont décalés vers la gauche : le bit de poids fort est sorti, et un nouveau 0 est entré à droite.

On peut s'en servir pour agir sur les bits qui nous intéressent.

Décaler revient à multiplier ou diviser par 2, et permet aussi de calculer un modulo.

Voici les instructions importantes :

- `shl reg1, reg2` <=> décale `reg1` vers la gauche du nombre indiqué par `reg2`
- `shr reg1, reg2` <=> décale `reg1` vers la droite du nombre indiqué par `reg2`

Remarque : `reg2` peut être remplacé par une constante ou par un emplacement mémoire.

Quand on parle de *bit* ou d'*octet de poids fort* ou *faible*, « poids » désigne l'importance du bit ou de l'octet pour la valeur.

- Le *bit de poids faible* (ou octet) a la plus petite valeur (la position la plus « basse »). Par exemple, modifier le bit le plus à droite ne change la valeur que de 1.
- Le *bit de poids fort* (ou octet) a la plus grande valeur (la position la plus « haute »).

**Pour ce défi**, en n'utilisant que les instructions suivantes :

- `mov`, `shr`, `shl`

Il faut faire ceci :
Placer dans `rax` le 5e octet de poids faible de `rdi`.

Par exemple :

```
rdi = | B7 | B6 | B5 | B4 | B3 | B2 | B1 | B0 |
Set rax to the value of B4
```
