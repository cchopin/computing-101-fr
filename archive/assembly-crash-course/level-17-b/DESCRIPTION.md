Avant chaque exécution, le niveau place dynamiquement des valeurs en mémoire, et ces valeurs changent à chaque fois. Il faut donc effectuer un calcul, sous une forme ou une autre, avec les registres. Le niveau indique quels registres sont préremplis et où placer le résultat. Dans la plupart des cas, c'est `rax`.

Dans ce niveau, on travaille avec le contrôle du flot d'exécution. Il s'agit d'utiliser des instructions qui contrôlent, directement ou indirectement, le registre spécial `rip`, le pointeur d'instruction. On utilisera des instructions comme `jmp`, `call`, `cmp` et leurs variantes pour obtenir le comportement demandé.

Rappel : pour tous les sauts, il existe trois types :

- Les sauts relatifs : on saute d'un certain nombre d'octets vers l'avant ou l'arrière, à partir de l'instruction courante.
- Les sauts absolus : on saute vers une adresse mémoire fixe.
- Les sauts indirects : on saute vers l'adresse contenue dans un registre ou un emplacement mémoire.

Ici, on se concentre sur les sauts relatifs. Cela signifie qu'on demande au processeur de « sauter en avant d'un certain nombre d'octets à partir de l'endroit où il s'exécute ». C'est utile, car le code peut se déplacer en mémoire et le saut atteint quand même la bonne cible.

Pour écrire un saut relatif, il faut quelques outils :
* `labels` (étiquettes) : au lieu de calculer les adresses à la main, on peut utiliser des étiquettes comme repères. L'assembleur calcule automatiquement le décalage entre l'instruction de saut et l'étiquette.
* `nop` (*No Operation*) : une instruction d'un seul octet qui ne fait rien. Sa taille est prévisible, et elle peut servir de remplissage pour obtenir une distance exacte.
* `.rept` (directive de répétition) : une directive qui demande à l'assembleur de répéter une instruction plusieurs fois : [manuel de GNU Assembler](https://ftp.gnu.org/old-gnu/Manuals/gas-2.9.1/html_chapter/as_7.html) (en anglais). Elle sert à générer un bloc d'instructions `nop` sans les écrire une par une.

Il faut faire ceci :
* Faire en sorte que la première instruction du code soit un `jmp`.
* Faire de ce `jmp` un saut relatif de exactement 0x51 octets à partir de l'instruction courante.
* Remplir l'espace entre le saut et sa destination avec des instructions `nop`, à l'aide de `.rept`.
* À l'étiquette où le saut relatif atterrit, placer 0x1 dans `rax`.

Quand votre code s'exécute, le processeur effectue le saut, passe par-dessus toutes les instructions `nop`, et continue à l'instruction qui place 0x1 dans `rax`. Cela montre comment contrôler le flot d'exécution avec des sauts relatifs.
