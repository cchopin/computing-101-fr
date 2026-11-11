Plus petit encore : `/challenge/reverse-me` vérifie maintenant deux octets à la fois, sous forme de **mots** (words).

La règle ne change pas : les octets inversés sont exactement ceux de la lecture.
Une lecture de mot échange ses deux octets, et les mots restent dans l'ordre des adresses.
(Une lecture d'un seul octet n'a rien à échanger, c'est pourquoi les octets isolés n'ont jamais besoin de correction de boutisme.)

Désassembler `/challenge/reverse-me`, lire les huit immédiates `cmp ax, 0x....`, échanger les deux octets de chacune, garder les mots dans l'ordre des adresses, puis lancer le programme :

```console
hacker@dojo:~$ objdump -d -M intel /challenge/reverse-me
hacker@dojo:~$ /challenge/reverse-me YOUR_PASSWORD_HERE
```

---
**ATTENTION** :
`/challenge/reverse-me` est un binaire **SUID** : sous gdb, il abandonne ses privilèges, et le `open("/flag")` qu'il contient échoue silencieusement.
Utiliser `objdump` pour le lire, mais le lancer **directement** pour obtenir le flag.
