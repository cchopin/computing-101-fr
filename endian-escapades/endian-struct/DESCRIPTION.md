Les vrais programmes lisent rarement un tampon à une taille uniforme.
Ils lisent des *structures* (struct) : quelques champs de tailles *différentes*, placés les uns après les autres en mémoire.

Ce `/challenge/reverse-me` traite votre mot de passe comme une structure.
Vous ne connaissez peut-être pas le langage C, mais si vous le connaissiez, la structure serait définie ainsi :

```c
struct { uint64_t a; uint32_t b; uint16_t c; uint8_t d; uint8_t e; };
```

Le désassemblage charge chaque champ à sa propre largeur et à son propre décalage :

```
movabs rbx, 0x................   ; a: 8-byte field at +0
mov    rax, [rdi+0]
cmp    rax, rbx
mov    eax, [rdi+8]               ; b: 4-byte field at +8
cmp    eax, 0x........
mov    ax,  [rdi+12]              ; c: 2-byte field at +12
cmp    ax,  0x....
mov    al,  [rdi+14]              ; d: 1-byte field at +14
cmp    al,  0x..
mov    al,  [rdi+15]              ; e: 1-byte field at +15
cmp    al,  0x..
```

Ce défi reprend à lui seul l'ensemble du module.
Pour chaque champ, lisez trois informations dans l'accès : sa *largeur* (d'après `rax`, `eax`, `ax` ou `al`), son *décalage* (`[rdi+X]`) et sa *valeur* (remettez l'immédiate dans l'ordre des octets selon la largeur du champ).
Recomposez les champs dans l'ordre des décalages, et vous obtenez le mot de passe.

```console
hacker@dojo:~$ objdump -d -M intel /challenge/reverse-me
hacker@dojo:~$ /challenge/reverse-me YOUR_PASSWORD_HERE
```

---
**ATTENTION** :
`/challenge/reverse-me` est un binaire **SUID** : sous gdb, il abandonne ses privilèges, et le `open("/flag")` qu'il contient échoue silencieusement.
Utilisez `objdump` pour le lire, mais lancez-le **directement** pour obtenir le flag.
