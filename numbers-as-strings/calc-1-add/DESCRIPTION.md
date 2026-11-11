Vous savez lire un nombre depuis du texte avec votre `atoi`, et écrire un nombre sous forme de texte avec votre `itoa`.
Maintenant, faites-les travailler ensemble dans un seul programme : une calculatrice.

Une calculatrice lit une expression comme `6 + 7` et affiche le résultat.
Nous vous fournissons cette expression en trois morceaux, sur la ligne de commande :

```
prog 6 + 7
```

Ainsi, `argv[1]` est l'opérande de gauche (`"6"`), `argv[2]` l'opérateur (`"+"`), et `argv[3]` l'opérande de droite (`"7"`).
Chacun est une chaîne de caractères, comme les arguments que vous lisez déjà sur la pile.
Rappelez-vous que `argc` se trouve en `[rsp]`, et que les pointeurs vers les arguments suivent : `argv[0]` en `[rsp + 8]`, `argv[1]` en `[rsp + 16]`, `argv[2]` en `[rsp + 24]` et `argv[3]` en `[rsp + 32]`.

Les opérandes sont faciles : appliquez `atoi` à chacun pour obtenir sa valeur, exactement comme précédemment.
L'opérateur est la nouveauté.
C'est aussi une chaîne, mais d'un seul caractère : le caractère qui vous intéresse est donc son premier octet, `argv[2][0]`.
Chargez ce pointeur, lisez l'octet qu'il désigne, et vous obtenez l'opérateur sous forme d'un caractère unique, selon lequel vous pouvez vous brancher.

Pour ce niveau, il n'y a qu'un seul opérateur à gérer, `'+'` :

```
"6"   ->  atoi  ->   6
"7"   ->  atoi  ->   7
6  +  7  =  13
13   ->  itoa  ->  "13"
```

Mais un opérateur réel peut être n'importe quel caractère saisi par l'utilisateur, et vous ne savez qu'additionner.
Vérifiez donc d'abord l'octet de l'opérateur : s'il vaut `'+'`, effectuez l'addition ; sinon, vous ne le prenez pas en charge, et vous quittez avec un code de sortie non nul (il n'y a alors aucune réponse à afficher).
Ce refus fait partie du travail : reconnaître l'unique opérateur que vous gérez, et renoncer pour tous les autres.

Lorsque l'opérateur est `'+'` : utilisez `atoi` sur les deux opérandes, additionnez-les, transformez la somme en texte avec `itoa` dans un tampon de travail, puis écrivez ces octets sur le descripteur de fichier `1` (la sortie standard) avec `write`.
Réservez ce tampon sur la pile (un `sub rsp, 0x80` fait de la place, et `rsp` pointe alors dessus), comme vous aviez stocké la chaîne `/flag` sur la pile dans [Hello, Hackers](/computing-101/hello-hackers).
Quittez ensuite proprement avec le code `0`.

Il s'agit d'un programme complet : assemblez-le et liez-le comme un seul programme (sans `-shared`), puis soumettez-le :

```console
hacker@dojo:~$ as -o prog.o prog.s
hacker@dojo:~$ ld -o prog prog.o
hacker@dojo:~$ ./prog 6 + 7
13
hacker@dojo:~$ /challenge/check prog
```

Lisez les opérandes, branchez-vous selon l'opérateur, et affichez la somme.
