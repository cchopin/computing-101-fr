Un problème courant lié à la pile est le décalage des adresses qui se produit lorsqu'on lance un programme sous gdb.
Par défaut, gdb transmet son propre environnement (celui du shell, plus quelques ajouts propres à gdb) au programme débogué. Ces variables d'environnement supplémentaires décalent la pile vers la gauche : `argv[0]` se retrouve alors à une autre adresse que lorsqu'on lance le programme directement depuis le shell.
Ce point n'est pas essentiel pour l'instant, mais il devient gênant lorsqu'on cherche à comprendre pourquoi un exploit précis au bit près fonctionne sous gdb et non sur une cible qui tourne normalement.
Dans ces cas, il est important d'apprendre à « synchroniser » les deux environnements.

Ce défi enseigne les bases : rapprocher les adresses obtenues hors de gdb de celles obtenues dans gdb.

1. Lancer `/challenge/program` sous gdb (`gdb /challenge/program`, puis `run`).
   Le programme enregistre son propre `argv[0]` comme cible.
2. Quitter gdb. Lancer `/challenge/program` depuis le shell : il indiquera l'écart entre le `argv[0]` obtenu dans le shell et la cible.
3. Utiliser une variable d'environnement pour « compléter » l'environnement du shell jusqu'à ce que `argv[0]` atteigne la cible capturée dans gdb.
4. Flag !
