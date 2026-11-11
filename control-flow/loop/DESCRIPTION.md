Jusqu'ici, tous les schémas de flot de contrôle vus s'exécutaient en ligne droite : comparer, brancher, terminer.
Mais que faire si une même opération doit être répétée de nombreuses fois ?
C'est le rôle d'une **boucle** : une suite d'instructions qui saute *en arrière* pour se répéter.

Dans ce défi, `/challenge/reverse-me` compare `argv[1]` au mot de passe à l'aide d'une boucle :

```asm
loop:
  mov    al, BYTE PTR [rsi]       ; charger le caractère suivant du mot de passe
  cmp    al, BYTE PTR [rdi]       ; le comparer au caractère suivant de argv[1]
  jne    fail                     ; différence → saut vers fail
  cmp    al, 0x0                  ; atteint le caractère nul de fin ?
  je     success                  ; oui → tous les caractères correspondent !
  inc    rdi                      ; **inc**rémenter rdi pour passer au caractère suivant de argv[1]
  inc    rsi                      ; **inc**rémenter rsi pour passer au caractère suivant du mot de passe
  jmp    loop                     ; revenir en haut de la boucle : on recommence !
```

L'instruction clé est le `jmp loop` en bas.
Contrairement à `jne` (qui ne saute que si une condition est remplie), `jmp` saute **inconditionnellement**, toujours.
En revenant en arrière vers l'étiquette `loop`, le programme refait la même logique de comparaison sur la paire de caractères *suivante*.
La boucle se termine soit sur une différence (`jne fail`), soit lorsque le caractère nul de fin est atteint après que les autres caractères ont été comparés avec succès au mot de passe (`je success`).

C'est le mécanisme de base de toutes les boucles `for` et `while`, et de toutes les opérations sur les chaînes que l'on rencontre dans du code compilé.

Analyser le binaire, retrouver le mot de passe, et obtenir le flag !
