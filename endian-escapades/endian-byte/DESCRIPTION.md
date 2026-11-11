Un octet à la fois, dans le registre 8 bits `al` :

```
mov al, [rdi+0]
cmp al, 0x41
jne fail
```

Et voici la récompense. Un octet seul n'a pas d'ordre à inverser : les immédiates *sont* les caractères, déjà dans l'ordre. `0x41` est simplement `'A'`. Seize comparaisons d'octets, sans aucune correction de boutisme.

C'est le cas extrême de la règle appliquée depuis le début : l'unité d'inversion est la taille de la lecture, donc une lecture d'un octet n'inverse rien.
Lire les immédiates directement de haut en bas dans le désassemblage, puis lancer le programme.

```console
hacker@dojo:~$ objdump -d -M intel /challenge/reverse-me
hacker@dojo:~$ /challenge/reverse-me YOUR_PASSWORD_HERE
```

---
**ATTENTION** :
`/challenge/reverse-me` est un binaire **SUID** : sous gdb, il abandonne ses privilèges, et le `open("/flag")` qu'il contient échoue silencieusement.
Utiliser `objdump` pour le lire, mais le lancer **directement** pour obtenir le flag.
