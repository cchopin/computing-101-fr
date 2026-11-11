Vous savez maintenant envoyer des données vers la sortie standard avec `write`.
Mais comment un programme reçoit-il des données en entrée ?
Il les *lit* (read) depuis l'entrée standard !

Comme `write`, `read` est un appel système qui fait circuler des données entre les descripteurs de fichiers et la mémoire. Son numéro est `0`.
Dans le cas de `read`, il lit un certain nombre d'octets depuis le descripteur fourni, et les range en mémoire.
La syntaxe pseudo-C est la même que pour `write` :

```c
read(0, 1337000, 5);
```

Cet appel lit `5` octets depuis le descripteur `0` (entrée standard) et les place en mémoire à partir de `1337000`.
Ainsi, si l'on tape (ou si l'on envoie par un tube) `HELLO HACKERS` sur l'entrée standard, l'appel `read` ci-dessus produit la configuration mémoire suivante :

```text
  Address │ Contents
+────────────────────+
│ 1337000 │ 48       │
│ 1337001 │ 45       │
│ 1337002 │ 4c       │
│ 1337003 │ 4c       │
│ 1337004 │ 4f       │
+────────────────────+
```

Que sont ces nombres ??
Ce sont les représentations *hexadécimales* de lettres encodées en *ASCII*.
Si ces mots ne vous parlent pas, parcourez la première moitié environ du module [Dealing with Data](/fundamentals/data-dealings), puis revenez ici !

Dans ce défi, on combine `read` avec ce que l'on sait déjà faire avec `write`.
Votre programme doit :

1. d'abord `read` 8 octets depuis l'entrée standard, à l'adresse `1337000` ;
2. ensuite `write` ces 8 octets, depuis l'adresse `1337000`, vers la sortie standard ;
3. enfin, quitter avec le code de retour `42`.

Rappel : vous avez déjà écrit les étapes 2 et 3. Il suffit d'ajouter l'étape 1 !

----
**NOTE :**
Dans ce défi, vous écrivez 8 caractères, alors que dans le défi précédent, vous en écriviez 14.
Pensez à mettre à jour la taille de `write()` (dans `rdx`) !

**DÉBOGAGE :**
Vous rencontrez un problème ?
Compilez votre programme et lancez-le avec `strace` pour voir ce qui se passe au niveau des appels système !
