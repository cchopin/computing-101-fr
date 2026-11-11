Les débogueurs, gdb compris, observent le programme débogué *pendant son exécution* pour exposer des informations sur son comportement à l'exécution.
Dans le niveau précédent, nous lancions le programme à votre place. Ici, la magie est un peu réduite : il faut démarrer l'exécution du programme soi-même, et nous ferons le reste (par exemple, retrouver la valeur secrète à partir de celle-ci).

Quand on lance gdb, il finit par afficher une invite de commandes, qui ressemble à ceci :

```gdb
(gdb) 
```

On démarre un programme avec la commande `starti` :

```gdb
(gdb) starti
```

`starti` démarre le programme à sa toute **p**remière **i**nstruction.
Une fois le programme lancé, vous pouvez utiliser d'autres commandes gdb pour examiner son état réel à l'exécution.
On commence par le code en cours d'exécution, que l'on peut désassembler avec la commande `disassemble` !
Par exemple :

```gdb
(gdb) disassemble
Dump of assembler code for function main:
=> 0x0000000000401000 <+0>:     mov    rdi,0x539
   0x0000000000401007 <+7>:     mov    rdi,0x0
   0x000000000040100e <+14>:    mov    rax,0x3c
   0x0000000000401015 <+21>:    syscall
End of assembler dump.
```

Il s'agit du même programme que dans le défi objdump, cette fois exécuté dans gdb.
Comme précédemment, on peut retrouver ses secrets en lisant le désassemblage, même si nous irons plus loin plus tard !
Pour l'instant, lancez `starti` après avoir chargé le binaire dans gdb, et nous nous occupons du reste.
