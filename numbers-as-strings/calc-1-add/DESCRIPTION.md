On sait lire un nombre depuis du texte avec son `atoi`, et écrire un nombre sous forme de texte avec son `itoa`.
Maintenant, il faut les faire travailler ensemble dans un seul programme : une calculatrice.

Une calculatrice lit une expression comme `6 + 7` et affiche le résultat.
Cette expression est fournie en trois morceaux, sur la ligne de commande :

```
prog 6 + 7
```

Ainsi, `argv[1]` est l'opérande de gauche (`"6"`), `argv[2]` l'opérateur (`"+"`), et `argv[3]` l'opérande de droite (`"7"`).
Chacun est une chaîne de caractères, comme les arguments déjà lus sur la pile.
Rappel : `argc` se trouve en `[rsp]`, et les pointeurs vers les arguments suivent : `argv[0]` en `[rsp + 8]`, `argv[1]` en `[rsp + 16]`, `argv[2]` en `[rsp + 24]` et `argv[3]` en `[rsp + 32]`.

Les opérandes sont faciles : appliquer `atoi` à chacun pour obtenir sa valeur, exactement comme précédemment.
L'opérateur est la nouveauté.
C'est aussi une chaîne, mais d'un seul caractère : le caractère utile est donc son premier octet, `argv[2][0]`.
Charger ce pointeur, lire l'octet qu'il désigne : on obtient l'opérateur sous forme d'un caractère unique, selon lequel on peut se brancher.

Pour ce niveau, il n'y a qu'un seul opérateur à gérer, `'+'` :

```
"6"   ->  atoi  ->   6
"7"   ->  atoi  ->   7
6  +  7  =  13
13   ->  itoa  ->  "13"
```

Mais un opérateur réel peut être n'importe quel caractère saisi par l'utilisateur, et le programme ne sait qu'additionner.
Vérifier donc d'abord l'octet de l'opérateur : s'il vaut `'+'`, effectuer l'addition ; sinon, il n'est pas pris en charge, et le programme quitte avec un code de sortie non nul (il n'y a alors aucune réponse à afficher).
Ce refus fait partie du travail : reconnaître l'unique opérateur pris en charge, et renoncer pour tous les autres.

Lorsque l'opérateur est `'+'` : utiliser `atoi` sur les deux opérandes, additionner, transformer la somme en texte avec `itoa` dans un tampon de travail, puis écrire ces octets sur le descripteur de fichier `1` (la sortie standard) avec `write`.
Réserver ce tampon sur la pile (un `sub rsp, 0x80` fait de la place, et `rsp` pointe alors dessus), comme la chaîne `/flag` stockée sur la pile dans [Hello, Hackers](https://pwn.college/computing-101-fr/hello-hackers).
Quitter ensuite proprement avec le code `0`.

Il s'agit d'un programme complet : assembler et lier le programme comme un seul programme (sans `-shared`), puis le soumettre :

```console
hacker@dojo:~$ as -o prog.o prog.s
hacker@dojo:~$ ld -o prog prog.o
hacker@dojo:~$ ./prog 6 + 7
13
hacker@dojo:~$ /challenge/check prog
```

Lire les opérandes, se brancher selon l'opérateur, et afficher la somme.
