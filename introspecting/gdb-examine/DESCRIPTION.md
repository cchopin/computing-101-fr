Dans le dernier niveau, vous pouviez utiliser `stepi` pour exécuter `pop rdi`, puis `print $rdi` pour lire le secret.
Cette fois, il n'y a plus de `pop` : le programme se termine immédiatement :

```text
mov    rdi,0x0
mov    rax,0x3c
syscall             <- exit(0) --- the secret was never read!
```

Le secret est toujours `argc`, et il se trouve juste au sommet de la pile, mais le programme ne le place jamais dans un registre.
Il faut examiner la mémoire directement !

La commande `x` (e**x**amine, « examiner ») de gdb permet de regarder le contenu de la mémoire.
Comme vous l'avez vu plus tôt, le pointeur de pile (`$rsp`) pointe au départ directement sur `argc`. On peut donc le lire avec :

```text
x $rsp
```

Faites-le !

1. Lancez le programme.
2. Examinez le sommet de la pile.
3. Quittez gdb et soumettez la valeur avec `/challenge/submit-number`.

----

**NOTE :**
`x` affiche les valeurs en _hexadécimal_ par défaut.
On peut changer le format d'affichage en ajoutant `/` à la commande.
Par exemple, pour voir la valeur en décimal, utilisez `x/d $rsp`.
Dans tous les cas, `/challenge/submit-number` accepte l'hexadécimal (par exemple `0x2a`) comme le décimal (par exemple `42`).
