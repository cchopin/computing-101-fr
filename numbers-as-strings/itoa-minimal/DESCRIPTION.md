L'`itoa` à deux chiffres écrivait toujours *deux* caractères.
Mais `7` ne s'écrit pas `07`, et `0` ne s'écrit pas `00` : les nombres s'écrivent sans zéros en tête, avec autant de chiffres qu'ils en comptent réellement.
Dans ce niveau, nous supprimerons les zéros en tête de notre conversion, pour obtenir un bon `itoa`.

Nous traitons toujours des valeurs jusqu'à `99`, donc un seul `div` suffit.
Diviser par 10 comme précédemment : le quotient est le chiffre des dizaines, le reste celui des unités.
Si le quotient vaut `0`, il n'y a pas de chiffre des dizaines : on supprime le zéro en tête et l'on n'écrit que le reste.
Par exemple :

```
7:   7 / 10 = 0 rem 7   ->  quotient 0, so write just "7"
42:  42 / 10 = 4 rem 2  ->  quotient 4, so write "42"
```

Une valeur demande encore une attention particulière : `0` lui-même.
Son quotient vaut aussi `0`, mais n'écrire « rien » serait faux : on écrit donc un seul `'0'`.

Pour le reste, c'est la même chose :
Écrire `itoa(value, buf)` pour `value` compris entre `0` et `99` : écrire son texte décimal (sans zéros en tête) dans `buf`, et renvoyer dans `rax` le nombre de caractères écrits (`1` ou `2`).

----

**Remarque :**
Le test « le quotient vaut-il `0` ? » sera utile dans le niveau suivant, où nous gérerons enfin des nombres plus longs !
Le garder en tête !
