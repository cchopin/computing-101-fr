Un marqueur `%d` permet à la chaîne de format d'inclure un nombre changeant.
Les messages réels ont souvent besoin de plusieurs valeurs.

Prendre maintenant en charge plusieurs marqueurs `%d` dans la même chaîne de format.
Chaque `%d` consomme la prochaine valeur de la ligne de commande : le programme doit donc retenir quelle entrée de `argv` vient ensuite.

```
argv[1]:  "opened %d files and skipped %d"
argv[2]:  "7"
argv[3]:  "3"
output:   "opened 7 files and skipped 3"
```

Le premier `%d` utilise `argv[2]`, le deuxième `argv[3]`, et ainsi de suite.
Après l'affichage d'un nombre, reprendre le parcours de la chaîne de format après ce marqueur.

Construire et soumettre comme précédemment :

```console
hacker@dojo:~$ as -o prog.o prog.s
hacker@dojo:~$ ld -o prog prog.o
hacker@dojo:~$ ./prog 'opened %d files and skipped %d' 7 3
opened 7 files and skipped 3
hacker@dojo:~$ /challenge/check prog
```

Consommer les arguments décimaux dans l'ordre.
