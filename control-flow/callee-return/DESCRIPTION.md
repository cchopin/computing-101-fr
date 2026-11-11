Dans le défi précédent, votre fonction `solve` se terminait par un appel système `exit`.
Cela fonctionnait, mais l'appelant ne reprenait jamais la main : une fois le programme quitté, tout le processus disparaissait.
Dans un vrai programme, ce n'est évidemment pas souhaitable.

Un appelé doit normalement *rendre la main* à celui qui l'a appelé, afin que l'appelant puisse poursuivre son propre travail.
C'est le rôle de l'instruction **`ret`** (return, retour).

Rappelez-vous l'instruction `call` du défi précédent : elle empilait une *adresse de retour* avant de sauter dans votre code.
`ret` est sa contrepartie : elle **dépile cette adresse de retour sauvegardée et y saute**.
`call` et `ret` forment ensemble la paire de base d'appel et de retour de fonction en x86 : l'une pour entrer dans une fonction, l'autre pour en sortir.

Dans ce défi, votre fonction `solve` doit **renvoyer une valeur** au défi.
Dans la convention d'appel Linux x86-64, **la valeur de retour d'une fonction est placée dans `rax`**.
Vous avez déjà vu `rax` dans les appels système : il contient le numéro de l'appel à l'entrée, et le résultat à la sortie.
Le même registre contient la valeur de retour d'une fonction ordinaire.

Le fonctionnement :

1. Le défi exécute `call solve` en passant un argument dans `rdi`. L'instruction qui suit ce `call` est alors empilée comme « adresse de retour sauvegardée ».
2. Votre fonction effectue son travail.
3. Votre fonction place son résultat dans `rax`.
4. Votre fonction exécute `ret`, qui dépile l'adresse de retour sauvegardée et saute vers le défi.
5. Le défi lit `rax` comme résultat de votre fonction (« valeur de retour »).

Pour ce défi :

- `rdi` contiendra une valeur secrète sur 64 bits, choisie au hasard par le défi ;
- votre fonction doit renvoyer cette même valeur dans `rax`.

Une fois que le défi reçoit la bonne valeur, il vous donnera le flag !
