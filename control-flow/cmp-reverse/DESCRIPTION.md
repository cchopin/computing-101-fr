Dans le défi précédent, du code comparait une chaîne caractère par caractère.
Cette fois, les rôles sont inversés : *nous* avons écrit un programme, et il faut découvrir ce qu'il fait !

Dans `/challenge/reverse-me`, il y a un binaire SUID.
Il prend un argument sur la ligne de commande, le compare à un mot de passe caché, un octet à la fois (ça rappelle quelque chose ?), et si le mot de passe est correct, il lit et affiche le flag.
Si un caractère est faux, il quitte silencieusement.

Comment résoudre cela ?
Il faut lire le désassemblage du programme, analyser les instructions `cmp`, comprendre le mot de passe attendu, puis lancer le programme avec le bon argument.

Les outils nécessaires sont déjà connus !
Dans le module [Introspection logicielle](https://pwn.college/computing-101-fr/introspecting), se rappeler : `objdump -d -M intel /challenge/reverse-me` désassemble le binaire et affiche ses instructions assembleur.
On verra des instructions `cmp` semblables à celles écrites au défi précédent, mais au lieu de caractères entre guillemets simples `''`, les valeurs comparées seront écrites en hexadécimal.
Les valeurs immédiates de ces comparaisons *sont* les caractères du mot de passe, codés en valeurs ASCII hexadécimales.

Par exemple, si le désassemblage montre :

```
cmp    BYTE PTR [rax],0x70
```

`0x70` est le code ASCII de `'p'`.
On peut obtenir la liste complète des valeurs ASCII avec la commande `man ascii`.

Une fois les caractères du mot de passe retrouvés, lancer le programme directement :

```console
hacker@dojo:~$ /challenge/reverse-me VOTRE_MOT_DE_PASSE
```

----
**ATTENTION** :
`/challenge/reverse-me` est un binaire **SUID** : il s'exécute avec des privilèges élevés pour pouvoir lire `/flag`.
Cependant, **déboguer un programme retire ses privilèges SUID**. L'appel système `open("/flag")` à l'intérieur échouera donc **silencieusement** si on le lance sous gdb.
On peut utiliser gdb ou `objdump` pour *comprendre* le binaire et trouver le mot de passe, mais le lancer **directement** (hors de gdb) pour obtenir le flag.
