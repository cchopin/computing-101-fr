Dans le défi précédent, une boucle a été reconnue dans un programme écrit par quelqu'un d'autre.
Cette fois, il faut en écrire une soi-même.

Une boucle a besoin de trois éléments : l'endroit où se trouve le travail en cours, la quantité de travail déjà effectuée, et un test qui décide quand la boucle est terminée.
Pour une chaîne, la limite naturelle est le caractère nul de fin : l'octet `0` placé après le dernier caractère.
À chaque passage, la boucle examine l'octet courant, avance au suivant, met à jour le compteur et saute en arrière pour recommencer.

Écrire un programme qui calcule la longueur de `argv[1]` et quitte avec cette longueur comme code de retour.
Le programme doit examiner la chaîne octet par octet, et utiliser un saut en arrière pour répéter la boucle.

Par exemple, si l'argument est `pwn`, le programme doit quitter avec le code `3`.
Si l'argument est vide, il doit quitter avec le code `0`.

Soumettre le programme à `/challenge/check`, et obtenir le flag !
