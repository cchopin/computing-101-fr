Dans le niveau précédent, un cadre de pile a été réservé, mis à zéro, `rsp` restauré, et le retour fait en toute sécurité.
Maintenant, il faut faire travailler ce cadre.

Compter le nombre de valeurs d'octets *distinctes* présentes dans un tampon.
La méthode naturelle consiste à tenir une table avec une case par valeur d'octet possible : 256 cases, indexées par la valeur de l'octet lui-même.
Commencer par le même cadre de pile de 256 octets que précédemment, mis à zéro.
Ensuite, à chaque fois que la valeur d'octet `b` apparaît, écrire `1` dans la case `b`.
Après avoir parcouru le tampon, compter les cases marquées.

Écrire une fonction nommée `solve` qui reçoit dans `rdi` un pointeur vers un tampon, et dans `rsi` sa longueur, puis renvoie dans `rax` le nombre de valeurs d'octets distinctes parmi les `rsi` octets.
`mov byte ptr [rsp+rcx], 1` peut être utile pour marquer une valeur donnée (stockée dans `rcx`) comme « présente » dans la table de travail (dont la base est `rsp`).

Construire en bibliothèque partagée, puis la transmettre au correcteur :

```console
hacker@dojo:~$ as -o solve.o solve.s
hacker@dojo:~$ ld -shared -o solve.so solve.o
hacker@dojo:~$ /challenge/check solve.so
```

---
**INDICE :**
Ce niveau demande trois boucles, les unes après les autres : une pour effacer la table de travail, une pour marquer chaque valeur rencontrée, et une pour compter les cases marquées à la fin.

Pour déboguer une fonction soumise à l'intérieur d'une bibliothèque partagée, voir [Écrire depuis une bibliothèque partagée](https://pwn.college/computing-101-fr/control-flow/callee-write).
