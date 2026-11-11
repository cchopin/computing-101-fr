Apprenons à écrire du texte !

Sans surprise, le programme écrit du texte à l'écran en invoquant un appel système.
Il s'agit de l'appel système `write`, dont le numéro est `1`.
Mais l'appel `write` doit aussi préciser, par ses paramètres, *quelles* données écrire et *où* les écrire.

Vous vous souvenez peut-être, dans le module [Practicing Piping](/linux-luminarium/piping) du dojo [Linux Luminarium](/linux-luminarium), de la notion de *descripteurs de fichiers* (file descriptors, FD).
Pour rappel, chaque processus démarre avec trois descripteurs :

- **FD 0 :** l'*entrée standard* (Standard Input) est le canal par lequel le processus reçoit ses données. Par exemple, le shell utilise l'entrée standard pour lire les commandes que l'on tape.
- **FD 1 :** la *sortie standard* (Standard Output) est le canal par lequel le processus affiche ses données normales, comme le flag affiché dans les défis précédents, ou la sortie de programmes comme `ls`.
- **FD 2 :** l'*erreur standard* (Standard Error) est le canal par lequel le processus affiche ses messages d'erreur. Par exemple, si l'on tape une commande inexistante, le shell affiche sur l'erreur standard que cette commande n'existe pas.

Dans l'appel système `write`, c'est ainsi qu'on indique *où* écrire les données !
Le premier (et unique) paramètre de l'appel `exit` était le code de retour (`mov rdi, 42`). Le premier paramètre de `write` est le descripteur de fichier, et il n'est pas le seul.
Pour écrire sur la sortie standard, on place 1 dans `rdi`.
Pour écrire sur l'erreur standard, on place 2 dans `rdi`.
C'est simple !

Reste à dire *quoi* écrire.
On pourrait imaginer passer les données par un autre registre paramètre de l'appel `write`.
Mais ces registres ne contiennent pas beaucoup de données, et pour écrire un long texte comme celui de ce défi, il faudrait invoquer `write` plusieurs fois.
Cela coûte cher : le processeur doit passer des instructions du programme aux instructions de Linux, effectuer une série de calculs de gestion, interagir avec le matériel pour afficher les pixels à l'écran, puis revenir au programme.
C'est lent, et l'on cherche donc à limiter le nombre d'appels système.

La solution consiste à écrire plusieurs caractères à la fois.
L'appel `write` prend pour cela *deux* paramètres supplémentaires : une adresse *en mémoire* à partir de laquelle commencer l'écriture, et le *nombre* de caractères à écrire.
Ces paramètres sont transmis comme deuxième et troisième paramètres de `write`.
Dans la syntaxe pseudo-C vue avec `strace`, cela donne :

```c
write(file_descriptor, memory_address, number_of_characters_to_write)
```

Par exemple, pour écrire 10 caractères depuis l'adresse mémoire `1337000` vers la sortie standard (descripteur 1), on écrit :

```c
write(1, 1337000, 10);
```

Bien simple !
Reste à savoir comment transmettre ces paramètres.

1. Le premier paramètre d'un appel système, comme vu plus haut, est transmis par le registre `rdi`.
2. Le deuxième paramètre est transmis par le registre `rsi`.
   La convention de Linux veut que `rsi` serve de deuxième paramètre aux appels système.
3. Le troisième paramètre est transmis par le registre `rdx`.
   C'est la partie la plus déroutante de ce module : `rdi`, qui contient le premier paramètre, ressemble tellement à `rdx` qu'on les confond facilement. Le nommage est ainsi pour des raisons historiques, et il n'y a rien à y faire.
   Il faut donc faire attention.
   Un moyen mnémotechnique peut être « `rdi` est le premier paramètre (*d*ébut, en anglais *i*nitial), `rdx` est le paramètre supplémentaire (*x*tra) ».
   Ou bien, voyez-les comme deux amis aux noms proches, et vous ne vous tromperez pas.

Et, bien sûr, le numéro de l'appel `write` se place dans `rax` : `1`.
À part la confusion entre `rdi` et `rdx`, c'est très simple !

Vous savez désormais pointer un registre vers une adresse mémoire (voir le module [Memory](../memory)), vous savez donner le numéro d'un appel système, et vous savez remplir les autres registres.
Ce défi devrait donc être un jeu d'enfant !

Comme précédemment, on a écrit un seul caractère secret en mémoire, à l'adresse `1337000`.
Appelez `write` pour écrire ce seul caractère sur la sortie standard (pour l'instant ! Nous verrons les écritures de plusieurs caractères plus tard), et vous obtiendrez le flag !
