Les ordinateurs exécutent des *programmes informatiques* pour atteindre toutes sortes de buts.
Un programme peut être un jeu vidéo, un autre le navigateur qui sert à lire ce site, et ainsi de suite.

Un programme est constitué de *code informatique*, et ce code se compose d'un très grand nombre d'*instructions* élémentaires.
Chaque instruction est en général très simple. C'est leur enchaînement qui permet des choses formidables, comme regarder des mèmes sur Internet.

Ce calcul est effectué par l'*unité centrale de traitement*, ou *processeur* (Central Processing Unit, CPU), en coopération avec les autres composants matériels de l'ordinateur.
Les instructions sont transmises au processeur dans un langage appelé *langage d'assemblage* (Assembly Language). Chaque architecture de processeur a sa propre variante de ce langage.
Tout programme, quel que soit le langage dans lequel il a été écrit (C, C++, Java, Python, etc.), finit par être converti en instructions d'assemblage, ou interprété par elles.

La plupart des supports de pwn.college utilisent l'[architecture x86](https://en.wikipedia.org/wiki/X86), qui est l'architecture préférée de Zardus.
x86 a été créée par Intel à l'aube de l'ère du PC, et n'a cessé d'évoluer depuis.
Avec ARM (une autre architecture, moins cool), x86 équipe la majorité des processeurs de PC.

Dans ce module, on part du programme x86 le plus simple qu'on puisse imaginer, écrit directement en langage d'assemblage x86, puis on construit à partir de là.
Plongeons-nous dedans, et écrivons notre premier programme !

---

## Fiche de cours (ajout, hors traduction)

*Cette fiche n'est pas une traduction. Elle est rédigée à partir des diapositives de Yan Shoshitaishvili (Arizona State University), intitulées « Assembly Crash Course », et de leurs quatre parties : « Computer Architecture », « Assembly », « Data » et « Registers ». Ces diapositives sont en anglais ; leurs liens figurent dans les ressources du module. Les notions sont présentées dans l'ordre où les défis en ont besoin. Elle ne contient aucune solution de défi.*

### 1. Du programme au processeur

Un programme s'écrit le plus souvent dans un langage de haut niveau (C, C++, Rust, Python, Java, JavaScript, etc.). Le processeur, lui, ne comprend que des instructions codées en binaire. Il existe trois chemins pour passer de l'un à l'autre :

- le **compilateur** traduit le code source, une fois pour toutes, en instructions binaires (C, C++, Rust) ;
- l'**interpréteur** exécute le code au fur et à mesure, ou bien le code est d'abord transformé en **bytecode**, une représentation intermédiaire (Python, JavaScript, Java) ;
- le **compilateur JIT** (*just-in-time*, « à la volée ») traduit le bytecode en instructions binaires pendant l'exécution.

Quel que soit le chemin, le processeur reçoit au final des instructions binaires.

### 2. L'architecture d'un ordinateur

Le schéma suivant résume les composants principaux. Il est simplifié : les flèches représentent les échanges de données.

```text
   Mémoire  <-->  |                |
   Disque   <-->  |  élément de    |  <-->  Processeur (CPU)
   Réseau   <-->  |  liaison (pont)|
```

À l'intérieur du processeur, on trouve :

| Élément | Rôle |
|---|---|
| Registres | Petits emplacements de stockage, les plus rapides, qui contiennent les données sur lesquelles le processeur travaille. |
| Unité de contrôle (CU) | Lit les instructions, les interprète et coordonne les autres éléments. |
| Unité arithmétique et logique (ALU) | Effectue les calculs (addition, comparaison, opérations logiques). |
| Cache L1, puis cache L2 | Mémoires intermédiaires très rapides, entre les registres et la mémoire principale. |

Les diapositives montrent un processeur avec plusieurs cœurs, chacun ayant ses registres, son unité de contrôle, son ALU et son cache L1, et partageant un cache L2. Du plus rapide au plus lent (et du plus petit au plus grand), on passe donc des registres aux caches, puis à la mémoire, puis au disque.

L'idée de base de ce modèle date de 1945 : dans un rapport sur l'EDVAC, John von Neumann décrit un ordinateur où les instructions et les données sont stockées dans la même mémoire.

### 3. Le langage d'assemblage

Le binaire est la langue du processeur. L'assembleur en est une représentation textuelle, qui correspond directement aux instructions binaires : chaque ligne d'assembleur correspond à une instruction que le processeur exécute.

Une ligne d'assembleur suit toujours le même modèle, un peu comme une phrase :

- l'**opération** est le verbe : ce qu'on veut faire (ajouter, copier, comparer...) ;
- les **opérandes** sont les compléments : les données sur lesquelles on agit.

Une instruction peut donc ne rien avoir après l'opération, en avoir un, deux ou trois. Les opérandes peuvent être de trois types : une valeur écrite directement dans l'instruction (*valeur immédiate*), une donnée déjà placée dans un registre, ou une donnée stockée en mémoire.

Les grandes familles d'opérations sont : l'addition, la soustraction, la multiplication, la division, le déplacement de données (entre registres ou vers la mémoire), la comparaison et le test de propriétés des données.

**Dialectes.** Chaque architecture a son propre assembleur : x86, ARM, PowerPC, MIPS, RISC-V, PDP-11, etc. Pour x86, il existe deux syntaxes :

- la **syntaxe Intel**, celle du dojo : elle reflète la documentation officielle d'Intel, qui a créé l'architecture ;
- la **syntaxe AT&T**, très répandue dans les outils et les tutoriels, mais plus difficile à lire pour les débutants, parce que l'ordre des opérandes et la notation des registres y sont différents.

Ce dojo utilise la syntaxe Intel, et l'indique à l'assembleur avec la directive `.intel_syntax noprefix`.

**Pourquoi « assembleur » ?** On dit qu'un programme en assembleur est *assemblé* (et non *compilé*) en binaire. Selon les diapositives, Kathleen Booth a conçu l'un des premiers langages d'assemblage à la fin des années 1940, et le deuxième ordinateur à programme enregistré disposait d'un assembleur écrit par David Wheeler en 1948.

**Une précision sur la simplicité.** L'assembleur est très simple, parce que le processeur doit le comprendre directement. Son vocabulaire est limité, mais la difficulté vient de la précision requise : il faut savoir exactement ce que chaque registre contient à chaque instant.

### 4. Les nombres : décimal, binaire, hexadécimal

Le système décimal utilise dix chiffres (0 à 9). Le système binaire n'en utilise que deux (0 et 1). Un chiffre binaire s'appelle un *bit*. Comme en décimal, un nombre plus grand que 1 s'écrit avec plusieurs chiffres, placés selon leur puissance de la base.

Par exemple, 197 en décimal s'écrit `11000101` en binaire : 128 + 64 + 4 + 1 = 197.

Les nombres binaires sont longs à lire. On utilise donc deux notations plus compactes :

- l'**octal** (base 8), préfixé par `0` ;
- l'**hexadécimal** (base 16), préfixé par `0x`. Les chiffres au-delà de 9 sont notés A à F (A = 10, F = 15).

Un chiffre hexadécimal correspond à exactement quatre bits, ce qui en fait une notation idéale pour les valeurs binaires. Quelques valeurs à connaître :

| Décimal | Binaire | Hexadécimal |
|---|---|---|
| 10 | 1010 | 0xA |
| 16 | 10000 | 0x10 |
| 128 | 10000000 | 0x80 |
| 192 | 11000000 | 0xC0 |
| 224 | 11100000 | 0xE0 |
| 240 | 11110000 | 0xF0 |

**Pourquoi le binaire ?** Un circuit électronique est simple à construire et fiable s'il ne distingue que deux états : une lampe allumée ou éteinte, une tension haute ou basse. Des systèmes à trois états (ternaires) ont existé, mais le binaire s'est imposé.

### 5. Le texte en binaire : ASCII

Les bits servent à coder tout ce que l'ordinateur manipule : instructions, programmes, images, texte. Le premier format de codage de texte encore utilisé est l'**ASCII** (American Standard Code for Information Interchange). Il code en 7 bits l'alphabet anglais, les chiffres et les symboles courants. Pour l'essentiel :

- les majuscules valent `0x40` + la position de la lettre dans l'alphabet (A = `0x41`, B = `0x42`...) ;
- les minuscules valent `0x60` + la position de la lettre (a = `0x61`, b = `0x62`...) ;
- les chiffres valent `0x30` + la valeur du chiffre (`'0'` = `0x30`, `'7'` = `0x37`) ;
- l'espace vaut `0x20` ; les caractères inférieurs à `0x20` sont des *caractères de contrôle* : `0x09` (tabulation), `0x0a` (saut de ligne), `0x07` (sonnerie).

L'UTF-8, très répandu sur le Web (les diapositives indiquent environ 98 % des pages), est compatible avec l'ASCII : les 128 premiers caractères y ont le même codage. Le bit de poids fort (`0x80`) signale qu'un caractère est codé sur plusieurs octets.

### 6. Octets, mots et tailles

Un groupe de 8 bits s'appelle un **octet** (*byte*). Il peut prendre 256 valeurs, de `0x00` à `0xff`. L'octet s'est imposé dans les années 1960, et toutes les architectures modernes l'utilisent.

Les termes suivants désignent des tailles usuelles :

| Nom | Taille | Remarque |
|---|---|---|
| Quartet (*nibble*) | 4 bits | Un chiffre hexadécimal. |
| Octet (*byte*) | 8 bits | Une valeur de `0x00` à `0xff`. |
| Mot (*word*) | 16 bits | Sur x86, le mot désigne 2 octets, par tradition. |
| Double mot (*dword*) | 32 bits | 4 octets. |
| Quadruple mot (*qword*) | 64 bits | 8 octets. |

Attention : le mot « mot » est ambigu. Sur x86, il désigne historiquement 16 bits, alors que le processeur travaille sur des données de 64 bits. Pour éviter la confusion, on parle de « registre 64 bits » ou de « qword ».

### 7. Entiers : plage de valeurs et dépassements

Un registre de 64 bits peut représenter 2^64 valeurs différentes : de `0` à `0xffffffffffffffff`, soit 18446744073709551615 en décimal.

**Dépassement de capacité (integer overflow).** Si on ajoute 1 à `0xffffffffffffffff`, le résultat demande un 65e bit, qui n'existe pas dans le registre. Le registre ne contient alors que les 64 bits de poids faible : la valeur devient `0`. L'opération inverse, soustraire 1 à `0`, donne `0xffffffffffffffff`.

**Nombres négatifs (complément à deux).** Pour représenter les nombres négatifs, on utilise le *complément à deux* :

- le nombre `0` a une seule représentation, `0b00000000` sur 8 bits ;
- un nombre négatif est représenté par le grand nombre positif auquel il correspond : sur 8 bits, `-1` s'écrit `0b11111111` (255), et `-2` s'écrit `0b11111110` (254) ;
- le bit de poids fort reste le *bit de signe* : s'il vaut 1, le nombre est négatif ;
- l'avantage est que les additions et soustractions se calculent de la même façon, qu'on les considère comme signées ou non.

Sur 8 bits, les valeurs vont donc de -128 (`0b10000000`) à 127. Sur 64 bits, de -2^63 à 2^63 - 1.

**Vocabulaire.** Dans un mot, les bits et les octets de poids fort sont à gauche (« most significant », « leftmost », « high »), ceux de poids faible à droite (« least significant », « rightmost », « low »). Le premier bit est le bit de signe, et le dernier bit est le bit de poids le plus faible. Cette fiche ne traite pas de l'ordre dans lequel les octets sont rangés en mémoire (le *boutisme*, ou *endianness*), qui est un sujet à part.

### 8. Les registres

**Rôle.** Un registre est un emplacement de stockage à l'intérieur même du processeur. Il est beaucoup plus rapide que la mémoire, mais il en existe très peu. Les instructions lisent et écrivent principalement dans des registres.

**Taille.** La taille d'un registre correspond en général au mot de l'architecture : sur un processeur 64 bits, un registre contient 64 bits, soit 8 octets.

**Noms des registres à usage général.** Ils ont changé avec les générations de processeurs :

| Génération | Registres à usage général |
|---|---|
| 8086 | `ax`, `cx`, `dx`, `bx`, `sp`, `bp`, `si`, `di` |
| x86 (32 bits) | `eax`, `ecx`, `edx`, `ebx`, `esp`, `ebp`, `esi`, `edi` |
| amd64 (64 bits) | `rax`, `rcx`, `rdx`, `rbx`, `rsp`, `rbp`, `rsi`, `rdi`, `r8` à `r15` |

Sur amd64, on dispose donc de 16 registres à usage général. Le préfixe `r` indique la taille 64 bits, `e` la taille 32 bits.

**Pointeurs particuliers.** Certains registres ont un rôle précis :

- `rip` (*instruction pointer*) contient l'adresse de la prochaine instruction à exécuter. On ne peut pas le manipuler directement comme un registre ordinaire, par exemple avec `mov`.
- `rsp` (*stack pointer*) contient l'adresse du sommet de la pile, une zone de la mémoire qui sert au stockage temporaire. Il faut le manipuler avec précaution.
- Par convention, `rbp` sert de repère pour la pile. Ces conventions sont reprises plus loin dans le cours, selon les diapositives.

**Accès partiels.** On peut lire et écrire une partie d'un registre. Sur amd64, `rax` est découpé ainsi :

```text
rax  : 64 bits
eax  : 32 bits de poids faible de rax
ax   : 16 bits de poids faible de rax
ah   : 8 bits de poids fort de ax (bits 8 à 15)
al   : 8 bits de poids faible de ax (bits 0 à 7)
```

Les autres registres suivent le même modèle : `rcx`, `ecx`, `cx`, `ch`, `cl`, etc. Pour les registres `r8` à `r15`, on utilise les suffixes `d` (32 bits), `w` (16 bits) et `b` (8 bits).

Deux règles à retenir :

- écrire dans `ax` ou `al` ne modifie que les bits concernés : les bits de poids fort du registre restent inchangés ;
- écrire dans un registre 32 bits (par exemple `eax`) met à zéro les 32 bits de poids fort du registre 64 bits. Ce comportement est volontaire, pour des raisons de performance.

**Charger et copier des valeurs.** L'instruction `mov` se lit « déplacer », mais elle *copie* la valeur : la source n'est pas modifiée. Une valeur écrite directement dans l'instruction s'appelle une *valeur immédiate*. Pour les extensions de signe, l'instruction `movsx` recopie le bit de signe sur tous les bits de poids fort du registre de destination.

**Opérations arithmétiques et logiques.** Pour la plupart des instructions, le premier registre indiqué reçoit le résultat. Les principales instructions :

| Instruction | Équivalent | Rôle |
|---|---|---|
| `add rax, rbx` | `rax = rax + rbx` | Additionne `rbx` à `rax`. |
| `sub ebx, ecx` | `ebx = ebx - ecx` | Soustrait `ecx` à `ebx`. |
| `imul rsi, rdi` | `rsi = rsi * rdi` | Multiplie, en gardant les 64 bits de poids faible. |
| `inc rdx` / `dec rdx` | `rdx = rdx + 1` / `rdx = rdx - 1` | Incrémente ou décrémente. |
| `neg rax` | `rax = 0 - rax` | Change le signe (complément à deux). |
| `not rax` | `rax = ~rax` | Inverse chaque bit. |
| `and`, `or`, `xor` | `&`, `\|`, `^` | Opérations logiques bit à bit. |
| `shl`, `shr` | `<<`, `>>` | Décalage des bits à gauche ou à droite (remplissage par des zéros). |
| `sar` | `>>` | Décalage à droite avec extension du signe. |
| `rol`, `ror` | rotation | Les bits sortis d'un côté rentrent de l'autre. |

Pour observer le comportement de ces instructions sans risque, on peut utiliser l'outil [rappel](https://github.com/yrp604/rappel), mentionné dans les diapositives (en anglais).

**Autres registres.** Les processeurs x86 modernes possèdent d'autres registres : ceux utilisés par le système d'exploitation lui-même, ceux du calcul en virgule flottante, et ceux du calcul vectoriel sur de grandes quantités de données (32 registres `zmm` de 512 bits, selon les diapositives).

### Pour aller plus loin

- Le manuel de référence des architectures Intel 64 et IA-32, publié par Intel (en anglais), décrit toutes les instructions et tous les registres.
- Les pages de manuel Linux, accessibles avec la commande `man`, décrivent les appels système et les conventions du système.
