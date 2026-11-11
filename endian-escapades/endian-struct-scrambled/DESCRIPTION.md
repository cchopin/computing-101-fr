La structure précédente lisait ses champs de haut en bas, dans l'ordre de la mémoire : il suffisait donc de lire le désassemblage de haut en bas pour obtenir le mot de passe dans l'ordre.
Rien ne garantit cet ordre.
Un programme peut vérifier les champs d'une structure dans *n'importe quel* ordre : l'ordre des comparaisons dans le code n'a aucun rapport avec l'emplacement des octets en mémoire.

Ce `/challenge/reverse-me` vérifie les mêmes cinq champs, mais *dans le désordre* :

```
mov    ax,  [rdi+12]              ; the +12 word might be checked first...
cmp    ax,  0x....
mov    al,  [rdi+15]              ; ...then a byte from the very end...
cmp    al,  0x..
movabs rbx, 0x................    ; ...then the +0 qword, and so on.
mov    rax, [rdi+0]
cmp    rax, rbx
```

On ne peut donc plus lire le mot de passe en descendant simplement le désassemblage.
Retrouver la *valeur* de chaque champ comme précédemment, mais lire aussi son *décalage* dans le chargement `[rdi+X]` : c'est là que ses octets doivent être placés.
Mettre chaque champ à son décalage, les concaténer dans l'ordre des décalages, et on obtient le mot de passe.

```console
hacker@dojo:~$ objdump -d -M intel /challenge/reverse-me
hacker@dojo:~$ /challenge/reverse-me YOUR_PASSWORD_HERE
```

---
**ATTENTION** :
`/challenge/reverse-me` est un binaire **SUID** : sous gdb, il abandonne ses privilèges, et le `open("/flag")` qu'il contient échoue silencieusement.
Utiliser `objdump` pour le lire, mais le lancer **directement** pour obtenir le flag.
