Un seul chiffre était facile.
Un nombre à deux chiffres comme `42` doit être *découpé* en dizaines (`4`) et en unités (`2`). Ce découpage est une division : `42 / 10 = 4` (le quotient), et `42 % 10 = 2` (le reste).

x86 fournit *les deux* résultats avec une seule instruction `div`, mais `div` est une instruction délicate qu'il faut apprendre avec soin.
`div rcx` divise la valeur de **128 bits** formée par la concaténation de `rdx:rax` par `rcx`, et place le quotient dans `rax` et le reste dans `rdx`.
Il en découle trois conséquences :

- Elle divise `rdx:rax`, et pas seulement `rax` : il faut donc d'abord mettre `rdx` à zéro (`xor rdx, rdx`), sinon `div` traite les restes de registres comme la moitié haute du nombre (et peut faire planter le programme).
- Le diviseur vient d'un registre et non d'une valeur immédiate : charger donc le `10` dans un registre (par exemple `mov rcx, 10; div rcx`).
- Le dividende ne se contrôle pas : c'est *toujours* `rdx:rax`.

Après le `div`, `rax` contient les dizaines et `rdx` les unités.
Transformer chacune en caractère comme le faisait `itoa_digit` (en ajoutant `0x30`), puis stocker les deux.

Écrire `itoa(value, buf)`, que le défi appellera.
Cette fonction reçoit une valeur (`10` à `99`) dans `rdi`, et un pointeur vers le tampon de « sortie » dans `rsi`.
Découper le nombre de `rdi` avec `div`, convertir les deux chiffres comme ci-dessus, puis écrire leurs caractères dans ce tampon.
Renvoyer ensuite le nombre de caractères écrits (ici 2).
Penser à `.global itoa`.

**Écrire des caractères.**
La fonction `itoa_digit` du niveau précédent renvoyait le résultat dans `rax`, sans avoir à l'écrire dans un tampon.
Ici, il faut l'écrire.
Le caractère réel tient sur *un octet* (8 bits), alors que le registre qui le contient fait 64 bits (8 octets).
On ne veut que le dernier octet (celui de « poids faible »), auquel on accède directement par les *alias de registres partiels*, selon le registre :

| registre | octet de poids faible |
| -------- | ---------------------- |
| `rax`    | `al`                   |
| `rbx`    | `bl`                   |
| `rcx`    | `cl`                   |
| `rdx`    | `dl`                   |
| `rsi`    | `sil`                  |
| `rdi`    | `dil`                  |
| `rbp`    | `bpl`                  |
| `rsp`    | `spl`                  |
| `r8`     | `r8b`                  |
| `r9`     | `r9b`                  |
| `r10`    | `r10b`                 |
| `r11`    | `r11b`                 |
| `r12`    | `r12b`                 |
| `r13`    | `r13b`                 |
| `r14`    | `r14b`                 |
| `r15`    | `r15b`                 |

Ainsi, si le caractère est dans `rax` et que le tampon est pointé par `rsi`, il faut écrire `mov [rsi], al`.

C'est délicat, mais il faut le faire soigneusement, et le flag sera la récompense !

----

**Débogage :**
Ce niveau peut être délicat à réussir.
Pour le déboguer, ajouter un `_start` au code, comme ceci :

```asm
.global _start
_start:
    mov rdi, 42     # you'll pass 42 as the first argument to your function
    push 0          # this pushes eight 0 bytes to the stack, clearing what will be your output buffer
    mov rsi, rsp    # the output buffer as the second argument to itoa
    int3            # this is optional, if you want gdb to break here without having to set a breakpoint!
    call itoa       # there we go!

    mov rax, 60     # exit cleanly, like a cultured individual
    syscall
```

Assembler et lier le fichier comme un exécutable normal (sans `-shared`, car cette version a un point d'entrée), puis le charger dans `gdb` :

```console
hacker@dojo:~$ as -o debug.o debug.s
hacker@dojo:~$ ld -o debug debug.o
hacker@dojo:~$ gdb ./debug
(gdb) run
```

L'exécution s'arrête sur le `int3`. On peut alors avancer pas à pas avec les techniques vues dans [Introspection logicielle](https://pwn.college/computing-101-fr/introspecting), en observant la mémoire de la pile et les registres, jusqu'à ce que tout fonctionne !
On peut aussi déboguer le programme natif qui charge la `.so` :

```console
hacker@dojo:~$ gdb --args /challenge/harness your-solve.so 42
(gdb) run
```

Le premier argument après `/challenge/harness` est la bibliothèque, et le second le nombre de test transmis à `itoa`.
