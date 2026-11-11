Dans les défis précédents, vous avez utilisé `setz` pour récupérer le résultat d'une comparaison sous forme de `0` ou `1` dans `dil`, puis vous l'avez passé directement comme code de retour.
C'était astucieux, mais limité.
Et si vous voulez que votre programme fasse des *actions* différentes selon que les valeurs sont égales ou non ?

C'est là qu'interviennent les **sauts conditionnels**.
Au lieu de stocker le résultat de la comparaison dans un registre, vous demandez au processeur de *sauter* vers un autre endroit de votre code selon le résultat du `cmp`.
Le saut conditionnel le plus utile ici est `jne` (**j**ump if **n**ot **e**qual, saut si différent) :

```asm
cmp BYTE PTR [rax], 'p'
jne fail
```

Après le `cmp`, si les valeurs ne sont *pas* égales, le processeur saute à l'emplacement étiqueté `fail`.
On dit alors qu'il « prend la branche ».
Si les valeurs *sont* égales, le processeur se contente de passer à l'instruction suivante.
On dit alors qu'il ne prend pas la branche et « tombe » (fall through) sur l'instruction suivante.

En coulisses, `jne` teste le drapeau zéro (ZF) que `cmp` a réglé : `jne` saute quand ZF = 0, c'est-à-dire quand la soustraction n'était pas nulle, donc quand les valeurs différaient.
Il existe aussi `je` (**j**ump if **e**qual, saut si égal), qui fait l'inverse : il saute quand les valeurs *sont* égales.

Mais qu'est-ce que `fail` ?
C'est une **étiquette** (label), un nom donné à un endroit du code.
Les étiquettes ne produisent aucune instruction machine : elles marquent simplement un point vers lequel les sauts peuvent pointer.
On définit une étiquette en écrivant son nom suivi de deux-points :

```asm
fail:
  mov rdi, 1
  mov rax, 60
  syscall
```

L'assembleur remplace l'étiquette par une adresse, donc `jne fail` devient quelque chose comme `jne <adresse>` dans le code machine réel.
Vous pouvez donner presque n'importe quel nom à une étiquette (`fail`, `error`, `done`, `loop`, etc.), mais ce nom doit décrire ce qui se passe à cet endroit.

Avec les sauts conditionnels, votre programme peut suivre deux *chemins* d'exécution :

```
main:
  [charger et comparer]
  jne fail          ← saute vers fail si NON égal

success:
  mov rdi, 0
  mov rax, 60
  syscall

fail:
  mov rdi, 1
  mov rax, 60
  syscall
```

Si la comparaison réussit (les valeurs sont égales), l'exécution tombe sur le chemin de succès et quitte avec `0`, le code de retour habituel qui signale le succès sous Linux.
Si la comparaison échoue (les valeurs ne sont pas égales), l'exécution saute à l'étiquette `fail` et quitte avec `1`, ce qui signale un échec du programme.

Cet exemple est simple, mais nous commençons par le simple !
Le défi : écrivez un programme qui vérifie si le premier caractère de `argv[1]` est `'p'`, en utilisant des sauts conditionnels au lieu de `setz` :

1. charger le pointeur `argv[1]`, depuis `[rsp+16]`, dans un registre ;
2. comparer l'octet `BYTE PTR` situé à cette adresse à `'p'` ;
3. `jne fail` : sauter au cas d'échec si les caractères ne correspondent pas ;
4. écrire le cas de succès qui tombe à la suite (`exit(0)`) ;
5. définir l'étiquette `fail:` et écrire le cas d'échec (`exit(1)`).

La difficulté : le cas de succès (saut *non* pris) se trouve entre votre `jne` et le cas d'échec vers lequel pointe `jne`.
Cela demande un peu d'habitude, mais on s'y fait !
