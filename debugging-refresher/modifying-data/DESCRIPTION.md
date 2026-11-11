En réalité, gdb contrôle entièrement le processus cible.
Non seulement on peut analyser l'état du programme, mais on peut aussi le modifier.
gdb n'est sans doute pas le meilleur outil pour maintenir un programme sur le long terme, mais il est parfois utile pour modifier rapidement le comportement du processus cible, afin de l'analyser plus facilement.

On modifie l'état du programme avec la commande `set`.
Par exemple, `set $rdi = 0` met `$rdi` à zéro.
`set *((uint64_t *) $rsp) = 0x1234` place la valeur 0x1234 dans le premier élément de la pile.
`set *((uint16_t *) 0x31337000) = 0x1337` écrit 0x1337 sur 2 octets à l'adresse 0x31337000.

Supposons que la cible soit une application réseau qui lit depuis une socket, sur le descripteur 42.
Pour l'analyse, il serait peut-être plus simple qu'elle lise plutôt depuis l'entrée standard.
On peut y parvenir avec le script gdb suivant :

```gdb
start
catch syscall read
commands
  silent
  if ($rdi == 42)
    set $rdi = 0
  end
  continue
end
continue
```

Cet exemple montre comment interrompre automatiquement sur les appels système, et comment utiliser des conditions dans les commandes pour n'exécuter certaines commandes gdb que si la condition est remplie.

Dans le niveau précédent, la solution en script gdb devait sans doute encore être copiée et collée à la main.
Cette fois, essayez d'écrire un script qui n'a jamais besoin de dialoguer avec le programme, et qui résout automatiquement chaque défi en modifiant correctement les registres et la mémoire.

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
- La commande [set](https://sourceware.org/gdb/current/onlinedocs/gdb#Assignment) de gdb. Dans ce défi, cette commande sert précisément à modifier les registres (comme `$rdi` ci-dessus) et la mémoire (voir le bas de la section indiquée).
