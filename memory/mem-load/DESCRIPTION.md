Vue depuis un programme, la mémoire de l'ordinateur est un immense endroit où les données sont rangées.
Comme les maisons d'une rue, chaque emplacement de la mémoire a une *adresse* numérique, et ces numéros se suivent (en général).
Les ordinateurs modernes disposent d'une mémoire énorme, et la vue qu'a un programme typique de sa mémoire présente de grands trous. Imaginons une portion de rue où aucune maison n'a encore été construite : les adresses correspondantes sont sautées.
Ce ne sont que des détails : l'essentiel est que les ordinateurs stockent les données en mémoire, en général de façon séquentielle.

Dans ce défi, on pratique l'accès aux données stockées en mémoire.
Comment faire ?
Rappelons que pour placer une valeur dans un registre, on écrit par exemple :

```assembly
mov rdi, 31337
```

Après cela, la valeur de `rdi` est `31337`.
Très bien. Or on peut utiliser la même instruction pour accéder à la mémoire !
Il existe une autre forme de la commande, qui utilise le second paramètre comme une adresse pour accéder à la mémoire.
Supposons que la mémoire ressemble à ceci :

```text
  Address │ Contents
+────────────────────+
│ 31337   │ 42       │
+────────────────────+
```

Pour accéder au contenu de la mémoire à l'adresse 31337, on écrit :

```assembly
mov rdi, [31337]
```

Quand le processeur exécute cette instruction, il comprend que `31337` est une *adresse* et non une valeur brute.
Si l'on considère l'instruction comme une personne qui donne des consignes au processeur, et si l'on garde l'analogie de la rue, l'instruction *désigne une maison de la rue* au lieu de donner directement la donnée.
Le processeur se rend alors à cette adresse, sonne à la porte, ouvre, sort la donnée qui s'y trouve et la place dans `rdi`.
Ainsi, le `31337` de cet exemple est l'*adresse mémoire* : il sert à *pointer vers* la donnée stockée à cet endroit.
Après l'exécution de cette instruction, la valeur de `rdi` vaut `42` !

Mettons cela en pratique !
J'ai rangé un nombre secret à l'adresse mémoire `133700`, comme ceci :

```text
  Address │ Contents
+────────────────────+
│ 133700  │ ???      │
+────────────────────+
```

Il faut récupérer ce nombre secret et l'utiliser comme code de retour du programme.
Pour cela, on le charge dans `rdi`, dont la valeur, rappelons-le, est le premier paramètre de `exit` et sert de code de retour.
Bon courage !

----
*NOTE* : pour résoudre ce défi, assembler et lier le code, puis passer à `/challenge/check` le binaire exécutable.
