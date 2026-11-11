La sortie littérale permet de recopier des caractères visibles, mais un texte a aussi besoin d'un moyen de désigner des octets difficiles à saisir directement.
Le saut de ligne en est l'exemple classique : il fait passer le terminal à la ligne suivante, au lieu d'afficher un symbole visible.

L'ASCII a déjà été rencontré : il associe des valeurs d'octets aux caractères de texte et aux caractères de contrôle.
L'octet ASCII du saut de ligne est `0x0a`, soit `10` en décimal.

La convention courante pour écrire des caractères spéciaux dans une chaîne de format utilise le préfixe `\` : un saut de ligne s'écrit `\n`.
Dans ce niveau, les deux octets `\n` de la chaîne de format signifient « écrire un octet de sortie de valeur `0x0a` ».
La barre oblique inverse ouvre une **séquence d'échappement**, et l'octet suivant indique quel octet spécial écrire.

```
argv[1]:  "hello\nworld"
output:   "hello"
          "world"
```

Quand le parcours rencontre une barre oblique suivie de `n`, sauter ces deux octets, et écrire avec `write` un octet de valeur `0x0a`.
Continuer à gérer le texte ordinaire.

Lors des tests, il faut faire attention : certaines syntaxes du shell peuvent interpréter `\n` avant que le programme ne le reçoive.
Utiliser des guillemets simples autour de la chaîne de format pour que le programme reçoive les deux octets `\` et `n`, comme dans `./prog 'hello\nworld'`.

Construire et soumettre comme précédemment :

```console
hacker@dojo:~$ as -o prog.o prog.s
hacker@dojo:~$ ld -o prog prog.o
hacker@dojo:~$ ./prog 'hello\nworld'
hello
world
hacker@dojo:~$ /challenge/check prog
```

Transformer `\n` en octet de saut de ligne.
