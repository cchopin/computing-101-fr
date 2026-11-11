Appliquons maintenant ce qui a été appris pour vérifier un caractère précis d'un argument de la ligne de commande.

Se rappeler, dans le module [La pile](https://pwn.college/computing-101-fr/the-stack), que `[rsp+16]` contient un *pointeur* vers `argv[1]`, le premier argument de la ligne de commande.
Pour accéder au texte de l'argument, il faut d'abord charger ce pointeur dans un registre :

```asm
mov rax, [rsp+16]
```

Désormais, `rax` contient l'*adresse* de la chaîne de l'argument.
Le premier caractère de cette chaîne se trouve à `[rax]`, le deuxième à `[rax+1]`, et ainsi de suite.

Pour vérifier si le premier caractère est, par exemple, `'p'` :

```asm
cmp BYTE PTR [rax], 'p'
```

Cela lit un octet à l'adresse contenue dans `rax`, et le compare à la valeur ASCII de `'p'`.
À retenir : `BYTE PTR` indique au processeur qu'on travaille avec un seul octet, et non une valeur complète de 64 bits.
On l'a déjà vu dans le module [Entrées et sorties](https://pwn.college/computing-101-fr/hello-hackers), lorsque des chaînes ont été construites octet par octet sur la pile.

Après le `cmp`, le drapeau zéro indique si les caractères correspondent, et on peut récupérer ce résultat avec `setz dil`, comme précédemment.

Défi : écrire un programme qui vérifie si le premier caractère de `argv[1]` est `'p'`.
Quitter avec `1` si c'est le cas, `0` sinon.

Le programme doit utiliser 5 instructions :

1. charger le pointeur `argv[1]`, depuis `[rsp+16]`, dans un registre ;
2. comparer l'octet `BYTE PTR` situé à cette adresse à `'p'` ;
3. utiliser `setz dil` pour récupérer le résultat ;
4. placer le numéro de l'appel système `exit` ;
5. `syscall`.
