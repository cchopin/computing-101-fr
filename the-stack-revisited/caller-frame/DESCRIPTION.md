En plus de stocker des données temporaires et des adresses de retour, la pile conserve les *variables locales* des fonctions : les données qu'elles utilisent pour leur fonctionnement propre, et qui ne sont pas forcément nécessaires aux autres fonctions du programme.
Dans une situation de sécurité où un attaquant obtient une « exécution de code » à l'intérieur d'un processus, ces variables sont un livre ouvert : rien n'empêche le code d'un processus de lire les données éparpillées dans toute la pile !

Ce défi explore ce principe.
Une fois encore, il faut écrire une fonction `solve` que le défi appelle, mais **le défi ne passe aucun argument**.
À la place, la fonction `caller` du défi a stocké le flag dans ses propres variables locales avant d'appeler `solve`.
Il faut atteindre le « cadre » de l'appelant (on appelle ainsi la partie de la pile qui contient les variables locales d'une fonction et l'adresse de retour vers laquelle elle reviendra) et récupérer ces octets.

**Comment est-ce possible ?**
Reprenons pourquoi c'est possible.
Dans ce défi, la fonction `main` appelle la fonction `caller`, qui appelle ensuite la fonction `solve`.
Juste avant que `caller` n'exécute `call solve`, la pile ressemblait à ceci :

```text
                                          [smaller addresses]
   +───────────────────────────────────+  ◀── rsp, immediately before `call solve`
   │ caller's local region             │
   │ ... your flag is in here ...      │
   +───────────────────────────────────+
   │ caller's saved rbp                │
   +───────────────────────────────────+
   │ return address (back to main)     │
   +───────────────────────────────────+
   │ ... main's frame ...              │
   +───────────────────────────────────+
                                          [larger addresses]
```

L'instruction `call solve` fait deux choses :

1. Elle empile l'adresse de retour (8 octets). **Un push décrémente `rsp`**, donc l'adresse de retour se retrouve à une adresse *plus petite* que ce qui était déjà sur la pile.
2. Elle saute vers le code écrit.

Cette première étape est essentielle : la pile croît à l'envers de ce qu'on pourrait attendre.
`pop` ajoute en fait 8 à `rsp`, et `push` en retire 8.
Ce fonctionnement est contre-intuitif, et il embrouille souvent les débutants.
Si l'on se représente la pile comme une page large de 8 octets, on commence à écrire tout en bas de cette page, et l'on remonte d'une ligne à chaque `push`.
Autrement dit, `pop rdi` équivaut à `mov rdi, [rsp]; add rsp, 8`, et `push rdi` équivaut à `sub rsp, 8; mov [rsp], rdi`.

Parler de la pile sans confusion est presque impossible.
Par exemple, les personnes formées aux mathématiques placent souvent la coordonnée 0 en bas ou à gauche d'une page, alors que les personnes issues du jeu vidéo ou du développement web la placent en haut ou à gauche.
Cela entraîne une grande confusion sur la définition d'une « adresse plus haute » ou « plus basse ».
Chacun a sa façon de gérer ce point.
Dans ce document, faute de place horizontale, les schémas vont de 0 (en haut) à 0xffffffff (en bas). Dans la vie courante, sans contrainte de place, on se représente plutôt la mémoire de « gauche » (0) à « droite » (0xffffffff).

Quoi qu'il en soit, au moment où `solve` commence à s'exécuter, la pile ressemble à ceci :

```text
                                          [smaller addresses, where rsp goes if you grow your own frame]
   +───────────────────────────────────+
   │ return address (back to caller)   │  ◀── rsp points here
   +───────────────────────────────────+
   │ caller's stack frame              │
   │ ... your flag is in here ...      │
   +───────────────────────────────────+
   │ return address (back to main)     │
   +───────────────────────────────────+
   │ ... main's frame ...              │
   +───────────────────────────────────+
                                          [larger addresses]
```

Les variables locales de l'appelant se trouvent à des adresses *plus grandes* que le `rsp`, c'est-à-dire en dessous de `rsp` sur le schéma. Les données recherchées se trouvent dans cette zone.
Pour les trouver, on indexe la mémoire avec un décalage **positif** par rapport à `rsp`.

Dans l'autre sens, avec des décalages négatifs, vers des adresses *plus petites* que `rsp` (au-dessus de `rsp` sur le schéma), on tombe sur de l'espace de pile non alloué.
Il n'y a rien d'utile là-haut (pour l'instant !).

Au début de l'exécution de `solve`, la disposition est la suivante :

```text
   [rsp + 0x00]   your return address (back into caller's code)
   [rsp + 0x08]   first byte of caller's local region
   ...
   [rsp + 0x40]   the flag (copied here by the caller)
   ...
   [rsp + 0x110]  caller's return address (back to main)
```

Tâche : atteindre le cadre de l'appelant, récupérer le flag en `[rsp + 0x40]`, et l'écrire sur la sortie standard avec `write` (on sait déjà appeler la fonction système `write`).
Si ce but est atteint, `solve` affiche le flag !
