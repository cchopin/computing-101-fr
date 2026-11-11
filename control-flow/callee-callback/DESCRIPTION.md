Jusqu'ici, vous étiez du côté de l'*appelé* : le défi appelait votre `solve`, et c'est vous qui faisiez le travail.
Cette fois, c'est l'inverse : votre `solve` reçoit un *pointeur de fonction* en argument, et **vous devez appeler cette fonction** depuis votre code.

Un pointeur de fonction est simplement une adresse : une valeur sur 64 bits qui désigne l'emplacement d'un code en mémoire.
Le défi passe le pointeur dans `rdi` (le premier argument de votre fonction), donc pour l'appeler, vous pouvez utiliser la forme de `call` qui prend un registre :

```text
call rdi
```

Cela empile l'adresse de l'instruction qui suit le `call` (l'*adresse de retour*), puis saute à l'adresse contenue dans `rdi`.
Quand ce callback finit par exécuter un `ret`, l'exécution de votre fonction reprend juste après votre `call rdi` : c'est la même paire `call`/`ret` que dans les deux niveaux précédents.

Le callback affiche le flag pour vous. À vous de jouer !
