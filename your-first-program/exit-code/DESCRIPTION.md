Comme on le sait peut-être, chaque programme se termine avec un *code de retour* (exit code).
Ce code est transmis en paramètre à l'appel système `exit`.

De la même façon que le numéro d'un appel système (par exemple `60` pour `exit`) est placé dans le registre `rax`, les paramètres d'un appel système sont eux aussi transmis par des registres.
Un appel système peut prendre plusieurs paramètres, mais `exit` n'en prend qu'un : le code de retour.
Le premier paramètre d'un appel système est transmis par le registre `rdi`. C'est lui qui fait l'objet de ce défi.

Dans ce défi, le programme doit se terminer avec le code de retour `42`.
Il faut donc trois instructions :

1. Placer le code de retour dans `rdi`.
2. Placer le numéro de l'appel `exit` dans `rax` (`mov rax, 60`).
3. Exécuter `syscall` !

Il ne reste plus qu'à écrire le programme.
