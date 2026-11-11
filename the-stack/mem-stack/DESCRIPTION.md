Jusqu'ici, on a lu des données à des adresses mémoire que nous avons données : soit en dur (comme `133700`), soit stockées dans un registre (comme `rax`).
Mais il existe une zone de mémoire à laquelle le programme accède sans aucune préparation de notre part : la **pile** (stack).

La pile est une zone de mémoire allouée, utilisée comme espace de travail temporaire par le programme. Le registre `rsp` (*Stack Pointer*, pointeur de pile) pointe sur son sommet.
On reviendra plus tard sur la pile. Pour l'instant, l'élément important est le suivant : au démarrage d'un programme, `rsp` pointe vers une donnée qui indique le nombre d'arguments de la ligne de commande, y compris le nom du programme lui-même.

Si l'on lance :

```text
hacker@dojo:~$ /tmp/your-program hello world
```

la situation ressemble à ceci (les adresses réelles sont données à titre d'exemple) :

```text
    Adresse    │ Contenu
  +───────────────────────+
  │ ...        │ ...      │
  +───────────────────────+
┌▸│ 1337000    │ 3        │  ◀── le nombre d'arguments
| +───────────────────────+
| | 1337008    | ???      |
| +───────────────────────+
| | 1337016    | ???      |
│ +───────────────────────+
│
└────────────────────────────┐
                             │
   Registre │ Contenu        │
  +────────────────────────+ │
  │ rsp     │ 1337000      │─┘
  +────────────────────────+
```

`rsp` pointe vers la pile, et la valeur qui s'y trouve est `3` : une pour le nom du programme, une pour `hello`, une pour `world`.
La pile contient aussi d'autres données, comme on le voit sur le schéma, mais on ne s'en occupe pas pour l'instant.

Dans ce défi, lisez le nombre d'arguments à `[rsp]` et utilisez-le comme code de retour du programme.
Le programme sera lancé plusieurs fois avec différents arguments, pour vérifier que la lecture est correcte !
