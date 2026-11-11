Dans le défi précédent, vous avez écrit du code qui comparait une chaîne caractère par caractère.
Cette fois, les rôles sont inversés : *nous* avons écrit un programme, et *vous* devez découvrir ce qu'il fait !

Dans `/challenge/reverse-me`, il y a un binaire SUID.
Il prend un argument sur la ligne de commande, le compare à un mot de passe caché, un octet à la fois (ça vous rappelle quelque chose ?), et si le mot de passe est correct, il lit et affiche le flag.
Si un caractère est faux, il quitte silencieusement.

Comment résoudre cela ?
Il faut lire le désassemblage du programme, analyser les instructions `cmp`, comprendre le mot de passe attendu, puis lancer le programme avec le bon argument.

Vous avez déjà les outils pour cela !
Dans le module [Introspection logicielle](/computing-101/introspecting), souvenez-vous : `objdump -d -M intel /challenge/reverse-me` désassemble le binaire et affiche ses instructions assembleur.
Vous verrez des instructions `cmp` semblables à celles que vous avez écrites au défi précédent, mais au lieu de caractères entre guillemets simples `''`, les valeurs comparées seront écrites en hexadécimal.
Les valeurs immédiates de ces comparaisons *sont* les caractères du mot de passe, codés en valeurs ASCII hexadécimales.

Par exemple, si le désassemblage montre :

```
cmp    BYTE PTR [rax],0x70
```

`0x70` est le code ASCII de `'p'`.
Vous pouvez obtenir la liste complète des valeurs ASCII avec la commande `man ascii`.

Une fois les caractères du mot de passe retrouvés, lancez le programme directement :

```console
hacker@dojo:~$ /challenge/reverse-me VOTRE_MOT_DE_PASSE
```

----
**ATTENTION** :
`/challenge/reverse-me` est un binaire **SUID** : il s'exécute avec des privilèges élevés pour pouvoir lire `/flag`.
Cependant, **déboguer un programme retire ses privilèges SUID**. L'appel système `open("/flag")` à l'intérieur échouera donc **silencieusement** si vous le lancez sous gdb.
Vous pouvez utiliser gdb ou `objdump` pour *comprendre* le binaire et trouver le mot de passe, mais lancez-le **directement** (hors de gdb) pour obtenir le flag.
