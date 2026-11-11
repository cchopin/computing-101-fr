Pourquoi appelle-t-on la pile une *pile* ?
Jusqu'ici, on l'a utilisée comme une zone de mémoire dans laquelle on lit avec `mov`, comme pour n'importe quel déréférencement.
Mais la pile est faite pour être utilisée comme une pile de données : on retire les valeurs une à une, par le sommet, avec `pop` !

L'instruction `pop` est conçue pour cela.
`pop rdi` fait deux choses :

1. Elle lit la valeur située à `[rsp]` et la place dans `rdi` (comme `mov rdi, [rsp]`).
2. Elle ajoute 8 à `rsp`, pour que le pointeur de pile passe à la valeur suivante.

Avec le même exemple que précédemment :

```text
hacker@dojo:~$ /tmp/your-program hello world
```

Avant `pop rdi` :

```text
    Adresse    │ Contenu
  +───────────────────────+
  │ ...        │ ...      │
  +───────────────────────+
┌▸│ 1337000    │ 3        │  ◀── le nombre d'arguments
│ +───────────────────────+
│ | 1337008    | ???      |
│ +───────────────────────+
│
└────────────────────────────┐
                             │
   Registre │ Contenu        │
  +────────────────────────+ │
  │ rsp     │ 1337000      │─┘
  +────────────────────────+
  │ rdi     │ 0            │
  +────────────────────────+
```

Après `pop rdi` :

```text
    Adresse    │ Contenu
  +───────────────────────+
  │ ...        │ ...      │
  +───────────────────────+
  │ 1337000    │ 3        │
  +───────────────────────+
┌▸| 1337008    | ???      |
│ +───────────────────────+
│
└────────────────────────────┐
                             │
   Registre │ Contenu        │
  +────────────────────────+ │
  │ rsp     │ 1337008      │─┘
  +────────────────────────+
  │ rdi     │ 3            │
  +────────────────────────+
```

La valeur `3` a été retirée du sommet de la pile pour être placée dans `rdi`, et `rsp` a avancé de 8 octets, pour pointer vers la valeur suivante.
La donnée située à `1337000` est toujours présente en mémoire, mais pour la pile elle a disparu : `rsp` est passé au-delà.

Dans ce défi, utiliser `pop` pour lire le nombre d'arguments, et quitter avec cette valeur !
