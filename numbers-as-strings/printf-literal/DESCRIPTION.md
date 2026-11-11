Vous savez déjà écrire des octets sur la sortie standard.
Maintenant, vous allez placer cela dans une boucle et commencer à construire un petit programme de type `printf`.

L'entrée est une **chaîne de format** dans `argv[1]`.
Pour ce premier niveau, il n'y a pas encore de marqueurs spéciaux.
Chaque octet de la chaîne de format est du texte ordinaire : votre tâche est donc d'écrire ces octets sur la sortie standard.

```
argv[1]:  "score: "
output:   "score: "
```

Vous pouvez écrire un octet à la fois en parcourant la chaîne, ou repérer une suite d'octets ordinaires et l'écrire d'un seul coup.
Dans les deux cas, arrêtez-vous au NUL de la chaîne de format, puis quittez proprement.

Construisez et soumettez-le comme un exécutable :

```console
hacker@dojo:~$ as -o prog.o prog.s
hacker@dojo:~$ ld -o prog prog.o
hacker@dojo:~$ ./prog 'score: '
score: hacker@dojo:~$ /challenge/check prog
```

Notez que, dans l'exemple ci-dessus, `prog` n'affiche pas d'octet nul de fin, et l'invite de commande commence sur la même ligne.
Ce n'est pas gênant : le prochain niveau apprendra à votre formateur à écrire un saut de ligne.

Lors des tests, sachez que la ligne de commande dispose aussi d'un utilitaire `printf` intégré.
Si vous nommez votre programme `printf`, lancez-le par son chemin (par exemple `./printf`) pour éviter l'utilitaire intégré.
