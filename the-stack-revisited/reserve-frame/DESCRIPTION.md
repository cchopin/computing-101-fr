Dans les derniers niveaux, on a regardé vers la droite, dans le cadre de l'appelant, et vers la gauche, dans les données périmées d'un ancien appelé.
Maintenant, il faut réserver un cadre.

Jusqu'ici, les fonctions ont conservé leurs valeurs temporaires dans des registres.
Mais une fonction peut avoir besoin de plus d'espace de travail que ce que les registres peuvent contenir.
Sur x86 64 bits, une fonction réserve de l'espace de travail sur la pile en modifiant le pointeur de pile (`rsp`) pour qu'il pointe vers une adresse plus basse : `sub rsp, 256` réserve 256 octets à partir du nouveau pointeur de pile.
Ces octets sont ensuite adressables avec `[rsp]` jusqu'à `[rsp+255]`.
Ce qui se trouvait déjà sur la pile y est toujours, mais comme `rsp` a bougé vers la gauche, il faut désormais d'autres décalages par rapport à `rsp`.

Cela ne donne pas d'octets fraîchement mis à zéro.
Les octets traversés en déplaçant `rsp` sont de la mémoire de pile ordinaire, et ils peuvent contenir des restes laissés par du code antérieur.
Si ces octets servent de table ou d'ensemble de compteurs, les valeurs périmées ressembleront exactement à des valeurs écrites par la fonction.
En réalité, l'absence d'initialisation des données de pile, suivie de l'utilisation de ces déchets par le programme, est une source fréquente de vulnérabilités !
C'est pourquoi un cadre de pile destiné à des données temporaires commence normalement par une initialisation : on réserve l'espace, on y écrit des valeurs connues, on l'utilise, puis on remet `rsp` à sa place avant de retourner.

L'initialisation a lieu entre l'allocation et la libération du cadre de pile :

```asm
sub rsp, 256       # allocate a 256-byte frame
    ...            # initialize and use [rsp] through [rsp+255]
add rsp, 256       # deallocate the frame
ret
```

Cette dernière étape compte autant que la première.
`ret` dépile son adresse de retour depuis `[rsp]` : si `rsp` n'est pas revenu à sa position de départ, `ret` lira des octets erronés comme une adresse, et le programme plantera.

Écrire une fonction nommée `solve` qui réserve un cadre de pile de 256 octets, met à zéro chacun de ses octets, restaure `rsp`, puis retourne.
Le correcteur remplit le cadre potentiel d'octets non nuls avant d'appeler la fonction, puis vérifie que les 256 octets ont été effacés après son retour.
`mov byte ptr [rsp+rcx], 0` peut être utile pour effacer un octet au décalage `rcx`.

Construire en bibliothèque partagée, puis la transmettre au correcteur :

```console
hacker@dojo:~$ as -o solve.o solve.s
hacker@dojo:~$ ld -shared -o solve.so solve.o
hacker@dojo:~$ /challenge/check solve.so
```

Pour déboguer une fonction soumise à l'intérieur d'une bibliothèque partagée, voir [Écrire depuis une bibliothèque partagée](https://pwn.college/computing-101-fr/control-flow/callee-write).
