Un vrai début de programmeur en assembleur x86 ! On a déjà réglé des registres, déclenché des appels système et écrit un premier programme qui se termine proprement.
Il est temps d'aborder un nouveau concept majeur : la *mémoire*.

Un être humain dispose d'une [mémoire à court terme](https://en.wikipedia.org/wiki/Short-term_memory) et d'une [mémoire à long terme](https://en.wikipedia.org/wiki/Long-term_memory).
Pour effectuer un calcul précis, le cerveau charge en mémoire à court terme les informations apprises auparavant, agit sur elles, puis range éventuellement les nouveaux résultats en mémoire à long terme.
La société a aussi inventé d'autres formes de stockage, plus durables : la tradition orale, les journaux, les livres et Wikipédia.
Si la mémoire à long terme n'a pas assez de place, ou si l'information n'est pas assez importante pour y être gardée, on peut la rechercher sur Wikipédia, la mémoriser, puis la ramener en mémoire à court terme quand on en a besoin.

Cette hiérarchie d'accès va de « petit mais immédiatement accessible » (la mémoire à court terme, qui n'[enregistre que 5 à 9 éléments](https://www.simplypsychology.org/short-term-memory.html)) à « grand mais lent » (la mémoire à long terme), puis à « énorme mais extrêmement lent » (la recherche sur Wikipédia). Elle constitue le fondement de la [hiérarchie de la mémoire](https://en.wikipedia.org/wiki/Memory_hierarchy) de l'informatique moderne.
Le premier niveau, celui des registres, a été vu dans le module précédent : ils sont peu nombreux, mais très rapides.

Plus spacieuse que l'ensemble des registres réunis, mais beaucoup, beaucoup plus lente d'accès, se trouve la mémoire de l'ordinateur. C'est elle que ce module explore, pour donner un aperçu d'un autre niveau de la hiérarchie de la mémoire.

---

## Fiche de cours (ajout, hors traduction)

*Cette fiche n'est pas une traduction. Elle est rédigée à partir de la présentation « Assembly Crash Course », partie « Memory », de Yan Shoshitaishvili (Arizona State University), en anglais. Le rappel sur l'architecture de l'ordinateur reprend la présentation déjà vue dans le module 1. Elle ne contient aucune solution de défi.*

### 1. Pourquoi une mémoire

Les registres sont peu nombreux et coûteux à fabriquer. Il faut donc un endroit où stocker beaucoup de données, accessibles assez rapidement quand on en a besoin : c'est la **mémoire** de l'ordinateur (mémoire vive, ou RAM).

Du point de vue d'un processus, la mémoire est une suite d'adresses. Chaque adresse désigne **un octet**. Le processus s'en sert pour toutes sortes d'échanges : entre les registres et la mémoire, entre la mémoire et le disque, avec le réseau, ou avec la carte graphique.

| Niveau | Taille | Vitesse |
|---|---|---|
| Registres | quelques dizaines d'octets | la plus rapide |
| Caches (L1, L2) | de quelques kio à quelques Mio | très rapide |
| Mémoire vive | de quelques Gio à plusieurs dizaines de Gio | rapide, mais bien plus lente que les registres |
| Disque | de quelques centaines de Gio à plusieurs To | lent |

### 2. L'espace d'adressage d'un processus

Chaque processus voit sa propre mémoire, dite *virtuelle*. Selon les diapositives, les adresses vont de `0x10000` (début, pour des raisons de sécurité) jusqu'à `0x7fffffffffff`, la limite fixée par l'architecture et le système d'exploitation. Comme chaque adresse désigne un octet, cet espace représente environ 128 Tio (2^47 octets ; les diapositives indiquent 127 To).

Il ne s'agit pas de 128 Tio de RAM réelle : l'espace est « fictif » ou virtuel. Le système d'exploitation remplit une partie de cet espace au démarrage du processus, et le processus peut en demander davantage. On y trouve notamment :

- le code du programme ;
- la mémoire allouée dynamiquement, gérée par les bibliothèques ;
- le code des bibliothèques ;
- la pile (*stack*), décrite plus bas ;
- des zones auxiliaires du système ;
- les régions mappées à la demande par le processus.

### 3. La pile

La pile sert d'abord au stockage temporaire de valeurs.

- `push` place une valeur sur la pile, qu'il s'agisse d'un registre ou d'une valeur immédiate. Sur x86-64, une valeur immédiate poussée est au maximum de 32 bits.
- `pop` retire la valeur au sommet de la pile et la place dans le registre indiqué.
- `push` ne modifie pas le registre source, de même que `mov`.

Le processeur sait où se trouve la pile grâce au registre `rsp`, qui contient l'adresse de son sommet.

Une particularité historique : la pile **grandit vers les adresses basses**. Ainsi, `push` *diminue* `rsp` (de 8 octets pour un registre 64 bits), et `pop` l'*augmente*. Par exemple, `push rcx` est équivalent à `sub rsp, 8` suivi de `mov [rsp], rcx`.

### 4. Lire et écrire en mémoire avec `mov`

Les crochets `[ ]` indiquent un accès à la mémoire. Quand on écrit `mov rbx, [rax]`, le processeur lit en mémoire la donnée située à l'adresse contenue dans `rax`, et la place dans `rbx`. Dans l'autre sens, `mov [rax], rbx` écrit le contenu de `rbx` à l'adresse contenue dans `rax`.

Par défaut, la taille de l'accès dépend du registre utilisé : un registre 64 bits lit ou écrit 8 octets. Il est possible de lire ou d'écrire une partie seulement :

- `mov [rax], ebx` écrit les 4 octets de poids faible de `rbx` ;
- `mov bh, [rax]` lit 1 octet dans `bh`.

Attention, un point déjà vu dans le module 1 : lire 32 bits dans un registre met à zéro les 32 bits de poids fort du registre 64 bits. Écrire 32 bits *en mémoire* ne pose pas ce problème.

Quand la taille ne peut pas être déduite d'un registre, par exemple pour une valeur immédiate écrite en mémoire, il faut la préciser : `DWORD PTR` pour 4 octets, `QWORD PTR` pour 8 octets, `BYTE PTR` pour 1 octet.

### 5. L'ordre des octets : le « petit-boutisme »

Sur la plupart des systèmes modernes, les valeurs sur plusieurs octets sont stockées **à l'envers** : l'octet de poids faible est placé à l'adresse la plus basse. On parle de *little endian*, ou petit-boutisme (en anglais *endianness*).

Exemple : si l'on écrit la valeur `0xc001ca75` sur 4 octets à partir de l'adresse `0x10000`, la mémoire contient, dans l'ordre des adresses, les octets `75`, `ca`, `01`, `c0`.

Les octets ne sont réordonnés que pour les lectures et écritures de valeurs multi-octets. Les bits à l'intérieur d'un octet ne sont jamais inversés.

Selon les diapositives, ce choix remonte au microprocesseur 8008 d'Intel (1972), conçu pour la société Datapoint, qui utilisait ce format pour simplifier la gestion des retenues dans les calculs. Les processeurs suivants ont gardé cette convention pour rester compatibles avec leurs prédécesseurs.

### 6. Calculer des adresses

Une adresse peut être calculée à partir de plusieurs éléments : un registre de base, un registre d'index multiplié par une échelle (1, 2, 4 ou 8), et un décalage fixe, appelé *offset*. La forme générale est `[base+index*échelle+décalage]`.

Par exemple, `mov rbx, [rsp+rax*8]` lit 8 octets à l'adresse située à `rax` × 8 octets au-dessus du sommet de la pile. Ce calcul est limité à cette forme : on ne peut pas multiplier deux registres entre eux.

L'instruction `lea` (*Load Effective Address*) calcule une adresse sans lire la mémoire, et place le résultat dans un registre. Elle est souvent utilisée pour effectuer des calculs arithmétiques simples.

### 7. Adressage relatif à `rip`

L'instruction `lea` peut accéder directement au registre `rip` : `lea rax, [rip]` place dans `rax` l'adresse de l'instruction suivante. Il est aussi possible de lire des données situées à proximité du code avec `mov`, en utilisant `rip` comme base : c'est très pratique pour les données rangées près du code. Certaines protections de sécurité modernes reposent sur ce mécanisme.

### 8. Ce qui reste à voir

Les diapositives mentionnent d'autres régions de mémoire, créées par les appels système `mmap` ou par la fonction `malloc`. Elles seront étudiées dans les modules suivants.

### Pour aller plus loin

- La page Wikipédia sur le [boutisme](https://en.wikipedia.org/wiki/Endianness) (en anglais).
- Le manuel de référence des architectures Intel 64 et IA-32, publié par Intel (en anglais), pour la liste complète des modes d'adressage.
