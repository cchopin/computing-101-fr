La pile contient plus que `argc` et `argv` !
Juste après la liste des arguments, le noyau place les **variables d'environnement** que vous avez découvertes dans le [Linux Luminarium](/linux-luminarium).
Comme `argv`, elles sont stockées sur la pile sous forme de tableau de pointeurs vers des chaînes de caractères. Chaque chaîne contient le nom et la valeur de la variable, par exemple `PATH=/usr/bin:...`, `HOME=/home/hacker` ou `PWN=COLLEGE`.

Si un programme est appelé sans arguments (c'est-à-dire que `argc` vaut 1, et que la seule chaîne de `argv` est le nom du programme lui-même) et avec une seule variable d'environnement nommée `FLAG`, sa disposition de pile initiale peut ressembler à ceci :

```text
     Address    │ Contents
   +────────────────────────+
   │ rsp + 0    │ 1         │ ◀─── argc
   +────────────────────────+
   │ rsp + 8    │ rsp + 128 │──┐  argv[0]: pointer to the program name
   +────────────────────────+  │
   │ rsp + 16   │ 0         │  │  NULL (end of argv)
   +────────────────────────+  │
   │ rsp + 24   │ rsp + 200 │──┼──┐  envp[0]: pointer to the first env var
   +────────────────────────+  │  │
   │ rsp + 32   │ 0         │  │  │  NULL (end of envp)
   +────────────────────────+  │  │
                               │  │
     Address    │ Contents     │  │
   +────────────────────────+  │  │
   │ rsp + 128  │ "/tmp/..."│◀─┘  │  the program name
   +────────────────────────+     │
   │ ...        │ ...       │     │
   +────────────────────────+     │
   │ rsp + 200  │ "FLAG=..."│◀────┘  the first env var: the `FLAG` variable
   +────────────────────────+
```

Deux points sont à remarquer :

1. `argv` et `envp` sont **terminés par un pointeur NULL** : le noyau écrit un pointeur `NULL` à la fin de chaque liste de pointeurs.
   C'est ainsi que les programmes (et vous !) savent où se termine chaque liste : on parcourt les pointeurs jusqu'à rencontrer un `NULL`.
   Sur le schéma, le `NULL` en `rsp+16` marque la fin de `argv`, et un autre en `rsp+32` marque la fin de `envp`.

2. Les chaînes de `envp` ont la forme `NOM=VALEUR` (par exemple `PATH=/usr/bin:/bin`).
   Ainsi, `envp[0]` pointe vers une chaîne qui commence par le nom de la première variable d'environnement.

Dans ce défi, nous définissons la variable d'environnement `FLAG` avec le flag réel, puis nous lançons votre programme sans argument et sans autre variable d'environnement.
Autrement dit, `[rsp+24]` contiendra un pointeur vers la chaîne `FLAG=...`, et vous obtiendrez le flag en l'affichant avec `write()` !

C'est un défi de programme complet : il faut donc soumettre un exécutable, et non une bibliothèque partagée.
Assemblez et liez votre programme, puis transmettez l'exécutable au correcteur :

```console
hacker@dojo:~$ as -o envp.o envp.s
hacker@dojo:~$ ld -o envp envp.o
hacker@dojo:~$ /challenge/check envp
```
