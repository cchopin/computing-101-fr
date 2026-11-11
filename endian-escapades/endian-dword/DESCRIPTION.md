Le boutisme ne concerne pas seulement les valeurs sur 64 bits.
*Chaque* entier lu en mémoire revient en petit-boutiste, et les octets inversés sont exactement ceux couverts par cette lecture.
Prenons par exemple les octets suivants, placés les uns à la suite des autres en mémoire, à partir de l'adresse contenue dans `rdi` :

```
rdi -> 11 22 33 44 55 66 77 88
```

Si l'on lit les 8 octets dans `rsi` avec `mov rsi, [rdi]`, `rsi` vaut `0x8877665544332211`.
On peut aussi les lire comme deux valeurs sur 32 bits (4 octets), dans les registres partiels `esi` et `edx`, qui correspondent aux 32 bits de poids faible de `rsi` et `rdx` :

```
mov esi, [rdi]      // results in 0x44332211 in esi
mov edx, [rdi+4]    // results in 0x88776655 in edx
```

Une fois écrit, cela paraît évident, mais cela peut troubler. Ce qui *ne* se produit pas, c'est l'inversion de la valeur complète sur 8 octets : dans ce cas, `esi` aurait commencé par 0x88.

On s'exercera dans ce défi.
`/challenge/reverse-me` vérifie le même type de mot de passe quatre octets à la fois, sous forme de **valeurs sur 32 bits** (appelées « double mots », ou dwords). On obtient donc quatre entiers au lieu de deux.
Chaque double mot inverse toujours ses quatre octets ; les double mots restent dans l'ordre des adresses, comme les quadruples mots.
Comme un double mot tient dans la taille d'immédiate habituelle, chaque vérification est un simple `cmp eax, 0x........` : la valeur à retrouver figure directement dans l'instruction :

```
mov eax, [rdi+0]
cmp eax, 0x44434241
jne fail
```

Désassembler `/challenge/reverse-me`, lire les quatre immédiates des `cmp eax`, remettre chaque double mot dans l'ordre des octets, les concaténer dans l'ordre des adresses, puis lancer le programme avec le résultat :

```console
hacker@dojo:~$ objdump -d -M intel /challenge/reverse-me
hacker@dojo:~$ /challenge/reverse-me YOUR_PASSWORD_HERE
```

---
**ATTENTION** :
`/challenge/reverse-me` est un binaire **SUID** : sous gdb, il abandonne ses privilèges, et le `open("/flag")` qu'il contient échoue silencieusement.
Utiliser `objdump` pour le lire, mais le lancer **directement** pour obtenir le flag.
