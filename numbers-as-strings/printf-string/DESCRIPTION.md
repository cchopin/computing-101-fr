Ajoutons maintenant `%s`, le marqueur qui insère une chaîne.
C'est comme `%d`, sauf que la valeur suivante de `argv` est déjà du texte : vous n'avez donc besoin ni de `atoi`, ni de `itoa`.

Pour `%s`, prenez la prochaine chaîne de la ligne de commande, déterminez sa longueur, et écrivez ses octets avec `write`.
Pour `%d`, continuez la conversion de nombres vue aux niveaux précédents.
Chaque marqueur consomme la prochaine valeur de la ligne de commande, dans l'ordre.
La chaîne de format est la seule chaîne qui contient de la syntaxe de formateur.
Si l'argument de `%s` contient des octets comme `\` ou `%`, recopiez-les tels quels, au lieu de les traiter comme des échappements ou des marqueurs.

```
argv[1]:  "%s has %d flags"
argv[2]:  "hacker"
argv[3]:  "3"
output:   "hacker has 3 flags"
```

```
argv[1]:  "%s"
argv[2]:  "LITERAL\WITH\SLASHES"
output:   "LITERAL\WITH\SLASHES"
```

Votre programme doit donc désormais suivre deux positions : où l'on en est dans la chaîne de format, et quelle valeur de `argv` doit être consommée ensuite.

Construisez et soumettez comme précédemment :

```console
hacker@dojo:~$ as -o prog.o prog.s
hacker@dojo:~$ ld -o prog prog.o
hacker@dojo:~$ ./prog '%s has %d flags' hacker 3
hacker has 3 flags
hacker@dojo:~$ /challenge/check prog
```

Consommez les valeurs dans l'ordre et écrivez chaque morceau au moment voulu.
