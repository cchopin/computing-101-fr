Votre `atoi` à deux chiffres calculait `first * 10 + second`.
Et s'il y a plus de deux chiffres ?
Il suffit de garder un total courant et, pour chaque nouveau chiffre, de calculer `total = total * 10 + digit`.
Cette répétition est une *boucle*, que vous avez pratiquée dans [Écrire des boucles](https://pwn.college/computing-101-fr/control-flow/loop-write) et que vous adapterez ici.

Lisez les chiffres de gauche à droite :

```
"123":
  total = 0
  '1':  total =  0*10 + 1  =   1
  '2':  total =  1*10 + 2  =  12
  '3':  total = 12*10 + 3  = 123
```

On répète cela jusqu'à la fin de la chaîne. Par convention, celle du langage C (et utilisée ici), la fin est représentée par un octet de valeur 0x00 (c'est-à-dire `00000000` en binaire, ou la valeur 0 en décimal).
Attention : ce n'est pas le caractère '0', qui vaut `0x30` (`00110000` en binaire).

Votre boucle se résume donc ainsi : lire l'octet suivant ; s'il vaut `0`, sauter après la boucle (reportez-vous à [Écrire des boucles](https://pwn.college/computing-101-fr/control-flow/loop-write) pour la structure) ; sinon convertir le chiffre comme le faisait `atoi_digit`, multiplier le total par `10`, y ajouter le chiffre, puis revenir au début de la boucle.

Votre `atoi` reçoit dans `rdi` un pointeur vers la chaîne et doit renvoyer la valeur entière dans `rax`.
Parcourez les chiffres en boucle, et renvoyez le nombre.

----

**Débogage :**
Ce niveau peut être délicat à réussir.
Pour le déboguer, ajoutez à votre code un `_start` qui simule l'appel, comme ceci :

```
.global _start
_start:
    push 0x333231   # "123" on the stack -- little-endian, so 0x31 ('1') is the first byte, and the high zero bytes terminate it
    mov rdi, rsp    # a pointer to that string, as the first argument to atoi
    int3            # this is optional, if you want gdb to break here without having to set a breakpoint!
    call atoi       # there we go!

    mov rdi, rax    # atoi's result comes back in rax; exit with it so you can read it back with `echo $?`
    mov rax, 60     # exit
    syscall
```

Assemblez et liez-le comme un exécutable normal (sans `-shared`, car cette version a un point d'entrée), puis chargez-le dans `gdb` :

```console
hacker@dojo:~$ as -o debug.o debug.s
hacker@dojo:~$ ld -o debug debug.o
hacker@dojo:~$ gdb ./debug
(gdb) run
```

L'exécution s'arrête sur votre `int3`. Vous pouvez alors avancer pas à pas avec les techniques vues dans [Introspection logicielle](https://pwn.college/computing-101-fr/introspecting), en observant `rdi` parcourir la chaîne et le total se construire dans `rax`, jusqu'à ce que tout fonctionne !
