Dans le niveau précédent, on a construit le nom « /flag » en écrivant chaque octet sur la pile.
C'est une technique utile, mais pénible à écrire et difficile à relire : essayez de repérer une faute de frappe dans une longue phrase écrite ainsi !
Ce défi montre une meilleure façon de faire.

Le code d'assembleur peut aussi contenir des octets qui ne sont pas destinés à être exécutés.
Par exemple, si on les place après le dernier appel système `exit`, le processeur s'arrête avant de les atteindre.
Ces octets restent dans la mémoire du programme, sans être interprétés comme des instructions.

Pour les chaînes, l'assembleur fournit une directive pratique pour écrire ces octets :

```asm
_start:
    ...
    mov rax, 60
    syscall           // exit!
path:
    .asciz "/flag"    // never executed, but still there!
```

La directive `.asciz` produit les octets de la chaîne, suivis de l'octet nul que Linux attend à la fin d'un nom de fichier.
L'étiquette `path:` marque l'endroit où commencent ces octets.
Dans les défis suivants, lorsqu'un programme compilé charge un pointeur vers une chaîne stockée, on voit la même idée vue de l'autre côté : les octets sont stockés dans le programme, et une instruction calcule leur adresse à l'exécution.

Reste un problème : pour transmettre ce chemin à l'appel système `open`, il faut placer son adresse dans `rdi`.
Autrefois, les programmes étaient toujours chargés à la même adresse en mémoire, et l'on pouvait donc coder cette adresse en dur :

```asm
_start:
    ...
    mov rdi, path    // this would tell the assembler to store the address of `path` in rdi
    ...
path:
    .asciz "/flag"
```

Malheureusement, **CELA NE FONCTIONNE PAS** dans un contexte de cybersécurité !
Les logiciels modernes sont compilés pour pouvoir être chargés à n'importe quelle adresse de la mémoire, pour des raisons de sécurité que nous verrons dans la ceinture jaune.
Cela signifie qu'au moment de l'assemblage, *l'assembleur ne connaît pas la bonne adresse*.
Pour les applications classiques, ce problème peut être résolu au démarrage, mais les processeurs modernes le résolvent autrement : avec l'*adressage relatif à l'instruction courante* (Instruction Pointer Relative Addressing).

Sur x86 64 bits, le *pointeur d'instruction* (`rip`) est un registre qui contient toujours l'adresse de la *prochaine* instruction que le processeur va exécuter.
Ce n'est cependant pas un registre ordinaire, au sens où son usage est plus limité que celui de `rdi` (c'est une particularité de x86 ; beaucoup d'autres architectures permettent d'accéder directement à leur pointeur d'instruction).
Cela dit, x86 64 bits *permet* d'utiliser des adresses relatives à `rip` pour lire et écrire en mémoire.
Par exemple :

```asm
_start:
    ...
    mov rdi, [rip+path]
    ...
path:
    .asciz "/flag"
```

**CE N'EST TOUJOURS PAS CE QUE L'ON VEUT !**
Pourquoi ? Parce que cela *lit* les 8 octets situés à `[rip+path]` pour les placer dans `rdi`, au lieu de placer *l'adresse* de ces octets dans `rdi`.
`rdi` contiendrait alors les valeurs `'f'`, `'l'`, et ainsi de suite, alors que l'appel `open` a besoin de l'adresse et non des valeurs.

Heureusement, il existe une instruction qui *presque* lit, mais qui place dans `rdi` (ou dans un autre registre) l'adresse qui aurait été lue.
Cette instruction s'appelle **l**oad **e**ffective **a**ddress, soit « chargement de l'adresse effective » (le mot *effective* désigne les calculs que le processeur effectue pour obtenir l'adresse, comme l'ajout d'un décalage à `rip` dans ce cas) :

```asm
_start:
    ...
    lea rdi, [rip+path]
    ...
path:
    .asciz "/flag"
```

Cela place dans `rdi` l'adresse de la chaîne « /flag », et non son contenu.
On peut voir `path` comme l'adresse à laquelle se trouve le premier octet de `"/flag\0"` : `mov` copie les octets situés à cet endroit, alors que `lea` copie l'adresse, afin que le noyau puisse parcourir les octets jusqu'à l'octet nul.

Une remarque sur le calcul : bien que l'on écrive `[rip+path]`, ce qui est *réellement* ajouté à `rip` est l'écart entre les adresses de `rip` (qui pointe ici vers l'instruction suivant `lea`) et de la chaîne « /flag ».
La syntaxe est un peu étrange, c'est une autre particularité de x86.

Cette technique sert dans ce défi à indiquer le chemin passé à `open`.
Le programme doit ouvrir le nom de fichier stocké, `read` depuis le fd renvoyé vers la mémoire, `write` exactement le nombre d'octets renvoyé par `read` (`mov rdx, rax`, comme dans le défi read-exact), et `exit` avec le code `42`.
Les éléments nouveaux sont :

1. stocker le nom de fichier avec `.asciz`, après le code ;
2. charger l'adresse du nom de fichier pour `open` avec `lea rdi, [rip + path]`.

Lancer `/challenge/check` avec le programme, lire le flag, et l'écrire sur la sortie standard !
