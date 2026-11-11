Le premier outil est assez simple : le **s**yscall **trace**r, c'est-à-dire `strace`, le traceur d'appels système.

On donne à `strace` un programme à exécuter. Grâce aux fonctions du système d'exploitation Linux, il observe le programme et enregistre chaque appel système qu'il effectue, avec son résultat.
Prenons par exemple le programme du défi précédent :

```console
hacker@dojo:~$ strace /tmp/your-program
execve("/tmp/your-program", ["/tmp/your-program"], 0x7ffd48ae28b0 /* 53 vars */) = 0
exit(42)                                 = ?
+++ exited with 42 +++
hacker@dojo:~$
```

`strace` indique quels appels système sont déclenchés, quels paramètres leur sont passés et quelles données ils renvoient.
La syntaxe de la sortie est `system_call(parameter, parameter, parameter, ...)`, où le premier mot désigne l'appel système.
Cette syntaxe est empruntée à un langage de programmation appelé C, mais il est inutile de s'en préoccuper pour l'instant.
Retenez simplement comment lire cette syntaxe précise.

Dans cet exemple, `strace` signale deux appels système : le second est l'appel `exit` par lequel le programme demande sa propre fin, et l'on voit le paramètre qui lui a été passé (42).
Le premier est un appel `execve`. Nous le verrons plus tard : c'est en quelque sorte le pendant de `exit`, puisqu'il démarre un nouveau programme (ici, `your-program`).
Ce programme ne l'invoque pas lui-même dans ce cas : sa détection par `strace` est un effet étrange de son fonctionnement, que nous étudierons plus tard.

Sur la dernière ligne, on voit le résultat de `exit(42)` : le programme se termine avec un code de retour de `42` !

L'appel `exit` est facile à observer sans `strace`, puisque son rôle est précisément de fournir un code de retour que l'on peut lire. D'autres appels système sont moins visibles.
Par exemple, l'appel système `alarm` (numéro 37 !) programme un minuteur dans le système d'exploitation : au bout du nombre de secondes indiqué, Linux termine le programme.
Cet appel sert, par exemple, à arrêter un programme qui s'est figé. Ici, on l'utilise pour s'entraîner à espionner des appels système avec `strace`.

Dans ce défi, il faut utiliser `strace` sur le programme `/challenge/trace-me` pour trouver la valeur passée en paramètre à l'appel système `alarm`, puis appeler `/challenge/submit-number` avec le nombre obtenu en argument.
Bon courage !
