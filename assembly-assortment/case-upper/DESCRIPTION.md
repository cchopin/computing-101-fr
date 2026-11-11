Pour passer en minuscules, on a activé le bit de casse avec `or`.
Passer en majuscules est l'opération inverse : on **efface** ce même bit.

`and` est l'outil pour effacer des bits : on l'a utilisé pour ramener des bits à zéro par masquage. Un `0` dans le masque force le bit du résultat à `0` ; un `1` laisse passer le bit d'origine.
Pour effacer uniquement le bit `0x20` et conserver tous les autres, le masque est donc `0x20` inversé, soit `0xDF`.

```
  0110 0001   ('a', 0x61)
& 1101 1111   (0xDF: keep every bit except 0x20)
---------
  0100 0001   ('A', 0x41)
```

```
and rax, 0xDF
```

Cela efface le bit de casse d'une seule lettre.
Cette fois, on l'appliquera à une chaîne entière.

Une chaîne est une suite d'octets en mémoire, les uns après les autres, qui se termine par un octet `0` (le terminateur NUL) indiquant la fin de la chaîne.
Pour la parcourir, il faut une boucle : la même forme que celle où l'on revient au début avec `jmp`, que l'on a pratiquée dans [Écrire des boucles](https://pwn.college/computing-101-fr/control-flow/loop-write).

La boucle s'organise ainsi : lire l'octet suivant ; si c'est le NUL (`0`), sauter après la boucle et c'est terminé ; sinon effacer son bit de casse, le réécrire en mémoire, passer à l'octet suivant, et revenir au début de la boucle.
La structure générale de la boucle est :

```
loop:
  ...
  je done
  ...
  jmp loop
done:
  ...
  ...
  ret
```

Écrivez une fonction qui reçoit dans `rdi` un pointeur vers une chaîne ASCII en minuscules, et la met en majuscules *sur place*, en bouclant jusqu'au NUL.
Elle ne renvoie rien.
La nommer `str_upper` (elle traite une chaîne entière, pas un seul caractère), et l'exporter avec `.global str_upper`.

La construire en bibliothèque partagée, puis la transmettre au correcteur :

```console
hacker@dojo:~$ as -o str_upper.o str_upper.s
hacker@dojo:~$ ld -shared -o str_upper.so str_upper.o
hacker@dojo:~$ /challenge/check str_upper.so
```
