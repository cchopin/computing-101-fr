Votre premier programme a planté…
Ce n'est pas grave, cela arrive à tout le monde !
Dans ce défi, on apprend à faire en sorte que le programme se termine proprement, au lieu de planter.

Le démarrage d'un programme et son arrêt propre sont pris en charge par le *système d'exploitation* (operating system). Le système d'exploitation gère l'existence des programmes, leurs interactions entre eux, avec le matériel, avec l'environnement réseau, et ainsi de suite.

Les programmes « interagissent » avec le processeur au moyen d'instructions d'assembleur, comme l'instruction `mov` écrite précédemment.
De la même façon, ils interagissent avec le système d'exploitation, toujours par l'intermédiaire du processeur, en utilisant l'instruction `syscall`, ou *appel système* (System Call).

Un peu comme on passe un appel téléphonique pour commander un repas dans un restaurant, les programmes utilisent les appels système pour demander au système d'exploitation d'effectuer une action pour leur compte.
En simplifiant beaucoup, tout ce que fait un programme et qui ne consiste pas à calculer sur des données passe par un appel système.

Il existe de très nombreux appels système. Linux en compte environ 330, même si ce nombre évolue au fil du temps, car des appels sont ajoutés ou abandonnés.
Chaque appel système est désigné par un *numéro d'appel système* (syscall number), en partant de 0. Pour invoquer un appel précis, le programme place son numéro dans le registre `rax`, puis exécute l'instruction `syscall`.
Par exemple, pour invoquer l'appel système numéro 42 (que l'on découvrira plus tard !), on écrirait deux instructions :

```assembly
mov rax, 42
syscall
```

C'est très puissant, et très simple !

Dans ce défi, on découvre notre premier appel système : `exit`.
L'appel `exit` met fin au programme.
En quittant explicitement le programme, on évite le plantage rencontré avec le programme précédent !

Le numéro de l'appel `exit` est `60`.
À vous de jouer : écrivez votre premier programme. Il doit placer `60` dans `rax`, puis invoquer `syscall` pour quitter proprement !
