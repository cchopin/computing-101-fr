Ce niveau porte sur les registres : il faut modifier leur contenu ou le lire.

Les valeurs sont placées dynamiquement en mémoire avant chaque exécution, donc elles changent d'une fois à l'autre. Il faut effectuer un calcul sur les registres. Le niveau indique quels registres sont préremplis et où placer le résultat, en général `rax`.

La division en assembleur est plus particulière que la division mathématique habituelle. Il s'agit de la division entière : toutes les valeurs sont des nombres entiers.

Par exemple : `10 / 3 = 3` en division entière.

Pourquoi ?

Parce que `3.33` est arrondi à l'entier inférieur.

Les instructions utiles pour ce niveau sont :
- `mov rax, reg1`
- `div reg2`

Remarque : `div` est une instruction particulière qui divise un dividende de 128 bits par un diviseur de 64 bits, et qui produit à la fois le quotient et le reste, en ne prenant qu'un seul registre comme opérande.

Comment fonctionne cette instruction `div`, qui opère sur un dividende de 128 bits, soit le double de la taille d'un registre ?

Pour l'instruction `div reg`, il se passe ceci :
- `rax = rdx:rax / reg`
- `rdx = remainder` (reste)

`rdx:rax` signifie que `rdx` contient les 64 bits de poids fort du dividende de 128 bits, et `rax` les 64 bits de poids faible.

Il faut faire attention à ce qui se trouve dans `rdx` et `rax` avant d'appeler `div`.

Il faut calculer :
- `speed = distance / time` (vitesse = distance / temps), où :
  - `distance = rdi`
  - `time = rsi` (temps)
  - `speed = rax` (vitesse)

La distance tient au plus sur 64 bits, donc `rdx` doit valoir 0 avant la division.
