Jusqu'ici, on écrivait une *bibliothèque chargeable* : une fonction que le défi charge et appelle à sa place.
Cette fois, il faut écrire un programme complet : il commence à `_start`, s'exécute seul et se termine lorsqu'il a fini.

Le programme reçoit le nombre comme argument de la ligne de commande.
Au démarrage d'un programme, la pile contient ses arguments : `argc` (le nombre d'arguments) se trouve en `[rsp]`, suivi des pointeurs vers les arguments. `argv[0]` (le nom du programme lui-même) est en `[rsp + 8]`, et `argv[1]` (le premier argument réel) en `[rsp + 16]`.
Le nombre recherché est donc la chaîne pointée par `[rsp + 16]`.

Lire l'argument, le convertir avec `atoi`, puis renvoyer la valeur comme le fait un *programme* : au lieu de la renvoyer dans `rax`, quitter en utilisant cette valeur comme code de retour, avec l'appel système `exit` et la valeur dans `rdi`.
Un code de retour tient sur un seul octet : le nombre donné sera donc compris entre `0` et `255`.

Cette fois, assembler et lier le programme comme un programme normal (sans `-shared`), puis le soumettre :

```console
hacker@dojo:~$ as -o prog.o prog.s
hacker@dojo:~$ ld -o prog prog.o
hacker@dojo:~$ /challenge/check prog
```

Convertir l'argument, puis quitter avec sa valeur.
