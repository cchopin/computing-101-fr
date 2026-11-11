Passons maintenant à GDB.
GDB signifie le **G**NU **D**e**b**ugger, le débogueur GNU, qui sert en général à traquer et comprendre les bugs.
Plus précisément, un débogueur est un outil qui permet d'observer et d'inspecter de près un autre processus.
Il existe de nombreux débogueurs célèbres, et dans l'univers Linux, gdb est de loin le plus courant.

On apprend gdb pas à pas, au moyen d'une série de défis.
Dans celui-ci, on se contente de le lancer.
On s'y prend ainsi :

```console
hacker@dojo:~$ gdb /path/to/binary/file
```

Dans ce défi, le binaire qui contient le secret est `/challenge/debug-me`.
Une fois ce fichier chargé dans gdb, le reste se fait « par magie » : nous nous chargeons de l'analyse et nous vous donnons le nombre secret.
Dans les niveaux suivants, vous apprendrez à retrouver ce nombre par vous-même !

Une fois le nombre obtenu, échangez-le contre le flag avec `/challenge/submit-number`.
