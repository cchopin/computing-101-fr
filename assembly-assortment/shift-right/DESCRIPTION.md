Utilisez maintenant `shr` pour positionner des bits.
Ramenez le deuxième octet de la valeur d'entrée à l'extrémité de poids faible, puis réutilisez le masque de [Masquer des bits](/computing-101/assembly-assortment/bit-and) pour éliminer tout ce qui se trouve en dehors de cet octet.

Écrivez une fonction nommée `solve` qui reçoit dans `rdi` une valeur et renvoie dans `rax` son *deuxième* octet, c'est-à-dire les bits 8 à 15, sous forme de nombre de 0 à 255.

Construisez-la en bibliothèque partagée, puis transmettez-la au correcteur :

```console
hacker@dojo:~$ as -o solve.o solve.s
hacker@dojo:~$ ld -shared -o solve.so solve.o
hacker@dojo:~$ /challenge/check solve.so
```
