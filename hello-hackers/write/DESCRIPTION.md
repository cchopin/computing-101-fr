Apprenons à écrire du texte !

Sans surprise, votre programme écrit du texte à l'écran en invoquant un appel système.
Il s'agit de l'appel système `write`, dont le numéro est `1`.
Cependant, l'appel `write` doit aussi indiquer, par ses paramètres, *quoi* écrire et *où* l'écrire.

Vous vous souvenez peut-être, du module [Practicing Piping](https://pwn.college/linux-luminarium/piping) du dojo [Linux Luminarium](https://pwn.college/linux-luminarium), de la notion de *descripteurs de fichiers* (file descriptors, FD).
Pour rappel, chaque processus démarre avec trois FD :

- **FD 0 :** l'*entrée standard* (Standard Input) est le canal par lequel le processus reçoit ses données. Par exemple, le shell lit par ce canal les commandes que l'on tape.
- **FD 1 :** la *sortie standard* (Standard Output) est le canal par lequel les processus écrivent leurs données normales, comme le flag affiché dans les défis précédents, ou la sortie d'utilitaires comme `ls`.
- **FD 2 :** l'*erreur standard* (Standard Error) est le canal par lequel les processus écrivent les détails d'erreur. Par exemple, si l'on tape mal une commande, le shell écrit sur l'erreur standard que cette commande n'existe pas.

C'est dans l'appel `write` que l'on indique *où* écrire les données !
Le premier (et unique) paramètre de l'appel `exit` était le code de retour (`mov rdi, 42`), et le premier paramètre de `write` est le descripteur de fichier (le premier, mais pas le seul, dans ce cas !).
Pour écrire sur la sortie standard, on place 1 dans `rdi`. Pour écrire sur l'erreur standard, on place 2 dans `rdi`.
Simple !

Reste à savoir *quoi* écrire.
On pourrait imaginer qu'un autre registre paramètre précise les données à écrire.
Mais ces registres ne contiennent pas beaucoup de données, et pour écrire un long texte comme celui-ci, il faudrait invoquer `write` plusieurs fois.
Cela coûte cher : le processeur doit passer des instructions du programme à celles du noyau Linux, effectuer des calculs de gestion, dialoguer avec le matériel pour afficher les pixels à l'écran, puis revenir au programme.
C'est lent, et l'on cherche donc à limiter le nombre d'appels système.

La solution consiste à écrire plusieurs caractères à la fois.
L'appel `write` le permet en prenant *deux* paramètres pour le « quoi » : un *où* en mémoire (l'adresse de départ) et un *combien* de caractères à écrire.
Ces paramètres sont passés en deuxième et troisième positions dans `write`.
Dans la syntaxe pseudo-C vue avec `strace`, cela donne :

```c
write(file_descriptor, memory_address, number_of_characters_to_write)
```

Par exemple, pour écrire 10 caractères à partir d'une adresse mémoire donnée vers la sortie standard (descripteur 1), on écrit :

```c
write(1, memory_address, 10);
```

Simple, n'est-ce pas ?
Reste à voir comment placer ces paramètres.

1. Le premier paramètre d'un appel système, comme vu plus haut, est placé dans le registre `rdi`.
2. Le deuxième paramètre est placé dans `rsi`. Sous Linux, la convention veut que `rsi` soit le deuxième paramètre des appels système.
3. Le troisième paramètre est placé dans `rdx`. C'est la partie la plus délicate de tout le module : `rdi` et `rdx` se ressemblent tellement qu'on les confond facilement. Ce nommage remonte à l'histoire, et il est là pour de bon.
   Il faut donc faire attention.
   Un moyen mnémotechnique : « `rdi` est le paramètre **i**nitial, `rdx` le paramètre e**x**tra » ?
   Ou bien, on peut voir ces deux registres comme deux amis aux prénoms proches : on finit par ne plus les confondre.

Enfin, le numéro de l'appel `write`, `1`, est placé dans `rax`.
À part la confusion entre `rdi` et `rdx`, c'est très simple !

On sait maintenant placer le numéro d'appel système et les autres registres.
Mais où se trouvent en mémoire les données à écrire ?

Dans ce défi, le programme reçoit un argument sur la ligne de commande, par exemple :

```text
/tmp/your-program H
```

Quand un programme est lancé avec des arguments, la pile contient des *pointeurs* vers chacun d'eux.
Ce sont des *adresses* stockées en mémoire : `[rsp+16]` ne contient pas le texte de l'argument, mais l'*adresse* à laquelle ce texte se trouve.

Pour obtenir l'adresse du premier argument, il suffit donc de charger le pointeur depuis la pile, comme on l'a déjà fait !

```asm
mov rsi, [rsp+16]
```

Cette instruction place dans `rsi` l'adresse du texte du premier argument, ce que `write` attend comme deuxième paramètre !

Le programme reçoit un seul caractère comme premier argument.
Il faut appeler `write` pour écrire ce caractère (pour l'instant ! Les écritures de plusieurs caractères viendront plus tard) sur la sortie standard : le flag est alors affiché.
