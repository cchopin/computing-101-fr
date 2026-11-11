Il faut maintenant utiliser `shr` pour positionner des bits.
Ramener le deuxième octet de la valeur d'entrée à l'extrémité de poids faible, puis réutiliser le masque de [Masquer des bits](https://pwn.college/computing-101-fr/assembly-assortment/bit-and) pour éliminer tout ce qui se trouve en dehors de cet octet.

Écrivez une fonction nommée `solve` qui reçoit dans `rdi` une valeur et renvoie dans `rax` son *deuxième* octet, c'est-à-dire les bits 8 à 15, sous forme de nombre de 0 à 255.

La construire en bibliothèque partagée, puis la transmettre au correcteur :

```console
hacker@dojo:~$ as -o solve.o solve.s
hacker@dojo:~$ ld -shared -o solve.so solve.o
hacker@dojo:~$ /challenge/check solve.so
```
