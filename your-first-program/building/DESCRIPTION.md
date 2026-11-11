Vous avez donc écrit votre premier programme ?
Jusqu'ici, c'est nous qui nous sommes chargés de le transformer en exécutable que le processeur peut lancer.
Dans ce défi, c'est vous qui le construisez !

Pour construire un exécutable binaire, il faut :

1. Écrire le code d'assembleur dans un fichier (souvent avec l'extension `.S` ou `.s`. Dans cet exemple, on utilise `program.s`).
2. Assembler le fichier en un *fichier objet* (object file), avec la commande `as`.
3. Lier un ou plusieurs fichiers objets exécutables pour produire le binaire final, avec la commande `ld` !

Reprenons ces étapes une à une.

**Écrire l'assembleur.**  
Le fichier d'assembleur contient, tout simplement, le code d'assembleur.
Pour le niveau précédent, il pourrait ressembler à ceci :

```console
hacker@dojo:~$ cat program.s
mov rdi, 42
mov rax, 60
syscall
hacker@dojo:~$
```

Il faut cependant ajouter une information supplémentaire.
Ce cours utilise la syntaxe Intel de l'assembleur, et il faut l'indiquer à l'assembleur.
Pour cela, on place une directive au début du code, comme ceci :

```console
hacker@dojo:~$ cat program.s
.intel_syntax noprefix
mov rdi, 42
mov rax, 60
syscall
hacker@dojo:~$
```

`.intel_syntax noprefix` indique à l'assembleur qu'on utilise la syntaxe Intel, et plus précisément la variante où il n'est pas nécessaire d'ajouter de préfixe supplémentaire à chaque instruction.
Ce n'est pas une instruction x86 (comme `mov` ou `syscall`) : elle n'apparaît donc pas dans le binaire final et n'est pas exécutée par le processeur.
Les autres directives seront vues plus tard. Pour l'instant, on laisse l'assembleur s'en charger !

**Assembler le code en fichier objet.**  
Ensuite, on assemble le code, avec l'outil `as` (pour **as**sembler) :

```console
hacker@dojo:~$ ls
program.s
hacker@dojo:~$ cat program.s
.intel_syntax noprefix
mov rdi, 42
mov rax, 60
syscall
hacker@dojo:~$ as -o program.o program.s
hacker@dojo:~$ ls
program.o   program.s
hacker@dojo:~$
```

Ici, `as` lit `program.s`, le transforme en code binaire et produit un *fichier objet* nommé `program.o`.
Ce fichier contient du code binaire assemblé, mais il n'est pas encore prêt à être exécuté.
Il faut d'abord le *lier*.

**Lier les fichiers objets en exécutable.**  
Dans un flux de développement classique, le code source est compilé et l'assembleur est assemblé en fichiers objets. Il y en a généralement plusieurs, car chaque fichier source d'un programme donne en principe son propre fichier objet.
Ces fichiers sont ensuite *liés* entre eux pour former un seul exécutable.
Même lorsqu'il n'y a qu'un seul fichier, il faut tout de même le lier pour préparer l'exécutable final.
Cette étape est réalisée par la commande `ld` (dont le nom vient de l'expression « **l**ink e**d**itor », c'est-à-dire *éditeur de liens*) :

```console
hacker@dojo:~$ ls
program.o   program.s
hacker@dojo:~$ ld -o program program.o
ld: warning: cannot find entry symbol _start; defaulting to 0000000000401000
hacker@dojo:~$ ls
program.o   program.s   program
hacker@dojo:~$
```

Cela crée un fichier `program` que l'on peut exécuter !
Voici son exécution :

```console
hacker@dojo:~$ ./program
hacker@dojo:~$ echo $?
42
hacker@dojo:~$
```

Dans le shell, `$?` contient le code de retour de la dernière commande exécutée.

C'est pratique !
On sait désormais construire des programmes.
Dans ce défi, il faut suivre ces étapes soi-même : construire l'exécutable, puis le passer à `/challenge/check` pour obtenir le flag !

---
**Le symbole `_start` ?**  
Le lecteur attentif aura remarqué que `ld` affiche un avertissement au sujet de `entry symbol _start` (symbole d'entrée _start).
Le symbole `_start` indique à `ld` l'endroit où l'exécution doit commencer lorsque le fichier ELF est lancé.
L'avertissement précise qu'en l'absence de `_start`, l'exécution commence au tout début du code.
C'est parfaitement suffisant pour ce dojo !

Pour faire disparaître le message, on peut déclarer le symbole `_start` dans le code, comme ceci :

```console
hacker@dojo:~$ cat program.s
.intel_syntax noprefix
.global _start
_start:
mov rdi, 42
mov rax, 60
syscall
hacker@dojo:~$ as -o program.o program.s
hacker@dojo:~$ ld -o program program.o
hacker@dojo:~$ ./program
hacker@dojo:~$ echo $?
42
hacker@dojo:~$
```

Deux lignes ont été ajoutées.
La seconde, `_start:`, crée une *étiquette* (label) nommée `_start`, qui pointe vers le début du code.
La première, `.global _start`, demande à `as` de rendre l'étiquette `_start` *globalement visible* au niveau de l'éditeur de liens, et non seulement à l'intérieur du fichier objet.
Comme `ld` est l'éditeur de liens, cette directive est nécessaire pour que `_start` soit visible.

Pour tous les défis de ce dojo, commencer l'exécution au début du fichier suffit. Mais si l'on ne veut pas voir ces avertissements, on sait maintenant comment les éviter !
