Dans les derniers niveaux, nous avons :

- utilisé une adresse qu'on donnait (dans un niveau, `133700`, dans un autre, `123400`) pour charger une valeur secrète depuis la mémoire ;
- utilisé une adresse que l'on avait placée dans `rax` pour charger une valeur secrète depuis la mémoire ;
- utilisé une adresse qu'on donnait (dans le niveau précédent, `567800`) pour charger *l'adresse* d'une valeur secrète dans un registre, puis utilisé ce registre comme pointeur pour récupérer la valeur secrète !

Combinons les deux derniers cas.
Dans ce défi, on a rangé notre `SECRET_VALUE` en mémoire à l'adresse `SECRET_LOCATION_1`, puis on a rangé `SECRET_LOCATION_1` en mémoire à l'adresse `SECRET_LOCATION_2`.
Ensuite, on a placé `SECRET_LOCATION_2` dans `rax` !
Le résultat ressemble à ceci, avec `123400` pour `SECRET_LOCATION_1` et `133700` pour `SECRET_LOCATION_2` (dans le vrai défi, ces valeurs seront différentes et cachées) :

```text
      Address │ Contents
    +────────────────────+
┌──▸│ 133700  │ 123400   │─┐
│   +────────────────────+ │
│ ┌▸│ 123400  │ 42       │ │
│ │ +────────────────────+ │
│ └────────────────────────┘
└──────────────────────────┐
                           │
     Register │ Contents   │
    +────────────────────+ │
    │ rax     │ 133700   │─┘
    +────────────────────+
```

Ici, il faut effectuer deux lectures en mémoire : la première déréférence `rax` pour lire `SECRET_LOCATION_1` à l'endroit que pointe `rax` (c'est-à-dire `SECRET_LOCATION_2`), la seconde déréférence le registre qui contient désormais `SECRET_LOCATION_1` pour charger `SECRET_VALUE` dans `rdi`, afin de l'utiliser comme code de retour !

Cela paraît compliqué, mais on a déjà fait pratiquement tout cela.
À assembler le tout !
