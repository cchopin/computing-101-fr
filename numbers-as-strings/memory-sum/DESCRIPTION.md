Le boss : tout assembler.

Lire les nombres depuis `argv`, convertir chacun avec `atoi`, additionner dans un total, transformer ce total en texte avec `itoa`, puis écrire le résultat sur la sortie standard avec `write`.
Sans argument numérique, afficher `0`.

- `argc` se trouve en `[rsp]` ; les entrées de la table `argv`, de 8 octets chacune, commencent en `[rsp + 8]`, et `[rsp + 16]` est l'entrée de `argv[1]`.
- Garder un registre qui pointe vers chaque entrée de la table, de `argv[1]` jusqu'à `argv[argc - 1]`.
- Déréférencer cette entrée de table pour obtenir le pointeur vers la chaîne que `atoi` attend.
- Avancer le registre de 8 octets après chaque nombre.

Rappel : `atoi` est un appel de fonction : tout état de boucle encore nécessaire après l'appel doit donc être préservé selon les [règles de convention d'appel pratiquées précédemment](https://pwn.college/computing-101-fr/control-flow/caller-saved-registers).

Les nombres, voire la somme totale, peuvent être négatifs : c'est justement pour cela que le `atoi` et l'`itoa` gèrent le signe.
Convertir ensuite le total avec `itoa` dans un tampon de travail (par exemple sur la pile), puis utiliser `write` pour écrire ce nombre d'octets sur le descripteur de fichier `1`.

Construire et soumettre comme précédemment :

```console
hacker@dojo:~$ as -o prog.o prog.s
hacker@dojo:~$ ld -o prog prog.o
hacker@dojo:~$ /challenge/check prog
```

Additionner, convertir le total, l'afficher, et c'est terminé !

----
**Débogage :**
Ne pas oublier gdb !
Insérer un `int3`, utiliser `breakpoint` dans gdb, faire `stepi` sur les instructions, et chercher à bien comprendre les échecs éventuels pour les corriger !
