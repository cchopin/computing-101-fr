Voici une nouvelle instruction pour la boîte à outils : `and`.

Un `and` bit à bit compare deux valeurs bit par bit.
Chaque bit de sortie vaut `1` seulement si *les deux* bits d'entrée valent `1`, et `0` sinon.
Voici la règle pour une paire de bits :

```
0 & 0 = 0
0 & 1 = 0
1 & 0 = 0
1 & 1 = 1
```

`and` applique cette règle à toutes les positions de bits en même temps :

```
  1011 0111   (your value)
& 0000 0001   (the mask)
---------
  0000 0001   (only the lowest bit survives)
```

Le masque ci-dessus ne vaut `1` qu'à un seul endroit : le bit de poids faible.
Partout où le masque vaut `0`, le résultat est forcé à `0` : tous les bits sauf le plus faible sont donc effacés.
Il ne reste que le bit de poids faible de la valeur, isolé.

Ce bit a une propriété particulière : il indique si le nombre entier est pair ou impair.
Un nombre est pair exactement lorsque son bit de poids faible vaut `0`, et impair lorsqu'il vaut `1`.
En masquant tout sauf le bit de poids faible, comme on vient de le faire, on obtient donc la parité du nombre.

Écrivez une fonction nommée `solve` qui reçoit dans `rdi` une valeur sur 64 bits et renvoie dans `rax` `1` si la valeur est **paire**, et `0` si elle est **impaire**.

Attention : le bit isolé vaut `1` pour un nombre *impair*, alors que `solve` doit renvoyer `1` pour un nombre *pair*.
Le bit de poids faible n'est donc pas directement la réponse : il faut en inverser la valeur.

La construire en bibliothèque partagée, puis la transmettre au correcteur :

```console
hacker@dojo:~$ as -o solve.o solve.s
hacker@dojo:~$ ld -shared -o solve.so solve.o
hacker@dojo:~$ /challenge/check solve.so
```
