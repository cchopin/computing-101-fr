Dans le niveau précédent, on a passé des arguments au programme avec `run` dans gdb.
Les programmes peuvent aussi lire leur entrée standard, et gdb permet de rediriger cette entrée lors de l'exécution du programme débogué.
La syntaxe est celle de la redirection du shell, mais elle se place après `run` dans gdb :

```text
(gdb) run < /path/to/input
```

Dans ce défi, `/challenge/debug-me` lit l'entrée nécessaire dans `/challenge/secret`.
Le lancer sous gdb en redirigeant `/challenge/secret` vers son entrée standard, lire le nombre secret qu'il affiche, puis soumettre ce nombre avec `/challenge/submit-number`.
