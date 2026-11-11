Vous savez maintenant envoyer des données vers la sortie standard avec `write`.
Mais comment un programme reçoit-il des données ?
Il les *lit* (read) depuis l'entrée standard !

Comme `write`, `read` est un appel système qui fait circuler des données entre les descripteurs de fichiers et la mémoire, et son numéro est `0`.
Dans le cas de `read`, il lit un certain nombre d'octets depuis le descripteur fourni et les range en mémoire.
La syntaxe pseudo-C est la même que pour `write` :

```c
read(0, some_address, 5);
```

Cet appel lit `5` octets depuis le descripteur `0` (entrée standard) et les place en mémoire à partir de `some_address`.
Ainsi, si l'on tape (ou si l'on envoie par un tube) `HELLO HACKERS` sur l'entrée standard, l'appel `read` ci-dessus produit la configuration mémoire suivante :

```text
     Address     │ Contents
+───────────────────────────+
│ some_address   │ 48       │
│ some_address+1 │ 45       │
│ some_address+2 │ 4c       │
│ some_address+3 │ 4c       │
│ some_address+4 │ 4f       │
+───────────────────────────+
```

Que sont ces nombres ??
Ce sont les représentations *hexadécimales* de lettres encodées en *ASCII*.
Si ces mots ne vous parlent pas, parcourez la première moitié environ du module [Dealing with Data](/fundamentals/data-dealings), puis revenez ici !

Dans ce défi, on combine `read` avec les capacités déjà acquises avec `write`.
Le flag est transmis sur l'entrée standard du programme : 128 octets de flag.
Le programme doit :

1. d'abord `read` 128 octets depuis l'entrée standard, dans la mémoire du programme ;
2. ensuite `write` ces 128 octets, depuis cette zone mémoire, vers la sortie standard ;
3. enfin, quitter avec le code de retour `42`.

Mais quelle adresse utiliser ?
Il faut un emplacement valide et accessible en écriture, et l'on en connaît déjà un : la pile !
Le registre `rsp` pointe vers le sommet de la pile, où il reste beaucoup de place accessible en écriture.
On peut donc utiliser `rsp` comme adresse mémoire : `mov rsi, rsp`.

----
**DÉBOGAGE :**
Vous rencontrez un problème ?
Rappelez-vous le module Introspection !
Compilez votre programme et lancez-le avec `strace` pour voir ce qui se passe au niveau des appels système, ou lancez-le dans `gdb` pour examiner les valeurs des registres et de la mémoire.

**RAPPEL :**
Les étapes 2 et 3 sont déjà écrites, en grande partie (dans les défis précédents, on chargeait `rsi` depuis `[rsp+16]`, mais ici on le place directement à `rsp` avec `mov rsi, rsp`).
Il suffit d'ajouter l'étape 1 !
