On sait maintenant déréférencer des pointeurs en mémoire comme un pro ! Mais un pointeur ne désigne pas toujours directement la donnée recherchée.
Il peut par exemple pointer vers une collection de données (un livre entier, par exemple), et il faut alors viser un endroit précis à l'intérieur de cette collection.

Par exemple, si le pointeur (disons `rdi`) désigne une suite de nombres en mémoire, comme ceci :

```text
    Address │ Contents
  +────────────────────+
┌▸│ 133700  │ 50       │
│ │ 133701  │ 42       │
│ │ 133702  │ 99       │
│ │ 133703  │ 14       │
│ +────────────────────+
│
└────────────────────────┐
                         │
   Register │ Contents   │
  +────────────────────+ │
  │ rdi     │ 133700   │─┘
  +────────────────────+
```


Si l'on veut le deuxième nombre de cette suite, on peut écrire :

```assembly
mov rax, [rdi+1]
```

Très simple !
En termes de mémoire, on appelle ces emplacements des *octets* : chaque adresse mémoire représente un octet précis.
L'exemple ci-dessus accède à la mémoire à 1 octet après l'adresse pointée par `rdi`.
On appelle cet écart de 1 octet un *décalage* (*offset*) : dans cet exemple, le décalage par rapport à l'adresse pointée par `rdi` est de 1.

Pratiquons ce concept.
Comme précédemment, on initialise `rdi` pour qu'il pointe vers la valeur secrète, mais pas *directement* vers elle.
Cette fois, la valeur secrète se trouve à un décalage de 8 octets par rapport à l'adresse pointée par `rdi`, comme ceci :

```text
    Address │ Contents
  +────────────────────+
┌▸│ 31337   │ 0        │
│ │ 31337+1 │ 0        │
│ │ 31337+2 │ 0        │
│ │ 31337+3 │ 0        │
│ │ 31337+4 │ 0        │
│ │ 31337+5 │ 0        │
│ │ 31337+6 │ 0        │
│ │ 31337+7 │ 0        │
│ │ 31337+8 │ ???      │
│ +────────────────────+
│
└────────────────────────┐
                         │
   Register │ Contents   │
  +────────────────────+ │
  │ rdi     │ 31337    │─┘
  +────────────────────+
```

Bien sûr, l'adresse réelle n'est pas `31337`.
Elle sera choisie au hasard et placée dans `rdi`.
Déréférencez `rdi` avec un décalage de `8` pour obtenir le flag !
