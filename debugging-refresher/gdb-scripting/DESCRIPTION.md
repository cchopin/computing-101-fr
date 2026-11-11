On écrit du code pour exprimer une idée qui peut être reproduite et affinée.
On peut considérer notre analyse comme un programme qui prend la cible à analyser en guise de données.
Comme le dit l'adage, le code est une donnée, et la donnée est du code.

Utiliser gdb de façon interactive, comme dans les niveaux précédents, est très puissant, mais les scripts gdb le sont tout autant.
En écrivant un script gdb, on peut très vite créer un outil d'analyse sur mesure.
Si vous savez utiliser gdb en interactif, vous savez déjà écrire un script gdb : la syntaxe est exactement la même.
On peut écrire les commandes dans un fichier, par exemple `x.gdb`, puis lancer gdb avec l'option `-x <PATH_TO_SCRIPT>`.
Ce fichier exécute toutes les commandes une fois gdb lancé.
On peut aussi exécuter une commande isolée avec `-ex '<COMMAND>'`.
Plusieurs arguments `-ex` peuvent être enchaînés.
Enfin, certaines commandes peuvent être exécutées à chaque session gdb si on les place dans `~/.gdbinit`.
Vous voudrez sans doute y mettre `set disassembly-flavor intel`.

Dans un script gdb, les *commandes de point d'arrêt* (breakpoint commands) sont une construction très puissante. Considérez le script suivant :

```gdb
start
break *main+42
commands
  x/gx $rbp-0x32
  continue
end
continue
```

Ici, chaque fois que l'instruction à `main+42` est atteinte, on affiche une variable locale, puis on poursuit l'exécution.

Voyons maintenant un script similaire, un peu plus avancé, qui utilise des commandes que vous n'avez pas encore vues :

```gdb
start
break *main+42
commands
  silent
  set $local_variable = *(unsigned long long*)($rbp-0x32)
  printf "Current value: %llx\n", $local_variable
  continue
end
continue
```

Ici, `silent` indique que gdb ne doit pas signaler qu'un point d'arrêt a été atteint, pour une sortie plus lisible.
Puis la commande `set` définit une variable dans la session gdb, dont la valeur est celle de notre variable locale.
Enfin, la valeur courante est affichée avec une chaîne formatée.

Utilisez les scripts gdb pour récupérer les valeurs aléatoires de ce niveau.
Cela peut paraître difficile, mais cela vous servira beaucoup dans la suite de votre parcours.

----
**DOCUMENTATION UTILE :**
- La commande [run](https://sourceware.org/gdb/current/onlinedocs/gdb#Starting) de gdb
- La commande [continue](https://sourceware.org/gdb/current/onlinedocs/gdb#Continuing-and-Stepping) de gdb
- La commande [info](https://sourceware.org/gdb/current/onlinedocs/gdb#Registers) de gdb
- La commande [print](https://sourceware.org/gdb/current/onlinedocs/gdb#Data) de gdb
- La commande [examine](https://sourceware.org/gdb/current/onlinedocs/gdb#Memory) de gdb
- La commande [break](https://sourceware.org/gdb/current/onlinedocs/gdb#Set-Breaks) de gdb
- La commande [display](https://sourceware.org/gdb/current/onlinedocs/gdb#Auto-Display) de gdb
- Les [différentes commandes de pas à pas](https://sourceware.org/gdb/current/onlinedocs/gdb#Continuing-and-Stepping) de gdb (toute cette section)
- Les [commandes de point d'arrêt scriptées](https://sourceware.org/gdb/current/onlinedocs/gdb#Break-Commands) de gdb
