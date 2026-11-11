Le défi du niveau précédent héritait de l'environnement du shell interactif, aussi bien dans gdb qu'en dehors.
En réalité, les différences d'environnement sont souvent plus importantes entre la configuration locale et la cible analysée.
Le binaire déboguégé depuis le shell, et le même binaire exécuté comme service, tâche cron ou script distant, voient des environnements complètement différents (un autre `HOME`, un autre `PATH`, un *ensemble* de variables totalement différent).

Ce niveau explore ce principe un peu plus loin.
Ici, le wrapper `gdb` définit son propre environnement au lieu d'hériter de celui du shell, et le défi, lorsqu'il est lancé directement, oblige à faire de même : il exige un environnement ne contenant qu'une seule variable.
Par exemple :

```console
hacker@dojo:~$ /challenge/program
You're running me with 8 environment variables, but I need exactly 1! Clear the environment and set one variable, then rerun me!
hacker@dojo:~$ /challenge/program
```

Comment vider l'environnement ?
On peut le faire avec la commande `env`, que nous avons déjà utilisée pour afficher les variables d'environnement exportées dans le [Linux Luminarium](https://pwn.college/linux-luminarium/variables).
La commande `env` peut aussi servir de *wrapper* pour contrôler précisément l'environnement d'un programme.
Par exemple, `env -i` vide complètement l'environnement du programme enfant :

```console
hacker@dojo:~$ env -i /challenge/program
You're running me with 0 environment variables, but I need exactly 1! Clear the environment and set one variable, then rerun me!
hacker@dojo:~$ /challenge/program
```

On peut aussi définir des variables après avoir vidé l'environnement :

```console
hacker@dojo:~$ env -i PWN=COLLEGE HACK=PLANET /challenge/program
You're running me with 2 environment variables, but I need exactly 1! Clear the environment and set one variable, then rerun me!
hacker@dojo:~$ /challenge/program
```

Cela permet de contrôler très finement son environnement.
Dans ce défi, ce contrôle fin sert à aligner des adresses dans un cadre un peu plus réaliste. Garder cette possibilité en tête pour d'autres situations !
