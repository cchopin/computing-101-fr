Notre solution précédente écrivait du texte, puis plantait.
Dans ce défi, on écrit du texte, puis on ne plante *pas* !

Pour cela, on invoque l'appel système `write`, puis l'appel système `exit` pour quitter proprement le programme.
Comment invoquer deux appels système ?
Exactement comme on exécute deux instructions : on prépare les registres nécessaires et on invoque `write`, puis on prépare les registres nécessaires et on invoque `exit` !

La solution précédente comptait 5 instructions (placer une valeur dans `rdi`, dans `rsi`, dans `rdx`, dans `rax`, et `syscall`).
Celle-ci doit comporter ces 5 instructions, plus trois autres pour `exit` (placer le code de retour dans `rdi`, placer le numéro d'appel `60` dans `rax`, et `syscall`).
Pour ce défi, quittons avec le code de retour `42` !
