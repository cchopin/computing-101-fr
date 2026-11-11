Dans ce niveau, on travaille avec le contrôle du flot d'exécution. Il s'agit d'utiliser des instructions qui contrôlent, directement ou indirectement, le registre spécial `rip`, le pointeur d'instruction. On utilisera des instructions comme `jmp`, `call`, `cmp` et leurs variantes pour obtenir le comportement demandé.

Dans ce niveau, votre code sera testé plusieurs fois avec des valeurs dynamiques ! Il sera exécuté dans des conditions variées, pour vérifier que la logique tient dans des usages ordinaires.

On découvre maintenant les sauts conditionnels, l'un des outils les plus utiles de x86. Dans les langages de haut niveau, une structure si-sinon permet par exemple de faire ceci :

```plaintext
if x is even:
    is_even = 1
else:
    is_even = 0
```

Cette logique peut s'implémenter avec des bits, ce que l'on a fait dans un niveau précédent. Dans ces structures, on contrôle le flot du programme à partir de valeurs dynamiques fournies au programme.

On peut implémenter la logique ci-dessus avec des `jmp`, comme ceci :

```assembly
; assume rdi = x, rax is output
; rdx = rdi mod 2
mov rax, rdi
mov rsi, 2
div rsi
; remainder is 0 if even
cmp rdx, 0
; jump to not_even code if it's not 0
jne not_even
; fall through to even code
mov rbx, 1
jmp done
; jump to this only when not_even
not_even:
mov rbx, 0
done:
mov rax, rbx
; more instructions here
```

Souvent, on veut plus qu'un simple si-sinon. Parfois, on veut deux tests si, suivis d'un sinon. Pour cela, il faut que le flot tombe jusqu'au prochain `if` lorsque le précédent échoue. Tous doivent sauter vers le même `done` après leur exécution, pour éviter le sinon.

Il existe beaucoup de types de sauts en x86, et il est utile de savoir les employer. Presque tous reposent sur le ZF, le *Zero Flag* (indicateur de zéro). Le ZF vaut 1 lorsqu'une comparaison `cmp` donne une égalité, et 0 sinon.

En s'appuyant sur ce qui précède, il faut implémenter ceci :

```plaintext
if [x] is 0x7f454c46:
    y = [x+4] + [x+8] + [x+12]
else if [x] is 0x00005A4D:
    y = [x+4] - [x+8] - [x+12]
else:
    y = [x+4] * [x+8] * [x+12]
```

Où :
- `x = rdi`, `y = rax`.

Supposez que chaque valeur déréférencée est un mot double signé. Les valeurs peuvent donc être négatives à chaque position mémoire.

Une solution valide doit utiliser au moins une fois :
- `jmp` (une variante quelconque), `cmp`
