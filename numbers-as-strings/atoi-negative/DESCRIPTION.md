Votre `atoi` gère les nombres positifs.
Mais les nombres peuvent être négatifs, et un nombre négatif arrive avec un signe moins en tête : la chaîne `"-42"` est constituée des quatre octets `'-'`, `'4'`, `'2'`, NUL.

Étendez votre convertisseur pour gérer ce signe.
Si le tout premier caractère est `'-'` (ASCII `0x2d`), retenez que le résultat doit être négatif, sautez le signe, et convertissez les chiffres qui suivent exactement comme auparavant.
Puis, à la fin, changez le signe du total.
Un nombre positif n'a évidemment pas de caractère de signe : il doit donc toujours être converti comme dans le niveau précédent.

Il y a deux façons de changer le signe d'un nombre : `neg rax` transforme un registre en son opposé, et `imul rax, -1` fait de même.
Choisissez celle que vous préférez !
