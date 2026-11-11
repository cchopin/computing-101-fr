Dans ce niveau, votre code sera testé plusieurs fois avec des valeurs dynamiques ! Il sera exécuté dans des conditions variées, pour vérifier que la logique tient dans des usages ordinaires.

Dans ce niveau, on travaille avec des fonctions ! Il faudra manipuler le pointeur d'instruction (rip) et réaliser des tâches plus complexes que d'habitude. On peut vous demander d'utiliser la pile pour stocker des valeurs, ou d'appeler des fonctions fournies par le niveau.

Dans les niveaux précédents, on a implémenté une boucle while qui compte les octets non nuls consécutifs d'une zone contiguë de mémoire.

Dans ce niveau, on reçoit à nouveau une zone contiguë de mémoire. Il faut la parcourir, en effectuant une opération conditionnelle sur chaque octet, jusqu'à atteindre un octet nul. Le tout doit être contenu dans une fonction !

Une fonction est un segment de code appelable, qui ne détruit pas le flot de contrôle.

Les fonctions utilisent les instructions `call` et `ret`.

L'instruction `call` empile l'adresse mémoire de l'instruction suivante, puis saute à la valeur stockée dans son premier argument.

Prenons les instructions suivantes comme exemple :
```
0x1021 mov rax, 0x400000
0x1028 call rax
0x102a mov [rsi], rax
```

1. `call` empile `0x102a`, l'adresse de l'instruction suivante.
2. `call` saute à `0x400000`, la valeur stockée dans `rax`.

L'instruction `ret` fait l'inverse de `call`.

`ret` dépile la valeur du sommet de la pile et saute vers elle.

Prenons les instructions et la pile suivantes comme exemple :

```
                            Stack ADDR  VALUE
0x103f mov rax, rdx         RSP + 0x8   0xdeadbeef
0x1042 ret                  RSP + 0x0   0x0000102a
```

Ici, `ret` saute vers `0x102a`.

Il faut implémenter la logique suivante :
```plaintext
str_lower(src_addr):
  i = 0
  if src_addr != 0:
    while [src_addr] != 0x00:
      if [src_addr] <= 0x5a:
        [src_addr] = foo([src_addr])
        i += 1
      src_addr += 1
  return i
```

`foo` est fournie à l'adresse `0x403000`. `foo` prend une seule valeur en argument et renvoie une valeur.

Toutes les fonctions (`foo` et `str_lower`) doivent respecter la convention d'appel amd64 de Linux (aussi appelée System V AMD64 ABI) :
[System V AMD64 ABI](https://en.wikipedia.org/wiki/X86_calling_conventions#System_V_AMD64_ABI) (en anglais)

Ainsi, votre fonction `str_lower` doit lire `src_addr` dans `rdi` et placer la valeur de retour dans `rax`.

Il faut noter que `src_addr` est une adresse mémoire (où se trouve la chaîne), et que `[src_addr]` désigne l'octet situé à `src_addr`.

Par conséquent, la fonction `foo` prend un octet en premier argument et renvoie un octet.
