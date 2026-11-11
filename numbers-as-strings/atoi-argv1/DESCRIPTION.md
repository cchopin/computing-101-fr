Jusqu'ici, vous écriviez une *bibliothèque chargeable* : une fonction que le défi charge et appelle à votre place.
Cette fois, vous écrirez un programme complet : il commence à `_start`, s'exécute seul et se termine lorsqu'il a fini.

Votre programme reçoit le nombre comme argument de la ligne de commande.
Au démarrage d'un programme, la pile contient ses arguments : `argc` (le nombre d'arguments) se trouve en `[rsp]`, suivi des pointeurs vers les arguments. `argv[0]` (le nom du programme lui-même) est en `[rsp + 8]`, et `argv[1]` (le premier argument réel) en `[rsp + 16]`.
Le nombre recherché est donc la chaîne pointée par `[rsp + 16]`.

Lisez-le, convertissez-le avec votre `atoi`, puis renvoyez la valeur comme le fait un *programme* : au lieu de la renvoyer dans `rax`, quittez en utilisant cette valeur comme code de retour, avec l'appel système `exit` et la valeur dans `rdi`.
Un code de retour tient sur un seul octet : le nombre donné sera donc compris entre `0` et `255`.

Cette fois, assemblez et liez-le comme un programme normal (sans `-shared`), puis soumettez-le :

```console
hacker@dojo:~$ as -o prog.o prog.s
hacker@dojo:~$ ld -o prog prog.o
hacker@dojo:~$ /challenge/check prog
```

Convertissez l'argument, puis quittez avec sa valeur.
