Dans le niveau précédent, on a regardé vers la droite, dans le cadre de l'appelant.
Maintenant, on regarde vers la gauche, vers des octets laissés par une fonction qui a déjà retourné.

Lorsque le code appelle une fonction, celle-ci peut déplacer `rsp` vers la gauche et utiliser sa propre mémoire de pile.
Au retour, elle ramène `rsp` vers la droite, mais les octets qu'elle a écrits ne sont pas effacés automatiquement.
Ils deviennent des données de pile périmées : de la mémoire ordinaire laissée par un code déjà terminé.
Le logiciel pourrait effacer ces octets avant de retourner, mais effacer des données demande d'exécuter davantage d'instructions et d'écrire davantage de mémoire.
Lorsque les données restantes sont sensibles, sauter cet effacement peut devenir une vulnérabilité.
Ce niveau commence par la plus petite version du problème : une seule valeur périmée de 8 octets.

Ce défi passe à `solve` un pointeur de fonction nommé `load_secret`.
L'appeler en premier.
Elle stocke un secret de 8 octets dans son propre cadre de pile puis retourne, en laissant ces octets à un décalage négatif par rapport au `rsp` actuel.
Le correcteur indiquera le décalage exact.

Comme le but est de retourner la valeur de 8 octets elle-même, la charger avec `mov` :

```asm
mov rax, qword ptr [rsp-0x40]
```

Ce décalage n'est qu'un exemple : utiliser celui affiché par le correcteur.
Écrire une fonction nommée `solve` qui appelle `load_secret`, charge la valeur périmée de 8 octets dans `rax`, puis retourne.

Construire en bibliothèque partagée, puis la transmettre au correcteur :

```console
hacker@dojo:~$ as -o solve.o solve.s
hacker@dojo:~$ ld -shared -o solve.so solve.o
hacker@dojo:~$ /challenge/check solve.so
```

Pour déboguer une fonction soumise à l'intérieur d'une bibliothèque partagée, voir [Écrire depuis une bibliothèque partagée](https://pwn.college/computing-101-fr/control-flow/callee-write).
