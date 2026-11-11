Dans le niveau précédent, nous exécutions la commande `disassemble` à la place de l'apprenant, après le lancement du programme.
Cette fois, c'est à faire soi-même !

Après avoir lancé le programme avec `starti`, il faut exécuter soi-même la commande `disassemble` :

```gdb
(gdb) starti
...
(gdb) disassemble
Dump of assembler code for function main:
=> 0x0000000000401000 <+0>:     mov    rdi,0x539
   0x0000000000401007 <+7>:     mov    rdi,0x0
   0x000000000040100e <+14>:    mov    rax,0x3c
   0x0000000000401015 <+21>:    syscall
End of assembler dump.
```

Lire le résultat pour trouver le nombre secret, puis le soumettre avec `/challenge/submit-number`.
