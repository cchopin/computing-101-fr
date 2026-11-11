Avant chaque exécution, le niveau place dynamiquement des valeurs en mémoire, et ces valeurs changent à chaque fois. Il faut donc effectuer un calcul, sous une forme ou une autre, avec les registres. Le niveau indique quels registres sont préremplis et où placer le résultat. Dans la plupart des cas, c'est `rax`.

Dans ce niveau, on travaille avec la mémoire. Il faut lire ou écrire des données stockées de façon linéaire en mémoire. Si ce point n'est pas clair, le module sur l'adressage linéaire de *Ike* (en anglais) est une bonne référence. On peut aussi devoir déréférencer des adresses, parfois plusieurs fois, vers des données placées dynamiquement en mémoire pour le niveau.

Jusqu'ici, les registres étaient le seul moyen de stocker des valeurs, comme des variables en mathématiques.

On peut aussi stocker des octets en mémoire !

Chaque adresse mémoire désigne un emplacement qui contient quelque chose. C'est comparable aux adresses de la vie courante.

Par exemple, l'adresse réelle « 699 S Mill Ave, Tempe, AZ 85281 » correspond au « ASU Brickyard ». On dit aussi qu'elle pointe vers « ASU Brickyard ». On peut le représenter ainsi :

```
['699 S Mill Ave, Tempe, AZ 85281'] = 'ASU Brickyard'
```

Une adresse est unique. Plusieurs adresses peuvent toutefois pointer vers la même chose, comme une personne qui possède plusieurs maisons.

La mémoire fonctionne exactement de la même façon !

Par exemple, l'adresse en mémoire où se trouve votre code (une fois chargé) est `0x400000`.

En x86, on accède au contenu d'un emplacement mémoire, ce qu'on appelle déréférencer, comme ceci :

```
mov rax, [some_address]        <=>     Moves the thing at 'some_address' into rax
```

Cela fonctionne aussi avec une adresse contenue dans un registre :

```
mov rax, [rdi]         <=>     Moves the thing stored at the address of what rdi holds to rax
```

L'écriture en mémoire fonctionne de la même façon :

```
mov [rax], rdi         <=>     Moves rdi to the address of what rax holds.
```

Ainsi, si `rax` vaut `0xdeadbeef`, alors `rdi` est stocké à l'adresse `0xdeadbeef` :

```
[0xdeadbeef] = rdi
```

Remarque : la mémoire est linéaire. En x86_64, elle va de `0` à `0xffffffffffffffff` (oui, c'est énorme).

Il faut faire ceci : placer la valeur stockée à l'adresse `0x404000` dans `rax`. Vérifier que `rax` contient la valeur d'origine stockée à `0x404000`.
