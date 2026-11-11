Ce niveau porte sur les registres : il faut modifier leur contenu ou le lire.

Les valeurs sont placées dynamiquement en mémoire avant chaque exécution, donc elles changent d'une fois à l'autre. Il faut effectuer un calcul sur les registres. Le niveau indique quels registres sont préremplis et où placer le résultat. Dans la plupart des cas, c'est `rax`.

Ce niveau porte sur les opérations au niveau des bits. Il faut manipuler directement les bits d'un registre ou d'un emplacement mémoire. On aura probablement besoin des instructions logiques de x86 : `and`, `or`, `not`, `xor`.

La logique bit à bit en assembleur est un autre concept intéressant ! x86 permet d'effectuer des opérations logiques bit par bit sur les registres.

Pour cet exemple, supposons que les registres ne contiennent que 8 bits.

Les valeurs de `rax` et `rbx` sont :
- `rax = 10101010`
- `rbx = 00110011`

Si l'on effectue un ET bit à bit de `rax` et `rbx` avec l'instruction `and rax, rbx`, le résultat se calcule en combinant les bits deux à deux, un par un : d'où le nom de logique bit à bit.

Donc, de gauche à droite :
- 1 ET 0 = 0
- 0 ET 0 = 0
- 1 ET 1 = 1
- 0 ET 1 = 0
- ...

On obtient finalement :
- `rax = 00100010`

Voici quelques tables de vérité pour référence :

- **ET**
  ```
  A | B | X
  ---+---+---
  0 | 0 | 0
  0 | 1 | 0
  1 | 0 | 0
  1 | 1 | 1
  ```

- **OU**
  ```
  A | B | X
  ---+---+---
  0 | 0 | 0
  0 | 1 | 1
  1 | 0 | 1
  1 | 1 | 1
  ```

- **OU exclusif (XOR)**
  ```
  A | B | X
  ---+---+---
  0 | 0 | 0
  0 | 1 | 1
  1 | 0 | 1
  1 | 1 | 0
  ```

Sans utiliser les instructions suivantes : `mov`, `xchg`

Il faut faire ceci :

Placer dans `rax` la valeur de `(rdi AND rsi)` (ET logique)

----
**Remarque :**
`rax` aura tous ses bits à `1`.
Si ce n'était pas le cas, ce niveau serait plus délicat !
