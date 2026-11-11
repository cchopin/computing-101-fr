Plus petit encore : `/challenge/reverse-me` vérifie maintenant deux octets à la fois, sous forme de **mots** (words).

La règle ne change pas : les octets inversés sont exactement ceux de la lecture.
Une lecture de mot échange ses deux octets, et les mots restent dans l'ordre des adresses.
(Une lecture d'un seul octet n'a rien à échanger, c'est pourquoi les octets isolés n'ont jamais besoin de correction de boutisme.)

Désassemblez `/challenge/reverse-me`, lisez les huit immédiates `cmp ax, 0x....`, échangez les deux octets de chacune, gardez les mots dans l'ordre des adresses, puis lancez le programme :

```console
hacker@dojo:~$ objdump -d -M intel /challenge/reverse-me
hacker@dojo:~$ /challenge/reverse-me YOUR_PASSWORD_HERE
```

---
**ATTENTION** :
`/challenge/reverse-me` est un binaire **SUID** : sous gdb, il abandonne ses privilèges, et le `open("/flag")` qu'il contient échoue silencieusement.
Utilisez `objdump` pour le lire, mais lancez-le **directement** pour obtenir le flag.
