`or` active toujours le bit de casse (il force la minuscule) ; `and` l'efface toujours (il force la majuscule).
Aucun des deux n'*inverse* la casse : chacun ne pousse une lettre que dans un seul sens.

Pour inverser un bit (de 1 à 0, ou de 0 à 1), il faut `xor`.
Comme `xor` met un bit à `1` exactement lorsque ses deux entrées *diffèrent*, un XOR avec `1` inverse le bit, et un XOR avec `0` le laisse inchangé.
Un XOR d'une lettre avec `0x20` inverse donc son bit de casse dans les deux sens : une majuscule devient une minuscule, et inversement.

```
  0110 0001   ('a', 0x61)        0100 0001   ('A', 0x41)
^ 0010 0000   (flip 0x20)      ^ 0010 0000   (flip 0x20)
---------                      ---------
  0100 0001   ('A', 0x41)        0110 0001   ('a', 0x61)
```

Cette astuce ne fonctionne *que parce que chaque octet est une lettre*.
Le bit `0x20` ne signifie « casse » que pour les lettres : l'inverser sur un espace ou un chiffre produit un autre caractère, et donc un résultat faux.
Les chaînes que vous recevez ne contiennent que des `A` à `Z` et des `a` à `z` : inverser aveuglément chaque octet est donc sûr, sans vérification.

Parcourez la chaîne comme précédemment : lisez un octet, inversez son bit de casse, réécrivez-le, passez au suivant, et répétez jusqu'au NUL.

Écrivez une fonction qui reçoit dans `rdi` un pointeur vers une chaîne ASCII à casse mixte, et inverse la casse de chaque lettre *sur place*, en bouclant jusqu'au NUL.
Elle ne renvoie rien.
Appelez-la `str_swapcase`, et exportez-la avec `.global str_swapcase`.

Construisez-la en bibliothèque partagée, puis transmettez-la au correcteur :

```console
hacker@dojo:~$ as -o str_swapcase.o str_swapcase.s
hacker@dojo:~$ ld -shared -o str_swapcase.so str_swapcase.o
hacker@dojo:~$ /challenge/check str_swapcase.so
```
