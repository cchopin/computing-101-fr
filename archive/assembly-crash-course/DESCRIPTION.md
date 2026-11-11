Maintenant que les bases de l'assembleur sont acquises, on explore quelques instructions et concepts supplémentaires !
Le cours accéléré d'assembleur (Assembly Crash Course) parcourt de nombreuses choses que l'on peut faire en assembleur, et prépare aux aventures à venir !

Pour interagir avec un niveau, on peut soit lancer le défi avec un fichier ELF en argument (par exemple `/challenge/run /path/to/your/elf`), soit envoyer des octets bruts sur l'entrée standard de ce programme.

---

## Fiche de cours (ajout, hors traduction)

*Cette fiche n'est pas une traduction. Elle est rédigée à partir des présentations de Yan Shoshitaishvili (Arizona State University), intitulées « Assembly Crash Course » : « Computer Architecture », « Assembly », « Data », « Registers », « Memory », « Control Flow », « System Calls » et « Building Programs ». Ces présentations sont en anglais ; leurs liens figurent dans les ressources du module. Elle ne contient aucune solution de défi.*

### 1. Rappels : architecture, assembleur, données, registres

Ces quatre parties sont des révisions du module précédent. La fiche correspondante est dans le module « Votre premier programme », à lire en cas de doute sur les registres, les opérandes ou la notation hexadécimale.

### 2. Mémoire

**Pourquoi la mémoire.** Les registres sont coûteux et peu nombreux. Il faut un endroit pour stocker beaucoup de données, accessible assez vite. Cet endroit est la mémoire du système.

**Espace d'adressage d'un processus.** Chaque adresse désigne un octet. L'espace va de `0x10000` à `0x7fffffffffff`, soit environ 127 téraoctets d'adresses. Ce n'est pas de la RAM réelle : c'est de la mémoire virtuelle, que le système d'exploitation remplit en partie au démarrage du processus. Le processus peut demander davantage de mémoire au système. Une carte typique contient le code binaire du programme, les données allouées dynamiquement, le code des bibliothèques, la pile, et les zones réservées au système.

**La pile.** La pile sert ici à stocker temporairement des données. On y empile une valeur avec `push` et on la dépile avec `pop`. Comme `mov`, `push` laisse la valeur dans le registre source. Les immédiats ne peuvent être empilés que sur 32 bits, même sur un processeur 64 bits.

Le registre `rsp` contient l'adresse du sommet de la pile. Historiquement, la pile croît vers les adresses basses : `push` diminue `rsp`, et `pop` l'augmente.

**Accès à la mémoire avec `mov`.** Une instruction `mov` peut lire ou écrire à une adresse entre crochets, par exemple `[rax]`. Chaque adresse contient un octet, donc une écriture de 8 octets occupe 8 adresses consécutives. On peut utiliser des tailles partielles (`ebx`, `bh`) pour lire ou écrire moins d'octets. Attention : écrire dans un registre 32 bits met à zéro les 32 bits de poids fort, mais écrire 32 bits en mémoire ne pose pas ce problème.

**Ordre des octets (little-endian).** Sur la plupart des systèmes modernes, les octets d'une valeur multi-octets sont rangés du plus faible au plus fort : c'est le *little-endian*. Les diapositives expliquent l'origine de ce choix : des processeurs Intel anciens l'ont adopté, et les architectures suivantes ont gardé la compatibilité. Les bits d'un même octet ne sont jamais mélangés, seul l'ordre des octets change.

**Calcul d'adresse.** Une adresse peut combiner un registre de base, un registre d'index multiplié par 1, 2, 4 ou 8, et une constante : `[base + index*échelle + décalage]`. L'instruction `lea` calcule cette adresse sans lire la mémoire. Au-delà de cette forme, le processeur ne calcule pas d'adresse.

**Adressage relatif à `rip`.** `lea` peut accéder directement à `rip`, ce qui permet de désigner des données placées près du code. Cette technique est aussi à la base de certaines protections modernes.

**Écrire une valeur immédiate.** Quand la taille n'est pas déductible du registre, il faut la préciser (par exemple `DWORD PTR` pour 32 bits).

### 3. Flot de contrôle

**Décider.** Le processeur exécute les instructions dans l'ordre, sauf si une instruction de saut modifie `rip`. Une instruction `jmp` saute à une étiquette sans condition.

**Sauts conditionnels.** Les instructions `je`, `jne`, `jg`, `jb`, etc. sautent selon l'état des *indicateurs* (flags) du registre `rflags` :

- le drapeau de retenue (`CF`) indique un dépassement non signé ;
- le drapeau zéro (`ZF`) indique un résultat nul ;
- le drapeau de débordement (`OF`) indique un dépassement signé ;
- le drapeau de signe (`SF`) indique un résultat négatif.

Les instructions arithmétiques modifient ces indicateurs. `cmp` soustrait sans garder le résultat, et `test` fait un ET logique sans garder le résultat. Les sauts non signés (`ja`, `jb`, `jae`, `jbe`) et signés (`jg`, `jl`, `jge`, `jle`) lisent des combinaisons différentes des indicateurs : il faut choisir celui qui correspond à l'interprétation des nombres.

**Boucles.** Une boucle se construit avec une étiquette, une instruction d'incrémentation, une comparaison, puis un saut conditionnel vers l'étiquette.

**Fonctions.** `call` empile l'adresse de l'instruction suivante, puis saute. `ret` dépile cette adresse dans `rip`.

**Convention d'appel.** L'appelant et l'appelé doivent se mettre d'accord sur le passage des arguments et sur la valeur de retour. Sous Linux amd64, les arguments passent dans `rdi`, `rsi`, `rdx`, `rcx`, `r8` et `r9`, et la valeur de retour est dans `rax`. Les registres `rbx`, `rbp`, `r12` à `r15` sont préservés par la fonction appelée ; les autres peuvent être modifiés, à condition de sauvegarder leur valeur sur la pile si on en a besoin. `rsp` doit toujours rester cohérent.

### 4. Appels système

**Interagir avec le monde extérieur.** Un programme ne peut pas, seul, lire un fichier, écrire à l'écran ou quitter proprement : il doit demander au système d'exploitation. C'est le rôle de l'instruction `syscall`.

**Convention d'un appel système.** Le numéro de l'appel est placé dans `rax`. Les arguments passent dans `rdi`, `rsi`, `rdx`, `r10`, `r8` et `r9`, dans cet ordre. La valeur de retour est dans `rax`. Les interfaces des appels système changent très rarement. Linux en compte plus de 300. Les appels classiques sont `read`, `write`, `open`, `fork`, `execve`, `wait` et `exit`. Une table des numéros est donnée dans les diapositives (en anglais).

**Chaînes de caractères.** Un argument « chaîne » (par exemple un chemin de fichier) est une suite d'octets contigus, terminée par un octet nul (`0`). Pour la construire, on écrit les octets un par un sur la pile, puis on passe l'adresse du début de la chaîne à l'appel système.

**Constantes.** Certains appels attendent des constantes, comme les modes d'ouverture d'un fichier. Leur valeur se trouve dans les fichiers d'en-tête C, et on peut la vérifier avec un petit programme C qui les affiche (en anglais).

### 5. Construire des programmes

**De l'assembleur au binaire.** Un programme d'assemblage est transformé en fichier objet par l'assembleur (`as`), puis lié en exécutable par l'éditeur de liens (`ld`), ou directement par `gcc`. La directive `.global _start` indique le point d'entrée et évite l'avertissement de l'éditeur de liens.

**Lire un programme.** L'outil `objdump` désassemble un exécutable et affiche ses instructions. `objcopy` permet d'extraire la seule section de code (`.text`) pour l'examiner en binaire.

**Débogage.** Un débogueur comme `gdb` peut placer des points d'arrêt. L'instruction `int3` déclenche un point d'arrêt : le programme s'arrête, et on peut inspecter son état. Dans ce cours, le défi fournit un débogueur automatique : `int3` suffit (voir l'astuce ci-dessus).

**Autres outils.** `strace` affiche les appels système d'un programme, ce qui en fait un bon point de départ pour déboguer. `rappel` permet d'explorer les effets d'une instruction, et la documentation de référence de x86 (en anglais) décrit chaque instruction.
