Le boss : tout assembler.

Lisez les nombres depuis `argv`, convertissez chacun avec votre `atoi`, additionnez-les dans un total, transformez ce total en texte avec votre `itoa`, puis écrivez-le sur la sortie standard avec `write`.
Sans argument numérique, affichez `0`.

- `argc` se trouve en `[rsp]` ; les entrées de la table `argv`, de 8 octets chacune, commencent en `[rsp + 8]`, et `[rsp + 16]` est l'entrée de `argv[1]`.
- Gardez un registre qui pointe vers chaque entrée de la table, de `argv[1]` jusqu'à `argv[argc - 1]`.
- Déréférencez cette entrée de table pour obtenir le pointeur vers la chaîne que `atoi` attend.
- Avancez le registre de 8 octets après chaque nombre.

Rappelez-vous que `atoi` est un appel de fonction : tout état de boucle encore nécessaire après l'appel doit donc être préservé selon les [règles de convention d'appel que vous avez pratiquées précédemment](/computing-101/control-flow/caller-saved-registers).

Les nombres, voire la somme totale, peuvent être négatifs : c'est justement pour cela que votre `atoi` et votre `itoa` gèrent le signe.
Convertissez ensuite le total avec `itoa` dans un tampon de travail (par exemple sur la pile), puis utilisez `write` pour écrire ce nombre d'octets sur le descripteur de fichier `1`.

Construisez et soumettez comme précédemment :

```console
hacker@dojo:~$ as -o prog.o prog.s
hacker@dojo:~$ ld -o prog prog.o
hacker@dojo:~$ /challenge/check prog
```

Additionnez, convertissez le total, affichez-le, et c'est terminé !

----
**Débogage :**
N'oubliez pas gdb !
Insérez un `int3`, utilisez `breakpoint` dans gdb, faites `stepi` sur les instructions, et cherchez à bien comprendre les échecs éventuels pour les corriger !
