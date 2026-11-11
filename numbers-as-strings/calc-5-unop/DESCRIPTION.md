Jusqu'ici, tous les opérateurs étaient *binaires* : deux opérandes, avec l'opérateur entre les deux.
Les deux derniers sont *unaires* : un seul opérande, précédé de l'opérateur.

- `-` change le signe : `- 5` vaut `-5`.
- `~` inverse chaque bit (NOT bit à bit) : `~ 5` vaut `-6`, car le complément à deux fait que `~x` est égal à `-x - 1`.

La nouveauté consiste à distinguer les deux formes.
Un appel binaire passe trois arguments après le nom du programme (`prog A OP B`), alors qu'un appel unaire n'en passe que deux (`prog OP A`).
C'est donc le **nombre d'arguments** qui détermine la forme, et vous savez déjà où il se trouve : `argc` est en `[rsp]`.
Testez-le *en premier* :

- `argc == 4` : le dispatch binaire déjà écrit (opérateur dans `argv[2]`).
- `argc == 3` : le nouveau dispatch unaire, opérateur dans `argv[1]` et opérande dans `argv[2]`.

Cette séparation permet précisément à `-` d'avoir deux sens : en binaire, `-` soustrait (`12 - 5 = 7`), et en unaire, `-` change le signe (`- 5 = -5`).
Le nombre d'arguments les distingue.

Pour les opérateurs unaires : appliquez `neg` à l'opérande pour `-`, et `not` pour `~`.
Affichez le résultat avec votre `itoa` signé, comme toute autre réponse.

```
- 5   ->  neg  ->  -5
~ 5   ->  not  ->  -6
```

Ajoutez la séparation selon `argc` et les deux branches unaires : un opérateur unaire non reconnu fait quitter, exactement comme un opérateur binaire.
Le shell développe un `~` seul en votre répertoire personnel : mettez-le donc entre guillemets (le `-` peut être saisi directement) :

```console
hacker@dojo:~$ as -o prog.o prog.s
hacker@dojo:~$ ld -o prog prog.o
hacker@dojo:~$ ./prog - 5
-5
hacker@dojo:~$ ./prog '~' 5
-6
hacker@dojo:~$ /challenge/check prog
```

Séparez selon le nombre d'arguments, et gérez les deux opérateurs unaires.
