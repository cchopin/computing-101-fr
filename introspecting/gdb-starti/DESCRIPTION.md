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
Essayez-la maintenant : nous configurerons gdb pour extraire automatiquement la valeur secrète une fois le programme en cours d'exécution.
