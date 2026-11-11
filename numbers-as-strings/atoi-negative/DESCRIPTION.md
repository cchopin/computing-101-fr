Le `atoi` précédent gère les nombres positifs.
Mais les nombres peuvent être négatifs, et un nombre négatif arrive avec un signe moins en tête : la chaîne `"-42"` est constituée des quatre octets `'-'`, `'4'`, `'2'`, NUL.

Étendre le convertisseur pour gérer ce signe.
Si le tout premier caractère est `'-'` (ASCII `0x2d`), retenir que le résultat doit être négatif, sauter le signe, et convertir les chiffres qui suivent exactement comme auparavant.
Puis, à la fin, changer le signe du total.
Un nombre positif n'a évidemment pas de caractère de signe : il doit donc toujours être converti comme dans le niveau précédent.

Il y a deux façons de changer le signe d'un nombre : `neg rax` transforme un registre en son opposé, et `imul rax, -1` fait de même.
Le choix de la méthode est libre !
