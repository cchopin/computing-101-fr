Dans le niveau précédent, une valeur périmée de 8 octets, provenant d'un ancien cadre appelé, a été chargée.
Maintenant, la même idée de pile périmée est appliquée à un tampon d'octets.

Ce défi passe à `solve` un pointeur vers une fonction nommée `read_flag`.
Appeler cette fonction en premier.
Elle place le flag dans son propre cadre de pile puis retourne.
Une fois qu'elle a retourné, cet ancien cadre se trouve à gauche du `rsp` actuel, et les **octets du flag y restent** tant que rien ne les écrase !

Écrire une fonction nommée `solve` qui appelle le pointeur de fonction contenu dans `rdi`, puis écrit sur la sortie standard les octets périmés du flag, provenant de l'ancien cadre appelé.
Du point de vue de `solve`, ces octets périmés se trouvent à un décalage négatif par rapport à `rsp` : trouver le décalage exact dans `gdb`, ou le lire dans la sortie du correcteur.
La distinction importante est celle entre valeur et adresse.
Pour charger une valeur de 8 octets depuis cet ancien cadre, on utiliserait `mov` :

```asm
mov rax, qword ptr [rsp-0x40]
```

Mais `write` a besoin de l'adresse du premier octet dans `rsi`, et non de la valeur qword qui y est stockée.
Pour cela, calculer l'adresse avec `lea` :

```asm
lea rsi, [rsp-0x40]
```

Ce décalage n'est qu'un exemple : utiliser celui affiché par le correcteur.
Construire en bibliothèque partagée, puis la transmettre au correcteur :

```console
hacker@dojo:~$ as -o solve.o solve.s
hacker@dojo:~$ ld -shared -o solve.so solve.o
hacker@dojo:~$ /challenge/check solve.so
```

Pour déboguer une fonction soumise à l'intérieur d'une bibliothèque partagée, voir [Écrire depuis une bibliothèque partagée](https://pwn.college/computing-101-fr/control-flow/callee-write).
