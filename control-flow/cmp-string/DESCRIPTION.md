Dans le défi précédent, vous avez utilisé `cmp` et `jne` pour vérifier un seul caractère et brancher vers un chemin d'échec.
Mais vérifier un seul caractère suffit rarement : les mots de passe, les commandes et les noms de fichiers sont des *chaînes* de plusieurs caractères.

La bonne nouvelle : vous savez déjà tout ce qu'il faut pour vérifier une chaîne entière.
Il suffit d'enchaîner plusieurs paires `cmp` / `jne`, une par caractère, toutes vers la même étiquette `fail` :

```asm
mov rax, [rsp+16]       ; charger le pointeur de argv[1]

cmp BYTE PTR [rax], 'Y'
jne fail

cmp BYTE PTR [rax+1], 'E'
jne fail

cmp BYTE PTR [rax+2], 'S'
jne fail
```

Chaque comparaison vérifie un caractère de la chaîne.
Rappelez-vous, du module [Mémoire de l'ordinateur](/computing-101/computer-memory), que `[rax+1]` désigne l'octet situé *juste après* l'adresse contenue dans `rax`, `[rax+2]` est deux octets plus loin, et ainsi de suite.
Comme les chaînes sont stockées sous forme d'octets consécutifs en mémoire, `[rax]` est le premier caractère, `[rax+1]` le deuxième, `[rax+2]` le troisième, etc.

Si un caractère ne correspond pas, `jne` saute immédiatement à `fail` : le programme ne vérifie pas le reste.
Seulement si *toutes* les comparaisons réussissent (tous les caractères correspondent) l'exécution tombe sur le chemin de succès.

C'est ainsi que fonctionnent de nombreuses comparaisons de chaînes au plus bas niveau : comparer octet par octet, et abandonner à la première différence.

Pratiquons maintenant.
Écrivez un programme qui vérifie si le premier argument commence par la chaîne `"pwn"` :

1. charger le pointeur du premier argument, depuis `[rsp+16]` ;
2. comparer l'octet situé au décalage 0 à `'p'`, avec `jne fail` si ce n'est pas le cas ;
3. comparer l'octet situé au décalage 1 à `'w'`, avec `jne fail` si ce n'est pas le cas ;
4. comparer l'octet situé au décalage 2 à `'n'`, avec `jne fail` si ce n'est pas le cas ;
5. écrire le chemin de succès : `exit(0)` ;
6. écrire l'étiquette `fail:` avec `exit(1)`.
