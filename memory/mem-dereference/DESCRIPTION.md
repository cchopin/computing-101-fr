Préfère-t-on accéder à la mémoire à `133700` ou à `123400` ?
Ce choix en dit peut-être long sur la personnalité, mais ce n'est pas très important d'un point de vue technique.
En réalité, la plupart du temps, on ne manipule pas d'adresses mémoire réelles en écrivant des programmes.

Comment est-ce possible ?
En général, les adresses mémoire sont stockées dans des registres, et l'on utilise les valeurs contenues dans les registres pour pointer vers des données en mémoire !
Commençons avec cette configuration de la mémoire :

```text
  Address │ Contents
+────────────────────+
│ 133700  │ 42       │
+────────────────────+
```

Et considérons cet extrait d'assembleur :

```assembly
mov rax, 133700
```

On obtient alors la situation suivante :

```text
    Address │ Contents
  +────────────────────+
┌▸│ 133700  │ 42       │
│ +────────────────────+
│
└────────────────────────┐
                         │
   Register │ Contents   │
  +────────────────────+ │
  │ rax     │ 133700   │─┘
  +────────────────────+
```

`rax` contient maintenant une valeur qui correspond à l'adresse de la donnée que l'on veut charger !
Chargeons-la :

```assembly
mov rdi, [rax]
```

Ici, on accède à la mémoire, mais au lieu d'indiquer une adresse fixe comme `133700`, on utilise la valeur stockée dans `rax` comme adresse.
En contenant l'adresse mémoire, `rax` est un *pointeur* qui *pointe vers* la donnée à lire !
Lorsqu'on utilise `rax` à la place de l'adresse qu'il contient pour accéder à la donnée référencée, on parle de *déréférencer* le pointeur.
Dans l'exemple ci-dessus, on *déréférence* `rax` pour charger dans `rdi` la donnée qu'il pointe (la valeur `42` à l'adresse `133700`).
Joli !

Cela met aussi en évidence un autre point : ces registres sont à *usage général* !
Le fait que `rax` ait servi jusqu'ici de numéro d'appel système dans nos défis n'empêche pas de l'utiliser à d'autres fins.
Ici, il sert de pointeur vers notre donnée secrète en mémoire.

De même, les *données* contenues dans les registres n'ont pas de rôle implicite.
Si `rax` contient la valeur `133700` et que l'on écrit `mov rdi, [rax]`, le processeur utilise cette valeur comme adresse à déréférencer.
Mais si l'on écrit `mov rdi, rax` dans les mêmes conditions, le processeur place simplement `133700` dans `rdi`.
Pour le processeur, une donnée reste une donnée : elle ne prend un sens particulier que selon la façon dont on l'utilise.

Dans ce défi, `rax` a été initialisé avec l'adresse de la donnée secrète rangée en mémoire.
Déréférencer `rax` pour charger la donnée secrète dans `rdi`, et l'utiliser comme code de retour du programme pour obtenir le flag !
