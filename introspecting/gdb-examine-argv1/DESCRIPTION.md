Dans le niveau précédent, on a utilisé `x` pour lire `argc` au sommet de la pile.
Mais la pile contient plus que `argc` !

Juste après le nombre d'arguments, la pile stocke des *pointeurs* vers chaque argument du programme.
Ce sont des *adresses* stockées en mémoire : `$rsp+16` ne contient pas directement le texte de l'argument, mais l'*adresse* à laquelle ce texte se trouve.

Par exemple, si le programme est lancé avec `/challenge/debug-me Hi` :

```text
     Address    │ Contents
   +────────────────────────────+
   │  rsp + 0   │ 2             │◀── argc
   +────────────────────────────+
   │  rsp + 8   │ 0x1234000     │──────┐
   +────────────────────────────+      │
   │  rsp + 16  │ 0x1234560     │────┐ │
   +────────────────────────────+    │ │
                                     │ │
                                     │ │
     Address    │ Contents           │ │
   +──────────────────────────────+  │ │
   │ 0x1234000  │ "/challenge/..."│◀─│─┘ the program name
   +──────────────────────────────+  │
   │ ...        │ ...                │
   +──────────────────────────────+  │
   │ 0x1234560  │ "Hi"            │◀─┘   the first argument
   +──────────────────────────────+
```

Pour obtenir les données réelles de l'argument, il faut deux déréférencements : un pour récupérer le pointeur depuis la pile, et un pour suivre ce pointeur jusqu'à la chaîne de caractères.

Dans ce défi, LE FLAG LUI-MÊME est passé comme premier argument !
Le programme ne l'utilise pas, il se termine simplement, mais le flag se trouve bien en mémoire.

Pour le trouver, il faut deux commandes `x`, avec deux formats d'affichage différents :

**Première étape :**
Il faut le *pointeur* vers le premier argument.
On l'a déjà fait, mais cette fois dans gdb.

```text
x/a $rsp+16
```

`/a` demande à `x` d'afficher la valeur comme une **a**dresse mémoire.
On obtient un très grand nombre hexadécimal, de la forme `0x7ffc001c4750`.

**Deuxième étape :**
Lire le texte du premier argument à cette adresse :

```text
x/s 0x7ffc001c4750
```

`/s` demande à `x` d'afficher la valeur comme une chaîne de caractères (**s**tring).
Remplacer l'adresse par celle obtenue à l'étape précédente.
Cela affiche le flag !

Faites-le !

1. Lancer le programme.
2. `x/a $rsp+16` pour obtenir l'adresse du premier argument.
3. `x/s <address>` pour lire la chaîne contenant le flag.
