Dans ce niveau, votre code sera testé plusieurs fois avec des valeurs dynamiques ! Il sera exécuté dans des conditions variées, pour vérifier que la logique tient dans des usages ordinaires.

Dans ce niveau, on travaille avec des fonctions ! Il faudra manipuler le pointeur d'instruction (`rip`) et réaliser des tâches plus complexes que d'habitude. On peut vous demander d'utiliser la pile pour stocker des valeurs, ou d'appeler des fonctions fournies par le niveau.

Dans le niveau précédent, on a écrit sa première fonction et appris à appeler d'autres fonctions. Ici, on travaille avec des fonctions qui disposent d'un cadre de pile (*stack frame*).

Un cadre de pile est un ensemble de pointeurs et de valeurs empilés pour sauvegarder des choses et réserver de l'espace sur la pile pour les variables de la fonction.

Commençons par le registre spécial `rbp`, le pointeur de base de la pile (*Stack Base Pointer*).

Le registre `rbp` sert à repérer le début du cadre de pile. Prenons l'exemple d'une liste (un espace contigu en mémoire) qui n'est utilisée que dans notre fonction. Elle contient 5 éléments, chacun étant un `dword`. Une liste de 5 éléments occuperait déjà 5 registres : on préfère donc réserver de la place sur la pile !

L'assembleur ressemblerait à ceci :

```assembly
; setup the base of the stack as the current top
mov rbp, rsp
; move the stack 0x14 bytes (5 * 4) down
; acts as an allocation
sub rsp, 0x14
; assign list[2] = 1337
mov eax, 1337
mov [rbp-0xc], eax
; do more operations on the list ...
; restore the allocated space
mov rsp, rbp
ret
```

On remarque que `rbp` sert toujours à restaurer la pile à son état d'origine. Si l'on ne restaure pas la pile après usage, on finit par en manquer. On remarque aussi que l'on soustrait à `rsp`, car la pile croît vers le bas. Pour disposer de plus de place sur la pile, on soustrait la place nécessaire. Les instructions `ret` et `call` fonctionnent toujours de la même façon.

Pour affecter une valeur à `list[2]`, on soustrait 12 octets (3 dwords). En effet, la pile croît vers le bas, et une fois `rsp` déplacé, notre espace de pile contient les adresses situées entre `rsp` et `rbp`.

Une fois encore, il faut écrire une ou plusieurs fonctions qui implémentent la logique suivante :

```plaintext
most_common_byte(src_addr, size):
  i = 0
  while i <= size-1:
    curr_byte = [src_addr + i]
    [stack_base - curr_byte * 2] += 1
    i += 1

  b = 0
  max_freq = 0
  max_freq_byte = 0
  while b <= 0xff:
    if [stack_base - b * 2] > max_freq:
      max_freq = [stack_base - b * 2]
      max_freq_byte = b
    b += 1

  return max_freq_byte
```

**Hypothèses :**

- Un octet ne peut jamais apparaître plus de `0xffff` fois
- La taille ne dépasse jamais `0xffff`
- La liste contient au moins un élément

**Contraintes :**

- Il faut placer la « liste de comptage » sur la pile
- Il faut restaurer la pile comme dans une fonction ordinaire
- Il ne faut pas modifier les données situées à `src_addr`
