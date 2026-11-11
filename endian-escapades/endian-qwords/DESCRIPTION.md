On a vu qu'une valeur sur plusieurs octets se trouve en mémoire octet de poids faible en premier : la relire dans un registre inverse donc ses octets.
Un registre ne contient que huit octets, mais beaucoup de valeurs sont plus larges (et plus larges que ce que le processeur peut lire en une fois).

Selon le programme, ces valeurs peuvent être lues à la suite, registre par registre.
Par exemple, prenons un mot de passe de 16 octets, comme dans ce défi.
Les seize octets ASCII, lus comme deux quadruples mots de 8 octets, peuvent être accédés ainsi :

| Adresse     | Valeur |
|-------------|--------|
| `0x1337000` | `0x41` |
| `0x1337001` | `0x42` |
| `0x1337002` | `0x43` |
| `0x1337003` | `0x44` |
| `0x1337004` | `0x45` |
| `0x1337005` | `0x46` |
| `0x1337006` | `0x47` |
| `0x1337007` | `0x48` |
| `0x1337008` | `0x49` |
| `0x1337009` | `0x4a` |
| `0x133700a` | `0x4b` |
| `0x133700b` | `0x4c` |
| `0x133700c` | `0x4d` |
| `0x133700d` | `0x4e` |
| `0x133700e` | `0x4f` |
| `0x133700f` | `0x50` |

Stockée octet par octet, cette chaîne de caractères n'est *pas* affectée par le boutisme.
Ce que le boutisme change, ce sont les octets *une fois chargés dans un registre après le `mov`* : chacun, étant une valeur sur plusieurs octets, se relit en commençant par l'octet de poids faible.
Ainsi, si `rdi` pointe vers ce tampon, `mov rsi, [rdi]` donne la valeur `0x4847464544434241`, et `mov rsi, [rdi+8]` donne `0x504f4e4d4c4b4a49`.

Pour reconstituer une valeur plus longue, garder les quadruples mots dans l'ordre où on les obtient, mais inverser les octets à l'intérieur de chacun.

Désassembler `/challenge/reverse-me`, lire les deux valeurs sur quadruple mot auxquelles il compare la saisie, remettre chacune dans l'ordre des octets, le concaténer dans l'ordre, puis lancer le programme avec le résultat :

```console
hacker@dojo:~$ objdump -d -M intel /challenge/reverse-me
hacker@dojo:~$ /challenge/reverse-me YOUR_PASSWORD_HERE
```

---
**ATTENTION** :
`/challenge/reverse-me` est un binaire **SUID** : sous gdb, il abandonne ses privilèges, et le `open("/flag")` qu'il contient échoue silencieusement.
Utiliser `objdump` pour le lire, mais le lancer **directement** pour obtenir le flag.
