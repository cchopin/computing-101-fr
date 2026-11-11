Dans le niveau précédent, vous avez vu deux fonctions se disputer `rdi`.
C'est un aperçu d'un problème plus vaste : il n'existe que seize registres à usage général, et toutes les fonctions du programme les partagent.
Quand vous faites un `call`, la fonction appelée va utiliser des registres pour son propre travail. Que devient alors la valeur que *vous* aviez dans ces registres ?

La réponse dépend de la [convention d'appel](https://fr.wikipedia.org/wiki/Convention_d%27appel) de votre architecture (ici, x86 64 bits), qui définit comment les fonctions passent leurs arguments et se partagent les registres.
En général, une convention d'appel répartit les registres en deux groupes, pour que des fonctions écrites séparément puissent coopérer :

- les registres **sauvegardés par l'appelant** (caller-saved) peuvent être librement écrasés par n'importe quelle fonction que vous appelez. Si vous avez besoin d'une valeur contenue dans l'un d'eux *après* un appel, c'est *votre* travail (celui de l'appelant) de la sauvegarder avant, puis de la restaurer. En général, on fait cela avec `push` avant l'appel et `pop` après. Sur x86-64, ces registres sont `rax` (que les fonctions appelées écrasent pour y placer leur valeur de retour), `rcx`, `rdx`, `rsi`, `rdi`, `r8`, `r9`, `r10` et `r11`.
- les registres **préservés par l'appelé** (callee-saved) doivent être laissés intacts par les fonctions que vous appelez. Les fonctions appelées *peuvent* les utiliser, mais elles doivent les restaurer dans leur état d'origine. Sur x86-64, ce sont `rbx`, `rbp`, `r12`, `r13`, `r14` et `r15`.

Notez que cette règle n'est qu'une *convention*.
Du code peut la violer, mais cela n'arrive pas vraiment avec du code raisonnable.

Ce niveau vous oblige à explorer les registres sauvegardés par l'appelant.
Votre fonction `solve` reçoit deux arguments :

1. `rdi` : un pointeur vers une fonction `clobber_function` qui écrase tous les registres sauvegardés par l'appelant ;
2. `rsi` : un pointeur vers une fonction `flag_function` qui vous donnera le flag si vous l'appelez avec vos registres non écrasés.

Vous *devez* appeler `clobber_function` avant `flag_function`.
Mais vous *devez* préserver vos registres sauvegardés par l'appelant avant d'appeler `clobber_function`, puis les restaurer ensuite.

Construisez votre bibliothèque partagée et soumettez-la au correcteur :

```console
hacker@dojo:~$ as -o your-solve.o your-solve.s
hacker@dojo:~$ ld -shared -o your-solve.so your-solve.o
hacker@dojo:~$ /challenge/check your-solve.so
```

Si c'est juste, le flag est à vous !
