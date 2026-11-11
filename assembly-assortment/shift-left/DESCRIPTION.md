Mettez maintenant le décalage à gauche que vous venez d'étudier en pratique.

Écrivez une fonction nommée `solve` qui reçoit dans `rdi` une valeur sur 64 bits, la décale de 4 positions vers la gauche (ce qui la multiplie par 16), et renvoie le résultat dans `rax`.

Construisez-la en bibliothèque partagée, puis transmettez-la au correcteur :

```console
hacker@dojo:~$ as -o solve.o solve.s
hacker@dojo:~$ ld -shared -o solve.so solve.o
hacker@dojo:~$ /challenge/check solve.so
```
