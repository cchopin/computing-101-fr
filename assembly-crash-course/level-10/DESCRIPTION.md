Avant chaque exécution, le niveau place dynamiquement des valeurs en mémoire, et ces valeurs changent à chaque fois. Il faut donc effectuer un calcul, sous une forme ou une autre, avec les registres. Le niveau indique quels registres sont préremplis et où placer le résultat. Dans la plupart des cas, c'est `rax`.

Dans ce niveau, on travaille avec la mémoire. Il faut lire ou écrire des données stockées de façon linéaire en mémoire. Si ce point n'est pas clair, le module sur l'adressage linéaire de *Ike* (en anglais) est une bonne référence. On peut aussi devoir déréférencer des adresses, parfois plusieurs fois, vers des données placées dynamiquement en mémoire pour le niveau.

Il faut faire ceci :
- Placer la valeur stockée à l'adresse `0x404000` dans `rax`.
- Incrémenter la valeur stockée à l'adresse `0x404000` de `0x1337`.

Vérifier que `rax` contient la valeur d'origine stockée à `0x404000`, et que `[0x404000]` contient maintenant la valeur incrémentée.
