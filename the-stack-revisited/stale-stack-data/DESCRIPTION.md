Dans le niveau précédent, vous avez chargé une valeur périmée de 8 octets provenant d'un ancien cadre appelé.
Maintenant, vous appliquez la même idée de pile périmée à un tampon d'octets.

Ce défi passe à votre `solve` un pointeur vers une fonction nommée `read_flag`.
Appelez cette fonction en premier.
Elle place le flag dans son propre cadre de pile puis retourne.
Une fois qu'elle a retourné, cet ancien cadre se trouve à gauche de votre `rsp` actuel, et les **octets du flag y restent** tant que rien ne les écrase !

Écrivez une fonction nommée `solve` qui appelle le pointeur de fonction contenu dans `rdi`, puis écrit sur la sortie standard les octets périmés du flag, provenant de l'ancien cadre appelé.
Du point de vue de `solve`, ces octets périmés se trouvent à un décalage négatif par rapport à `rsp` : trouvez le décalage exact dans `gdb`, ou lisez-le dans la sortie du correcteur.
La distinction importante est celle entre valeur et adresse.
Si vous vouliez charger une valeur de 8 octets depuis cet ancien cadre, vous utiliseriez `mov` :

```asm
mov rax, qword ptr [rsp-0x40]
```

Mais `write` a besoin de l'adresse du premier octet dans `rsi`, et non de la valeur qword qui y est stockée.
Pour cela, calculez l'adresse avec `lea` :

```asm
lea rsi, [rsp-0x40]
```

Ce décalage n'est qu'un exemple : utilisez celui affiché par le correcteur.
Construisez-la en bibliothèque partagée, puis transmettez-la au correcteur :

```console
hacker@dojo:~$ as -o solve.o solve.s
hacker@dojo:~$ ld -shared -o solve.so solve.o
hacker@dojo:~$ /challenge/check solve.so
```

Pour déboguer une fonction soumise à l'intérieur d'une bibliothèque partagée, reportez-vous à [Écrire depuis une bibliothèque partagée](https://pwn.college/computing-101-fr/control-flow/callee-write).
