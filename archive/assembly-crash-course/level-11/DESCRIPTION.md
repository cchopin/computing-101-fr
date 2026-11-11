Avant chaque exécution, le niveau place dynamiquement des valeurs en mémoire, et ces valeurs changent à chaque fois. Il faut donc effectuer un calcul, sous une forme ou une autre, avec les registres. Le niveau indique quels registres sont préremplis et où placer le résultat. Dans la plupart des cas, c'est `rax`.

Dans ce niveau, on travaille avec la mémoire. Il faut lire ou écrire des données stockées de façon linéaire en mémoire. Si ce point n'est pas clair, on peut revoir le module sur l'adressage linéaire de *Ike* (en anglais). On peut aussi devoir déréférencer des adresses, parfois plusieurs fois, vers des données placées dynamiquement en mémoire pour le niveau.

Rappel sur les tailles :

- Quad word (mot quadruple) = 8 octets = 64 bits
- Double word (mot double) = 4 octets = 32 bits
- Word (mot) = 2 octets = 16 bits
- Byte (octet) = 1 octet = 8 bits

En x86_64, on peut accéder à chacune de ces tailles lorsqu'on déréférence une adresse, comme pour les accès plus petits ou plus grands aux registres :

- `mov al, [address]` <=> place l'octet de poids faible situé à l'adresse dans `rax`
- `mov ax, [address]` <=> place le mot de poids faible situé à l'adresse dans `rax`
- `mov eax, [address]` <=> place le mot double de poids faible situé à l'adresse dans `rax`
- `mov rax, [address]` <=> place le mot quadruple complet situé à l'adresse dans `rax`

Il faut faire ceci :

- Placer dans `rax` l'octet situé à `0x404000`
- Placer dans `rbx` le mot situé à `0x404000`
- Placer dans `rcx` le mot double situé à `0x404000`
- Placer dans `rdx` le mot quadruple situé à `0x404000`
