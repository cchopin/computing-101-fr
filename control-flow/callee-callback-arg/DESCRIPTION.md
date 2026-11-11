Dans le niveau précédent, le callback a été appelé avec `call rdi` : facile, puisque le pointeur était déjà dans `rdi`.
Cette fois, il faut l'appeler **avec `1337` comme premier argument**.

La convention d'appel veut que le premier argument soit placé dans `rdi`, le même registre que celui qui contient le pointeur de fonction (puisque celui-ci a été passé comme premier argument à la fonction `solve`).
Cela pose un problème :

- si `1337` est placé dans `rdi`, le pointeur est écrasé, et `call rdi` tentera d'appeler une fonction à l'adresse `1337`, ce qui fera planter le programme ;
- si `call rdi` est fait avec `rdi` intact, le pointeur de fonction est passé comme premier argument au lieu de `1337`, et le flag n'est pas obtenu.

C'est une illustration d'une réalité fondamentale de l'assembleur : un programme doit partager le même ensemble de registres, et une fonction peut vouloir utiliser `rdi` pour un usage différent de celui d'une autre.
Pour résoudre ce conflit, les fonctions appelantes sauvegardent souvent les registres importants dans leur cadre de pile local avant d'invoquer les fonctions appelées.

Ici, la solution est simple : utiliser un autre registre pour le `call`, par exemple en copiant l'adresse de la fonction callback dans `rax`, puis en exécutant `call rax`.
Cela libérera `rdi` pour l'argument !
