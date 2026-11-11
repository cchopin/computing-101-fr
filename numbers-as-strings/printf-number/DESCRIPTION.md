La sortie littérale, les échappements de saut de ligne et la syntaxe échappée vous donnent la boucle « parcourir et écrire ».
Ajoutons maintenant le premier marqueur : `%d`.

L'octet `%` signifie « ceci est un marqueur ».
L'octet `d` signifie « prendre la prochaine valeur d'argument, la traiter comme un nombre décimal signé, et afficher ce nombre à cet endroit ».
La prochaine valeur de la ligne de commande commence à `argv[2]`.

```
argv[1]:  "value=%d"
argv[2]:  "-42"
output:   "value=-42"
```

Quand votre parcours atteint `%d`, sautez les deux octets du marqueur, convertissez la chaîne suivante de `argv` avec `atoi`, reconvertissez le nombre obtenu en texte avec le `itoa` signé, et écrivez ces chiffres immédiatement avec `write`.
Continuez ensuite le parcours de la chaîne de format.
Pour ce niveau, la chaîne de format contient au plus un marqueur `%d`.
Continuez à gérer le texte ordinaire, `\n`, `\\` et `%%` comme précédemment.

Construisez et soumettez comme précédemment :

```console
hacker@dojo:~$ as -o prog.o prog.s
hacker@dojo:~$ ld -o prog prog.o
hacker@dojo:~$ ./prog 'value=%d' -42
value=-42
hacker@dojo:~$ /challenge/check prog
```

Remplacez les marqueurs décimaux au fur et à mesure du parcours.
