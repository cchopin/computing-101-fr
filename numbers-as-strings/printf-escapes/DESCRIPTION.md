Maintenant, la chaîne de format contient de la syntaxe.
La barre oblique inverse ouvre des séquences d'échappement comme `\n`, et le signe pour cent introduit des marqueurs de format comme `%d`.

Il en résulte un problème pratique : la sortie doit parfois contenir une vraie barre oblique inverse ou un vrai signe pour cent.
La convention habituelle des formateurs consiste à doubler l'octet de syntaxe.
Les deux octets d'entrée `\\` écrivent un octet de sortie barre oblique inverse, et les deux octets d'entrée `%%` écrivent un octet de sortie signe pour cent.

```
argv[1]:  "path\\file"
output:   "path\file"

argv[1]:  "progress: 100%%"
output:   "progress: 100%"
```

Quand le parcours rencontre `\\`, sauter ces deux octets et écrire un octet barre oblique inverse avec `write`.
Quand il rencontre `%%`, sauter ces deux octets et écrire un octet signe pour cent.
Continuer à gérer le texte ordinaire et les échappements `\n`.

Construire et soumettre comme précédemment :

```console
hacker@dojo:~$ as -o prog.o prog.s
hacker@dojo:~$ ld -o prog prog.o
hacker@dojo:~$ ./prog 'progress: 100%%'
progress: 100%
hacker@dojo:~$ /challenge/check prog
```

Transformer les octets de syntaxe doublés en un seul octet littéral.
