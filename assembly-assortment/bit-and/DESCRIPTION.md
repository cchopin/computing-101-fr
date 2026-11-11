On a rencontré `and` avec le test pair ou impair. Ici, on va l'utiliser pour ne garder que les bits voulus.

Un `and` bit à bit compare deux valeurs bit par bit : chaque bit de sortie vaut `1` seulement si *les deux* bits d'entrée valent `1`.
C'est donc l'outil du *masquage* : on conserve les bits voulus et on force les autres à zéro.

Là où le masque vaut `1`, le bit d'origine passe ; là où il vaut `0`, le bit du résultat est effacé :

```
  1011 0110   (your value)
& 0000 1111   (the mask: keep the low 4 bits)
---------
  0000 0110   (everything above the low 4 bits is gone)
```

En x86, ce masquage s'effectue avec une seule instruction `and`, le masque étant le second opérande :

```
and rax, 0xF
```

Un usage courant consiste à isoler l'octet de poids faible d'une valeur, c'est-à-dire ses 8 bits de poids faible, en la masquant avec `0xFF`.

Écrivez une fonction qui reçoit dans `rdi` une valeur sur 64 bits et renvoie dans `rax` uniquement son octet de poids faible.
La nommer `LOBYTE`, en majuscules : elle désigne l'octet de poids faible (**LO**w **BYTE**), et ce nom sert souvent d'abréviation pour cette fonction dans les outils qu'on rencontrera plus tard.
L'exporter avec `.global LOBYTE`.

La construire en bibliothèque partagée, puis la transmettre au correcteur :

```console
hacker@dojo:~$ as -o lobyte.o lobyte.s
hacker@dojo:~$ ld -shared -o lobyte.so lobyte.o
hacker@dojo:~$ /challenge/check lobyte.so
```
