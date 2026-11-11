Un `or` bit à bit compare lui aussi deux valeurs bit par bit, mais sa règle est l'inverse de celle de `and` : chaque bit de sortie vaut `1` si *l'un* des deux bits d'entrée vaut `1`.
C'est donc l'outil pour *activer* des bits : on met à `1` des bits précis, sans toucher aux autres.

Là où le masque vaut `1`, le bit du résultat est forcé à `1` ; là où il vaut `0`, le bit d'origine passe sans changement :

```
  0100 0001   ('A', 0x41)
| 0010 0000   (turn on 0x20)
---------
  0110 0001   ('a', 0x61)
```

Cet exemple est une astuce classique.
En ASCII, une majuscule et sa minuscule correspondante ne diffèrent que par le bit de casse `0x20` (le sixième bit en partant de la droite).
La minuscule s'obtient donc en activant ce bit sur la majuscule :

| Majuscule    | Minuscule    |
|--------------|--------------|
| `A` = `0x41` | `a` = `0x61` |
| `H` = `0x48` | `h` = `0x68` |
| `P` = `0x50` | `p` = `0x70` |
| `Z` = `0x5A` | `z` = `0x7A` |

`or` prend ses opérandes comme `and` : la valeur à modifier, puis le masque.
En activant le bit de casse avec `or`, toute majuscule devient sa forme minuscule.

Écrivez une fonction qui reçoit dans `rdi` une lettre ASCII majuscule et renvoie sa forme minuscule dans `rax`.
Appelez-la `chr_lower`. Nommez vos fonctions d'après ce qu'elles font : votre code restera lisible à mesure qu'il grandit.
Exportez-la avec `.global chr_lower`.

Construisez-la en bibliothèque partagée, puis transmettez-la au correcteur :

```console
hacker@dojo:~$ as -o chr_lower.o chr_lower.s
hacker@dojo:~$ ld -shared -o chr_lower.so chr_lower.o
hacker@dojo:~$ /challenge/check chr_lower.so
```
