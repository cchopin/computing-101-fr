Avant chaque exécution, le niveau place dynamiquement des valeurs en mémoire, et ces valeurs changent à chaque fois. Il faut donc effectuer un calcul, sous une forme ou une autre, avec les registres. Le niveau indique quels registres sont préremplis et où placer le résultat. Dans la plupart des cas, c'est `rax`.

Dans ce niveau, on travaille avec la mémoire. Il faut lire ou écrire des données stockées de façon linéaire en mémoire. Si ce point n'est pas clair, on peut revoir le module sur l'adressage linéaire de *Ike* (en anglais). On peut aussi devoir déréférencer des adresses, parfois plusieurs fois, vers des données placées dynamiquement en mémoire pour le niveau.

Il faut noter, comme on a pu le remarquer, que les valeurs sont stockées dans l'ordre inverse de celui dans lequel on les écrit.

Par exemple, supposons :
```
[0x1330] = 0x00000000deadc0de
```

Si l'on regardait la façon dont cela se présente réellement en mémoire, on verrait :
```
[0x1330] = 0xde
[0x1331] = 0xc0
[0x1332] = 0xad
[0x1333] = 0xde
[0x1334] = 0x00
[0x1335] = 0x00
[0x1336] = 0x00
[0x1337] = 0x00
```

Ce format de stockage « à l'envers » est voulu en x86 : on l'appelle « little endian » (petit-boutiste).

Pour ce défi, le niveau fournit deux adresses, créées dynamiquement à chaque exécution.

La première adresse est placée dans `rdi`.
La seconde est placée dans `rsi`.

En s'appuyant sur ce qui précède, il faut faire ceci :
- Placer `[rdi] = 0xdeadbeef00001337`
- Placer `[rsi] = 0xc0ffee0000`

Indice : affecter une grande constante à un emplacement déréférencé peut demander une astuce. Il suffit de placer d'abord la constante dans un registre, puis de copier ce registre vers l'emplacement déréférencé.
