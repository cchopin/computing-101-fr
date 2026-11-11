Dans le défi précédent, le programme écrivait sa sortie, puis plantait.
Dans ce défi, on écrit la sortie, puis on ne plante pas !

On invoque pour cela l'appel système `write`, puis l'appel système `exit` pour quitter proprement le programme.
Comment invoquer deux appels système ?
Exactement comme on enchaîne deux instructions : on prépare les registres nécessaires et on invoque `write`, puis on prépare les registres nécessaires et on invoque `exit`.

La solution précédente comptait 5 instructions (charger l'adresse du premier argument depuis la pile, placer `rdi`, placer `rdx`, placer `rax`, et `syscall`).
Celle-ci doit en comporter ces 5, plus trois pour `exit` (placer le code de retour dans `rdi`, placer le numéro `60` dans `rax`, et `syscall`).
Pour ce défi, on quitte avec le code de retour `42` !
