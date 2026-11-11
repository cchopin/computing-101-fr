Dans le défi précédent, on a retrouvé le programme en lisant les caractères du mot de passe directement dans les instructions `cmp`.
Cette fois, le programme *transforme* la saisie avant de la comparer.
Il faut comprendre cette opération et l'inverser mentalement pour passer la vérification !

Dans `/challenge/reverse-me`, il y a un nouveau binaire SUID.
Il effectue un calcul sur le premier octet du premier argument du programme, puis compare le résultat à une valeur codée en dur.
Si la comparaison réussit, il lit et affiche le flag.
Sinon, il quitte silencieusement.

La nouvelle instruction est `add`, utilisée ainsi :

```asm
add rax, 42
```

Elle ajoute 42 au registre `rax` et stocke le résultat dans `rax`.
Le code suivant donne à `rax` la valeur 99 :

```asm
mov rax, 57
add rax, 42
```

Comme beaucoup d'instructions, `add` peut travailler avec la mémoire, des registres ou des valeurs immédiates. Si l'on désassemble ce binaire avec `objdump -d -M intel /challenge/reverse-me`, on peut voir quelque chose comme :

```
add    BYTE PTR [rax],0x2a
cmp    BYTE PTR [rax],0x96
```

Cela ajoute `0x2a` (42) au premier octet de la saisie (en mémoire), puis vérifie que le résultat vaut `0x96` (150).
Pour trouver le caractère attendu, il suffit d'inverser le calcul : `150 - 42 = 108` (`6c`).
D'après `man ascii`, `0x6c` correspond au caractère `'l'`.
Le caractère attendu est donc `l`. Penser à `man ascii` pour passer des valeurs hexadécimales aux caractères !

Une fois le caractère trouvé, lancer le programme :

```console
hacker@dojo:~$ /challenge/reverse-me YOUR_CHARACTER_HERE
```

Il ne reste plus qu'à jouer.
Aller chercher le flag.

---
**ATTENTION** :
`/challenge/reverse-me` est un binaire **SUID** : il s'exécute avec des privilèges élevés pour pouvoir lire `/flag`.
Cependant, **déboguer un programme lui retire ses privilèges SUID**, ce qui signifie que le `open("/flag")` contenu dans le programme échouera **silencieusement** sous gdb.
On peut utiliser gdb ou `objdump` pour *comprendre* le binaire, mais le lancer **directement** (hors de gdb) pour obtenir le flag.
