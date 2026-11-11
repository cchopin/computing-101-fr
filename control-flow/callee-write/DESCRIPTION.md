Jusqu'ici, chaque programme écrit était un exécutable complet : il commence à `_start`, s'exécute, et quitte avec un appel système.
Dans ce défi, le code sera **une seule fonction à l'intérieur d'une bibliothèque partagée**, et non un exécutable autonome.

Une bibliothèque partagée (un fichier `.so` sous Linux) est un morceau de code compilé qu'un *autre* programme charge à l'exécution, et dont il appelle les fonctions.
En général, ces bibliothèques fournissent des fonctions utilitaires, comme l'analyse de fichiers image (par exemple, `libpng` lit les fichiers PNG), ou des tâches liées au système (`libc` fournit beaucoup de gestion de la mémoire, des fichiers et des interactions avec le système).
Au plus profond, l'interaction avec le système d'exploitation passe par des appels système, mais les bibliothèques offrent une interface plus pratique que les appels système bruts.

Ce défi joue le rôle d'un programme qui charge la bibliothèque (à l'aide de `dlopen` de `libc`), recherche la fonction par son nom, et l'**appelle** avec des arguments.
Dans la terminologie de l'informatique, le code écrit est l'*appelé* (callee), et le défi est l'*appelant* (caller).

**L'instruction `call`.**  
Comment le correcteur entre-t-il dans le code écrit ?
Il exécute une instruction encore inconnue : `call`.

`call <cible>` est l'instruction d'appel de fonction de x86. Elle fait deux choses :

1. Elle empile l'adresse de l'instruction qui suit le `call` (l'*adresse de retour*) sur la pile.
2. Elle saute vers `<cible>`.

Dans notre cas, le correcteur exécute l'équivalent de `call solve`, et l'exécution arrive au début de la fonction `solve`.
Rien de spécial n'est à faire pour « recevoir » l'appel : il suffit de commencer à exécuter.

Pour ce premier défi, rien de spécial n'est non plus à faire pour *terminer* l'appel.
Le traitement de l'adresse de retour sauvegardée sera vu dans le défi suivant ; pour l'instant, le code se termine par l'appel système `exit` déjà connu.
C'est la même forme que pour tous les programmes écrits jusqu'ici : seul change *qui* a lancé le code.

**Écrire la fonction.**  
Le fichier d'assembleur doit ressembler à ceci :

```asm
.intel_syntax noprefix
.global solve
solve:
    <code écrit, se terminant par un appel système exit>
```

La ligne `.global solve` indique à l'assembleur de rendre ce code accessible, pour que d'autres puissent le trouver, exactement comme `.global _start` pour les exécutables, dans le défi [Construire](https://pwn.college/computing-101-fr/your-first-program).
L'étiquette `solve:` indique où se trouve le code.

**Construire une bibliothèque partagée.**  
On sait déjà assembler un programme avec `as` et le lier avec `ld`.
Pour produire une bibliothèque partagée au lieu d'un exécutable, passez `-shared` à `ld` :

```console
hacker@dojo:~$ as -o your-solve.o your-solve.s
hacker@dojo:~$ ld -shared -o your-solve.so your-solve.o
```

Soumettre ensuite le `.so` au correcteur :

```console
hacker@dojo:~$ /challenge/check your-solve.so
```

**La convention d'appel.**  
Quand le correcteur appelle `solve`, il passe les arguments dans des registres.
Dans ce défi, la fonction `solve` reçoit deux arguments :

| Registre | Rôle à l'entrée                                       |
|----------|-------------------------------------------------------|
| `rdi`    | Premier argument (un pointeur vers un tampon d'octets) |
| `rsi`    | Second argument (la longueur de ce tampon)            |

On a déjà vu `rdi` servir de premier argument à un appel système (le code de retour, un descripteur de fichier, etc.).
C'est parce que les appels système Linux et les fonctions Linux utilisent la même convention pour les premiers registres d'arguments.

Pour ce défi, le correcteur passera **le flag** comme tampon, avec sa longueur dans `rsi`.
Écrire les `rsi` octets à partir de `rdi` vers le descripteur de fichier 1 (sortie standard) avec l'appel système `write` (comme précédemment), puis quitter proprement avec le code `0`.
En cas de réussite, `solve` affiche le flag !

----
**Indice :** `write()` prend ses arguments dans l'ordre suivant : le descripteur de fichier (1 dans `rdi` pour la sortie standard), le tampon (un pointeur vers la mémoire, dans `rsi`) et la taille (dans `rdx`).
C'est *différent* des arguments avec lesquels la fonction sera appelée : il faut donc réorganiser certaines valeurs !

**Déboguer la solution.**  
Comme le code est une fonction dans une bibliothèque partagée, il n'y a pas de point d'entrée à lancer directement sous `gdb`, mais on peut *en fournir un*.
Ajouter un petit `_start` qui simule l'appel du correcteur : pointer `rdi` vers un tampon de remplacement, placer sa longueur dans `rsi`, et faire `call solve`.
La logique peut alors être suivie dans `gdb`, sans flag ni privilèges :

```asm
.global _start
_start:
    push 0x41414141   // placer quatre octets 'A' (0x41) sur la pile pour remplacer le flag
    mov rdi, rsp      // premier argument : un pointeur vers ces octets
    mov rsi, 4        // second argument : le nombre d'octets à afficher
    int3              // facultatif : gdb s'arrête ici sans point d'arrêt
    call solve        // solve s'exécute, affiche les octets, et quitte tout seul
```

Assembler et lier comme un exécutable normal (sans `-shared` : cette version a un point d'entrée), puis le charger dans `gdb` :

```console
hacker@dojo:~$ as -o debug.o debug.s
hacker@dojo:~$ ld -o debug debug.o
hacker@dojo:~$ gdb ./debug
(gdb) run
```

L'exécution s'arrête au `int3` ; il faut ensuite avancer pas à pas avec les techniques du module [Introspection logicielle](https://pwn.college/computing-101-fr/introspecting), en surveillant les registres et le tampon.
Si `solve` est correct, cela affiche `AAAA`, et la même logique affichera le vrai flag lorsque le `.so` sera soumis au correcteur.

Il est aussi possible de déboguer le programme natif avec des octets de remplacement au lieu du flag.
`/challenge/check` est le script Python du correcteur : ne pas le charger comme exécutable dans `gdb`.
Le programme natif qui charge le `.so` est `/challenge/harness` :

```console
hacker@dojo:~$ gdb --args /challenge/harness your-solve.so
(gdb) run
```

Le correcteur lancera ce même harnais avec le vrai flag lorsque le `.so` sera soumis.
