Dans le défi précédent, le mot de passe a été retrouvé en lisant des paires `cmp`/`jne` une par une.
Cette technique teste chaque possibilité l'une après l'autre : comparer, brancher, comparer, brancher...
Mais que faire si un programme doit aller vers l'une de *nombreuses* destinations selon une seule valeur ?

Il existe une méthode plus efficace : la **table de sauts** (jump table).
C'est un tableau d'adresses stocké en mémoire, une par destination possible (appelée **cas**).
Au lieu de comparer l'entrée à chaque possibilité, le programme utilise la valeur d'entrée comme *indice* dans la table, charge l'adresse située à cette position, et y saute.
Ce schéma s'appelle un **switch** (aiguillage), et c'est un élément fondamental des programmes.

Dans le désassemblage, on verra quelque chose comme :

```asm
mov    rax, 0                    ; mettre rax à zéro
mov    al, BYTE PTR [rcx]        ; charger le caractère dans l'octet de poids faible de rax
mov    rax, [rax*8+0x1234000]    ; charger une adresse stockée dans la table de sauts à 0x1234000
jmp    rax                       ; sauter vers elle
```

`dil` (l'octet de poids faible de `rdi`) est déjà connu : `al` est la même idée pour `rax`.
Écrire dans `al` ne modifie que les 8 bits de poids faible, et laisse le reste de `rax` intact.
C'est pourquoi le code met d'abord `rax` à zéro : cela garantit que les octets de poids fort valent `0`, donc après `mov al, [rcx]`, `rax` ne contient que la valeur du caractère (de 0 à 255).

La valeur du caractère sert directement d'indice dans une table de 256 entrées (une par valeur possible d'octet).
Chaque entrée est une adresse de **8 octets** qui pointe vers le code du cas correspondant.
Le programme implémente ainsi une logique conditionnelle *sans aucun saut conditionnel* !

Ce défi (dans `/challenge/reverse-me`) comporte 256 cas possibles, dont un seul, correspondant à un caractère alphanumérique, se comporte différemment des autres.
Examiner la table de sauts (il faudra regarder beaucoup d'entrées...), étudier comment le programme influence l'indice, et obtenir le flag !

----

Avec gdb, donner à `starti` ou `run` un argument d'un seul octet (par exemple `starti A`), pour que le programme ait un `argv[1]` à lire.

**NOTE :**
Il faut examiner le désassemblage avec `objdump -d -M intel /challenge/reverse-me`, mais objdump tentera d'interpréter les données de la table de sauts comme des instructions assembleur, ce qui produira du charabia.
Ignorer cette partie du désassemblage : il faut regarder ces données dans gdb.

**INDICE :**
gdb sera probablement nécessaire tout au long de ce défi, et `x/a` sera le meilleur allié.
Par exemple, si gdb est arrêté à l'instruction `mov rax, [rax*8+0x1337000]` (l'adresse sera différente selon les exécutions), on peut examiner les entrées de la table :

```gdb
(gdb) print $rax
1
(gdb) x/a $rax*8+0x1337000
0x1337008:  0x400100
(gdb) x/a 2*8+0x1337000
0x1337010:  0x400100
(gdb) x/a 98*8+0x1337000
0x1337310:  0x400200
(gdb)
```

Une fois l'entrée de la table qui pointe ailleurs trouvée, convertir sa position dans la table en octet d'entrée.
Utiliser l'adresse de la case de la table (l'adresse à gauche de la sortie de `x/a`), et non l'adresse stockée à l'intérieur.
Soustraire l'adresse de base de la table pour obtenir son décalage, puis diviser par 8, car chaque entrée de la table fait 8 octets.
Dans l'exemple ci-dessus, l'entrée inhabituelle est à `0x1337310`, donc `0x1337310 - 0x1337000 = 0x310`, et `0x310 / 8 = 98`.
L'octet d'entrée vaut donc `98`, que l'ASCII représente par `'b'`.

**INDICE :**
On peut aussi afficher toute la table de sauts en une fois.
La sortie sera longue, mais elle commence ainsi :

```gdb
(gdb) x/256a 0x1337000
0x1337000:  0x400100  0x400100
0x1337010:  0x400100  0x400100
...
(gdb)
```

Le nombre après `x/` indique combien d'entrées gdb doit afficher.
Comme l'octet d'entrée choisit parmi 256 entrées, chacune de 8 octets (une adresse de code), cela permet de parcourir la table pour trouver l'adresse qui diffère.
Si gdb affiche plusieurs entrées sur une même ligne, l'adresse à gauche est celle de la première entrée de la ligne ; la suivante se trouve 8 octets plus loin.
Comme pour le défi précédent, gdb sert à comprendre le binaire, puis `/challenge/reverse-me` se lance directement avec l'octet retrouvé pour obtenir le flag.
