Jusqu'ici, on était du côté de l'*appelé* : le défi appelait `solve`, et c'est ce code qui faisait le travail.
Cette fois, c'est l'inverse : `solve` reçoit un *pointeur de fonction* en argument, et **cette fonction doit être appelée** depuis le code.

Un pointeur de fonction est simplement une adresse : une valeur sur 64 bits qui désigne l'emplacement d'un code en mémoire.
Le défi passe le pointeur dans `rdi` (le premier argument de la fonction), donc pour l'appeler, on peut utiliser la forme de `call` qui prend un registre :

```text
call rdi
```

Cela empile l'adresse de l'instruction qui suit le `call` (l'*adresse de retour*), puis saute à l'adresse contenue dans `rdi`.
Quand ce callback finit par exécuter un `ret`, l'exécution de la fonction reprend juste après le `call rdi` : c'est la même paire `call`/`ret` que dans les deux niveaux précédents.

Le callback affiche le flag. À jouer !
