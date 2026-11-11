Vous avez transformé une saisie texte en nombre, mais les vrais programmes doivent aussi *afficher* des nombres sous forme de texte.
L'inverse de `atoi` s'appelle `itoa` (integer to ASCII). Nous allons le construire de la même façon : d'abord avec un seul chiffre, puis nous passerons à la suite !

Dans l'opération inverse de `atoi`, le caractère d'un chiffre est simplement sa valeur plus `'0'` (`0x30`).
Ainsi, si le caractère ASCII `'7'` est devenu la valeur `7` en retranchant `0x30`, alors la valeur `7` redevient le caractère `'7'` en ajoutant `0x30`.

```
7  ->  7 + 0x30  =  0x37  =  '7'
```

Nous commençons par `itoa_digit`.
Votre `itoa_digit` reçoit une valeur dans `rdi` (un seul chiffre, de `0` à `9`) et renvoie son caractère ASCII dans `rax`.
Pensez à `.global itoa_digit` pour que le défi puisse la trouver.

Le niveau précédent était un exécutable complet.
Celui-ci revient au fonctionnement en bibliothèque partagée, comme pour les fonctions `atoi` précédentes :

```console
hacker@dojo:~$ as -o your-solve.o your-solve.s
hacker@dojo:~$ ld -shared -o your-solve.so your-solve.o
hacker@dojo:~$ /challenge/check your-solve.so
```

Ajoutez `0x30`, renvoyez le caractère, et réclamez le flag.
