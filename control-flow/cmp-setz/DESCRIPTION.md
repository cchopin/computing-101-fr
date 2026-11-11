Jusqu'ici, vos programmes étaient assez simples : déplacer des valeurs, lire de la mémoire, invoquer un appel système.
Mais les vrais programmes doivent prendre des *décisions* : « si cette condition est vraie, faire ceci, sinon faire cela ».
C'est la base du **flot de contrôle** (control flow), et elle commence par la capacité à *comparer* des valeurs.

En assembleur x86, les comparaisons se font avec l'instruction `cmp`.
`cmp` compare deux valeurs en soustrayant la seconde opérande de la première.
Point crucial : **`cmp` ne stocke pas le résultat de la soustraction** à un endroit visible.
À la place, il met à jour les **indicateurs** (flags) internes du processeur, selon ce à quoi ressemblait le résultat.

Par exemple :

```asm
cmp rdi, 42
```

Cela calcule en interne `rdi - 42`, mais `rdi` n'est *pas* modifié.
En revanche, le processeur règle un bit particulier appelé **drapeau zéro** (ZF, Zero Flag) : si le résultat de la soustraction est nul (c'est-à-dire si les deux valeurs sont égales), ZF passe à 1.
Si `rdi` contient `42`, alors `42 - 42 = 0`, et ZF vaut 1.
Si `rdi` contient autre chose, le résultat n'est pas nul, et ZF vaut 0.

Après `cmp`, le processeur sait donc si les valeurs étaient égales.
Comment utiliser cette information ?

On ne peut pas copier directement les indicateurs dans un registre avec `mov`.
À la place, x86 propose une famille d'instructions « définir selon une condition » qui écrivent `0` ou `1` dans une destination d'un octet, selon les indicateurs.

Celle que nous utilisons ici est `setz` (« set if zero », définir si zéro) :

```asm
setz dil
```

Cette instruction regarde le drapeau zéro et :
- si ZF = 1 (les valeurs **étaient** égales, c'est-à-dire que le résultat de la soustraction était nul), elle écrit `1` dans `dil` ;
- si ZF = 0 (les valeurs **n'étaient pas** égales), elle écrit `0` dans `dil`.

En résumé : `1` veut dire « ça correspond ! », et `0` veut dire « non ».
Il existe aussi l'instruction complémentaire `setnz` (« set if not zero »), qui fait l'inverse, mais nous n'en aurons pas besoin ici.

Mais qu'est-ce que `dil` ?
Jusqu'ici, vous avez travaillé avec des registres 64 bits comme `rdi`, `rax` ou `rsp`.
L'instruction `setz`, elle, n'écrit qu'un seul octet (8 bits).
Heureusement, on peut accéder à des parties plus petites des registres 64 bits.
Pour `rdi` :

- `rdi` fait 64 bits au total ;
- `dil` est uniquement l'octet de poids faible (8 bits les plus à droite) de `rdi` : le **l**ow byte de r**di**.

Quand vous écrivez `setz dil`, vous placez un `0` ou un `1` dans le seul octet de poids faible de `rdi`, sans changer les octets de poids fort.
`rdi` est la valeur passée à l'appel système `exit`, mais Linux ne retient que les 8 bits de poids faible de cette valeur comme statut de sortie du processus.
C'est pourquoi modifier seulement `dil` suffit ici : le statut visible vaut `1` (égalité !) ou `0` (pas égal !), quels que soient les octets de poids fort de `rdi`.

Une dernière remarque sur `cmp` : il peut comparer un registre avec une valeur immédiate (`cmp rdi, 42`), ou même une zone de mémoire avec une valeur immédiate (`cmp QWORD PTR [rsp], 42`).
Mais il **ne peut pas** comparer deux zones de mémoire entre elles : une seule des deux opérandes peut être un déréférencement de mémoire.

Voici maintenant votre défi. Rappelez-vous, dans le module [La pile](https://pwn.college/computing-101-fr/the-stack), que `[rsp]` contient `argc`, le nombre d'arguments passés à votre programme, y compris le nom du programme.
Écrivez un programme qui :

1. compare `argc` à `42` (en chargeant d'abord `argc` dans un registre, ou en comparant directement à la mémoire) ;
2. utilise `setz dil` pour définir le code de retour : `1` si argc vaut 42, `0` sinon ;
3. quitte.
