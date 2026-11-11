Un problème courant lié à la pile est le décalage des adresses qui se produit lorsqu'on lance un programme sous gdb.
Par défaut, gdb transmet son propre environnement (celui de votre shell, plus quelques ajouts propres à gdb) au programme débogué. Ces variables d'environnement supplémentaires décalent la pile vers la gauche : `argv[0]` se retrouve alors à une autre adresse que lorsque vous lancez le programme directement depuis votre shell.
Ce point n'est pas essentiel pour l'instant, mais il devient gênant lorsqu'on cherche à comprendre pourquoi un exploit précis au bit près fonctionne sous gdb et non sur une cible qui tourne normalement.
Dans ces cas, il est important d'apprendre à « synchroniser » les deux environnements.

Ce défi vous enseigne les bases : rapprocher les adresses obtenues hors de gdb de celles obtenues dans gdb.

1. Lancez `/challenge/program` sous gdb (`gdb /challenge/program`, puis `run`).
   Le programme enregistre son propre `argv[0]` comme cible.
2. Quittez gdb. Lancez `/challenge/program` depuis votre shell : il vous indiquera l'écart entre le `argv[0]` obtenu dans votre shell et la cible.
3. Utilisez une variable d'environnement pour « compléter » l'environnement de votre shell jusqu'à ce que `argv[0]` atteigne la cible capturée dans gdb.
4. Flag !
