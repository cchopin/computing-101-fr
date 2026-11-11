Dans le niveau précédent, le nom de fichier était transmis en argument au programme.
Mais que faire si l'on doit ouvrir un fichier dont on connaît déjà le chemin ?
Il existe plusieurs façons de coder des chaînes en dur dans un programme. Pour le type de code que l'on écrit sur pwn.college, nous choisissons une méthode « de hacker », plus adaptée à l'exploitation de logiciels qu'au développement logiciel classique.
On inscrit la chaîne directement dans le programme, en l'écrivant sur la pile, octet par octet !

L'appel `open` a besoin d'un *pointeur* vers le nom de fichier : il faut donc que les octets `/ f l a g` soient stockés quelque part en mémoire.
On connaît déjà une adresse accessible en écriture : `rsp` (la pile).
On peut écrire chaque caractère un octet à la fois :

```asm
mov BYTE PTR [rsp], '/'
mov BYTE PTR [rsp+1], 'f'
mov BYTE PTR [rsp+2], 'l'
mov BYTE PTR [rsp+3], 'a'
mov BYTE PTR [rsp+4], 'g'
mov BYTE PTR [rsp+5], 0
```

Quelques remarques :

- **`BYTE PTR`** : quand on écrit à une adresse comme `[rsp]` avec une *valeur immédiate* (un nombre ou un caractère), le processeur ne sait pas combien d'octets l'on veut écrire : un ? deux ? huit ? `BYTE PTR` est une *directive de taille* qui indique à l'assembleur « exactement un octet ». Sans elle, l'assembleur ne sait pas quelle instruction produire et refuse de l'assembler.

- **Guillemets simples** : en assembleur, un caractère entre guillemets simples comme `'f'` représente la valeur ASCII de ce caractère, sur un octet. Ainsi `'f'` est une façon commode d'écrire `0x66`, et `'/'` vaut `0x2f`.

- **L'octet nul** : le dernier octet écrit est `0`, un *octet nul* (null). C'est ainsi que Linux sait où se termine une chaîne : il lit les octets à partir du pointeur fourni et s'arrête à l'octet `0`. Sans lui, `open` continuerait de lire après `"flag"`, dans le reste de la pile, et tenterait d'ouvrir un fichier au nom absurde !

Une fois ces octets écrits, `rsp` pointe vers la chaîne terminée par un zéro `"/flag"`, prête à être passée à `open`.

**À jouer !**
Cette fois, aucun argument n'est transmis au programme.
Il faut construire soi-même le nom de fichier.

Le programme doit :

1. écrire `"/flag\0"` sur la pile, octet par octet, avec `mov BYTE PTR [rsp+N], ...` ;
2. `open` le fichier (appel système `2`) : `rdi` = `rsp` (la chaîne qui vient d'être écrite), `rsi` = `0` ;
3. `read` depuis le fd renvoyé vers la mémoire (appel système `0`), avec un nombre d'octets nettement suffisant ;
4. `write` vers la sortie standard exactement le nombre d'octets renvoyé par `read` (appel système `1`) ;
5. `exit` avec le code `42` (appel système `60`).

----
**DÉBOGAGE :**
En cas de problème ?
Utiliser `strace` pour tracer les appels système.
Si `open` renvoie `-1`, le pointeur ou l'encodage de la chaîne est peut-être incorrect.
Essayez `x/s $rsp` dans `gdb` pour voir la chaîne réellement présente sur la pile.
