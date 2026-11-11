Les pointeurs peuvent devenir encore plus intéressants !
Imaginez qu'un ami habite une autre maison de la même rue.
Au lieu de retenir son adresse, on pourrait l'écrire sur un papier, et ranger ce papier chez *soi*.
Pour obtenir des données de l'ami, il faudrait alors indiquer au processeur la maison de départ, le faire chercher l'adresse de l'ami à l'intérieur, puis utiliser cette adresse comme pointeur vers sa maison.

De la même façon, comme les adresses mémoire ne sont que des valeurs, elles peuvent être rangées en mémoire et récupérées plus tard !
Imaginons que l'on stocke la valeur `133700` à l'adresse `123400`, et la valeur `42` à l'adresse `133700`.
Considérons les instructions suivantes :

```assembly
mov rdi, 123400    # after this, rdi becomes 123400
mov rdi, [rdi]     # after this, rdi becomes the value stored at 123400 (which is 133700)
mov rax, [rdi]     # here we dereference rdi, reading 42 into rax!
```

Ouah !
Ce stockage d'adresses est *extrêmement* courant dans les programmes.
Adresses et données sont stockées, chargées, déplacées et, parfois, confondues !
Quand cela arrive, des problèmes de sécurité peuvent survenir, et on en rencontrera beaucoup au fil du parcours dans pwn.college.

Pour l'instant, pratiquons le déréférencement d'une adresse stockée en mémoire.
Une valeur secrète est rangée à une adresse secrète, et cette adresse est elle-même stockée à l'adresse `567800`.
Il faut lire cette adresse, la déréférencer pour obtenir la valeur secrète, puis appeler `exit` avec cette valeur comme code de retour.
On a tout ce qu'il faut !
