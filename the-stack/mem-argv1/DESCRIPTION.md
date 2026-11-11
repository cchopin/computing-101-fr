On a déjà lu `[rsp]` pour obtenir le nombre d'arguments, et `[rsp+128]` pour lire une donnée à un décalage donné.
Regardons ce qui se trouve d'autre sur la pile !

Juste après le nombre d'arguments, la pile contient des *pointeurs* vers chaque argument du programme.
Ce sont des *adresses* stockées en mémoire : `[rsp+16]` ne contient pas directement le texte de l'argument, mais l'*adresse* à laquelle ce texte se trouve.

Par exemple, si le programme est lancé avec `/tmp/your-program Hi` :

```text
     Registre │ Contenu
   +───────────────────────────+
   │ rsp      │ 1337000        │─┐
   +───────────────────────────+ │
                                 │
  ┌──────────────────────────────┘
  │
  │    Adresse    │ Contenu
  │  +────────────────────────+
  │  │ ...        │ ...       │
  │  +────────────────────────+
  └▸ │ 1337000    │ 2         │  ◀── le nombre d'arguments (argc)
     +────────────────────────+
     │ 1337008    │ 1234000   │──────┐
     +────────────────────────+      │
     │ 1337016    │ 1234560   │────┐ │
     +────────────────────────+    │ │
     │ 1337024    │ 0         │    │ │
     +────────────────────────+    │ │
                                   │ │
   ┌───────────────────────────────┘ │
   │                                 │
   │   Adresse   │ Contenu           │
   │ +──────────────────────────+    │
   │ │ 1234000   │ "/tmp/..."   │◀───┘ le nom du programme
   │ +──────────────────────────+
   │ │ ...       │ ...          │
   │ +──────────────────────────+
   └▸│ 1234560   │ "Hi"         │ le premier argument !
     +──────────────────────────+
```

Pour obtenir les données de l'argument, il faut déréférencer deux fois : une fois pour lire le pointeur sur la pile, et une fois pour suivre ce pointeur jusqu'aux données.

```assembly
mov rdi, [rsp+16]   # charger le pointeur du premier argument (par exemple 1234560) depuis la pile
mov rdi, [rdi]      # suivre le pointeur pour lire les données réelles (par exemple "Hi")
```

Dans ce défi, le programme est lancé avec un argument.
Lisez la valeur du premier argument et quittez avec cette valeur !
