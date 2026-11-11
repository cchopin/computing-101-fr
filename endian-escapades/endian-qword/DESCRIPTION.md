Vous venez de voir que x86 stocke les valeurs sur plusieurs octets en *petit-boutiste*, c'est-à-dire l'octet de poids faible en premier.
Il est temps de s'en servir : `/challenge/reverse-me` cache un mot de passe de 8 caractères dans un seul quadruple mot, au cœur de son propre code.

Le programme charge votre saisie et compare les 8 octets d'un coup à une valeur codée en dur :

```
movabs rbx, 0x4847464544434241
mov    rax, [rdi]
cmp    rax, rbx
jne    fail
```

(`movabs` est nouveau, mais il s'agit simplement d'un `mov`. La constante immédiate d'un `mov` normal ne dépasse pas 32 bits : l'assembleur utilise donc cette forme plus large, « move absolute », lorsque la constante remplit les 64 bits. Lisez-la comme un `mov`.)

Cette constante immédiate est le mot de passe *tel que le processeur le lit en mémoire* : en petit-boutiste, ses octets apparaissent dans l'ordre inverse des caractères :

```
0x4847464544434241  ->  bytes 48 47 46 45 44 43 42 41  (high to low, as printed)
                    ->  low byte first: 41 42 43 44 45 46 47 48  ->  "ABCDEFGH"
```

Désassemblez le programme, lisez cette immédiate de `movabs`, remettez ses huit octets dans l'ordre pour obtenir le mot de passe, puis lancez le programme :

```console
hacker@dojo:~$ objdump -d -M intel /challenge/reverse-me
hacker@dojo:~$ /challenge/reverse-me YOUR_PASSWORD_HERE
```

---
**ATTENTION** :
`/challenge/reverse-me` est un binaire **SUID** : sous gdb, il abandonne ses privilèges, et le `open("/flag")` qu'il contient échoue silencieusement.
Utilisez `objdump` pour le lire, mais lancez-le **directement** pour obtenir le flag.
