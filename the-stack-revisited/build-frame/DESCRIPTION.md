Dans le niveau précédent, vous avez réservé un cadre de pile, l'avez mis à zéro, restauré `rsp` et retourné en toute sécurité.
Maintenant, vous allez faire travailler ce cadre.

Comptez le nombre de valeurs d'octets *distinctes* présentes dans un tampon.
La méthode naturelle consiste à tenir une table avec une case par valeur d'octet possible : 256 cases, indexées par la valeur de l'octet lui-même.
Commencez par le même cadre de pile de 256 octets que précédemment, mis à zéro.
Ensuite, à chaque fois que vous voyez la valeur d'octet `b`, écrivez `1` dans la case `b`.
Après avoir parcouru le tampon, comptez les cases marquées.

Écrivez une fonction nommée `solve` qui reçoit dans `rdi` un pointeur vers un tampon, et dans `rsi` sa longueur, puis renvoie dans `rax` le nombre de valeurs d'octets distinctes parmi les `rsi` octets.
`mov byte ptr [rsp+rcx], 1` peut vous être utile pour marquer une valeur donnée (stockée dans `rcx`) comme « présente » dans votre table de travail (dont la base est `rsp`).

Construisez-la en bibliothèque partagée, puis transmettez-la au correcteur :

```console
hacker@dojo:~$ as -o solve.o solve.s
hacker@dojo:~$ ld -shared -o solve.so solve.o
hacker@dojo:~$ /challenge/check solve.so
```

---
**INDICE :**
Ce niveau demande trois boucles, les unes après les autres : une pour effacer la table de travail, une pour marquer chaque valeur rencontrée, et une pour compter les cases marquées à la fin.

Pour déboguer une fonction soumise à l'intérieur d'une bibliothèque partagée, reportez-vous à [Écrire depuis une bibliothèque partagée](/computing-101/control-flow/callee-write).
