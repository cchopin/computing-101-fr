Maintenant, apprenons à la calculatrice les opérateurs *bit à bit* !

Tous les opérateurs vus jusqu'ici étaient arithmétiques. Les trois suivants combinent leurs opérandes bit par bit, et vous les avez tous rencontrés dans `assembly-assortment` :

- `^` est le XOR : chaque bit du résultat vaut `1` lorsque *exactement un* des bits d'entrée vaut `1`.
- `|` est le OR : chaque bit du résultat vaut `1` lorsque *l'un ou l'autre* bit d'entrée vaut `1`.
- `&` est le AND : chaque bit du résultat vaut `1` seulement lorsque *les deux* bits d'entrée valent `1`.

Ajoutez trois branches à votre dispatch : `'^'` → `xor`, `'|'` → `or`, `'&'` → `and`, en plus de `'+'`, `'-'` et `'*'`.
Tout opérateur que vous ne reconnaissez toujours pas fait quitter avec un code de sortie non nul.

Un résultat bit à bit reste un nombre sur 64 bits : affichez-le comme toute autre réponse, en le transmettant à votre `itoa` avec signe, puis en écrivant le texte avec `write`.

```
"12"  ->  atoi  ->  12     (0000 1100)
"10"  ->  atoi  ->  10     (0000 1010)
12  ^  10  =  6            (0000 0110)
12  |  10  =  14           (0000 1110)
12  &  10  =  8            (0000 1000)
```

Deux de ces caractères ont un sens particulier pour le shell : `|` enchaîne des commandes, et `&` en lance une en arrière-plan. Mettez-les donc entre guillemets lorsque vous lancez votre programme à la main (`^` peut être saisi sans guillemets) :

```console
hacker@dojo:~$ as -o prog.o prog.s
hacker@dojo:~$ ld -o prog prog.o
hacker@dojo:~$ ./prog 12 '|' 10
14
hacker@dojo:~$ /challenge/check prog
```

Ajoutez les trois branches bit à bit.
