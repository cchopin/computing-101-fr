Découvrons un registre de plus : `rsi` !
Comme `rdi`, `rsi` sert à stocker temporairement une donnée.
Par exemple :

```assembly
mov rsi, 42
```

On peut aussi copier des données d'un registre à un autre !
Regardez :

```assembly
mov rsi, 42
mov rdi, rsi
```

La première ligne place `42` dans `rsi`, et la seconde copie la valeur de `rsi` dans `rdi`.
Une précision s'impose : le mot *move* (déplacer) est trompeur. En réalité, l'instruction copie la valeur, elle ne la déplace pas. Après le code ci-dessus, `rsi` *et* `rdi` valent `42`.
On aurait pu appeler cette instruction `set` (affecter) plutôt que `mov`. Pourquoi a-t-on choisi `mov` ? Même des personnes très compétentes se livrent à des [spéculations](https://retrocomputing.stackexchange.com/questions/12968/why-is-the-processor-instruction-called-move-not-copy) à ce sujet. Le nom est resté, et il faut faire avec.

Passons au défi !
Dans ce défi, une valeur secrète est placée dans le registre `rsi`, et le programme doit se terminer en renvoyant cette valeur comme code de retour.
Comme `exit` utilise la valeur de `rdi` comme code de retour, il faut copier la valeur secrète de `rsi` dans `rdi`.
Lancer `/challenge/check` avec le code écrit pour obtenir le flag ! `/challenge/check` place la valeur secrète dans `rsi` avant d'exécuter le code écrit.
Bon courage !
