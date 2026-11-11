Dans le niveau précédent, vous avez appelé le callback avec `call rdi` : facile, puisque le pointeur était déjà dans `rdi`.
Cette fois, vous devez l'appeler **avec `1337` comme premier argument**.

La convention d'appel veut que le premier argument soit placé dans `rdi`, le même registre que celui qui contient le pointeur de fonction (puisque celui-ci a été passé comme premier argument à *votre* fonction `solve`).
Cela pose un problème :

- si vous placez `1337` dans `rdi`, vous écrasez le pointeur, et `call rdi` tentera d'appeler une fonction à l'adresse `1337`, ce qui fera planter le programme ;
- si vous faites `call rdi` avec `rdi` intact, vous passez le pointeur de fonction comme premier argument au lieu de `1337`, et vous n'obtiendrez pas le flag.

C'est une illustration d'une réalité fondamentale de l'assembleur : votre programme doit partager le même ensemble de registres, et une fonction peut vouloir utiliser `rdi` pour un usage différent de celui d'une autre.
Pour résoudre ce conflit, les fonctions appelantes sauvegardent souvent les registres importants dans leur cadre de pile local avant d'invoquer les fonctions appelées.

Ici, la solution est simple : utilisez un autre registre pour le `call`, par exemple en copiant l'adresse de la fonction callback dans `rax`, puis en exécutant `call rax`.
Cela libérera `rdi` pour votre argument !
