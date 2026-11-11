Dans le niveau précédent, on déréférençait `rax` pour lire une donnée dans `rdi`.
Le choix de `rax` était assez arbitraire.
On aurait pu utiliser n'importe quel autre pointeur, y compris `rdi` lui-même !
Rien n'empêche de déréférencer un registre pour remplacer son propre contenu par la valeur lue en mémoire.

Par exemple, voici ce que l'on obtient en faisant exactement cela avec `rax`.
Chaque ligne est annotée par un commentaire :

```assembly
mov [133700], 42
mov rax, 133700  # after this, rax will be 133700
mov rax, [rax]   # after this, rax will be 42
```

Dans cet extrait, `rax` passe du rôle de pointeur au rôle de registre qui contient la donnée lue en mémoire.
Le processeur rend tout cela possible !

Dans ce défi, on va explorer ce principe.
Au lieu d'initialiser `rax` comme précédemment, on a fait de `rdi` le pointeur vers la valeur secrète !
Il faut déréférencer `rdi` pour charger cette valeur dans `rdi`, puis appeler `exit` avec cette valeur comme code de retour.
Bon courage !
