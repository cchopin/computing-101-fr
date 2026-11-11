Jusqu'ici, le débogage que vous avez fait était **préemptif** : *vous* (le débogueur) lanciez le programme avec `stepi`, ce qui l'arrêtait de force pour le déboguer, sans que le programme en soit forcément conscient.
Dans ce défi, nous découvrons un autre modèle, où c'est le programme qui décide du moment où le débogueur s'arrête.
On parle alors de débogage **coopératif**.

Sur notre architecture x86 familière, le programme peut signaler qu'il souhaite être débogué avec l'instruction `int3`.
Si un débogueur est attaché au moment où `int3` est exécuté, il arrête le programme.
On parle alors de **point d'arrêt** (breakpoint) du programme.

Nous verrons plus tard comment poser des points d'arrêt depuis le débogueur lui-même, ce qui revient au modèle préemptif.
Dans ce défi, le vérificateur exécute votre programme sous gdb et attend que votre programme déclenche son propre point d'arrêt.
Pour cela, au lieu de lancer le programme avec `starti` puis de l'arrêter immédiatement, on utilise la commande `run` de gdb, qui l'exécute jusqu'à ce qu'un point d'arrêt soit atteint !

Quand votre programme exécute `int3`, gdb s'arrête et le vérificateur examine `$rdi`.
Si `$rdi` vaut `1337` à ce moment-là, vous obtenez le flag !

Écrivez un programme qui :

1. place `1337` dans `rdi` ;
2. exécute `int3` pour remettre le contrôle au débogueur.

Assemblez et liez votre code en exécutable ELF, puis soumettez cet exécutable :

```console
hacker@dojo:~$ /challenge/check /tmp/your-program
```

----
**NOTE :**
Quand un `int3` est exécuté par un programme qui ne tourne *pas* sous débogueur, vous voyez :

```
hacker@dojo:~$ /tmp/your-program
Trace/breakpoint trap
hacker@dojo:~$
```

Et le programme se termine...
Si vous voulez que le programme s'exécute en dehors d'un débogueur, supprimez cet `int3` !
