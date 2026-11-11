Le processeur raisonne en termes très simples. Il déplace des données, les modifie, prend des décisions en fonction de leur valeur et agit en conséquence. La plupart du temps, ces données sont stockées dans des *registres* (registers).

Un registre est simplement un conteneur de données. Le processeur peut y placer une donnée, copier une donnée d'un registre à un autre, et ainsi de suite. Au niveau matériel, les registres sont réalisés avec des circuits très coûteux, logés dans un espace microscopique et utilisés à une fréquence telle que des contraintes physiques, comme la vitesse de la lumière, influencent leurs performances. Le nombre de registres d'un processeur est donc très limité. Selon les architectures, leurs noms et leur nombre diffèrent, mais on trouve en général entre 10 et 20 registres « à usage général » (general purpose registers), que le code peut utiliser librement, auxquels s'ajoutent quelques dizaines de registres à usage particulier.

Dans sa version moderne, x86\_64, l'architecture x86 offre 16 registres à usage général. Ce défi présente le premier d'entre eux : `rax`.
Bonjour, Rax !

`rax` n'est qu'une petite pièce de la conception très complexe du processeur x86, mais c'est par là qu'on commence.
Comme les autres registres, `rax` contient une petite quantité de données. On y place une valeur avec l'instruction `mov`.
Une instruction se compose d'un *opérateur* (ici `mov`) et d'*opérandes*, qui sont les données sur lesquelles elle agit (ici le registre de destination, et la valeur à y stocker).

Par exemple, pour stocker la valeur `1337` dans `rax`, l'assembleur x86 s'écrit :

```assembly
mov rax, 1337
```

On remarque plusieurs choses :

1. La destination (`rax`) est indiquée *avant* la source (la valeur `1337`).
2. Les opérandes sont séparés par une virgule.
3. C'est *vraiment* simple !

Dans ce défi, on écrit son premier code assembleur. Il faut placer la valeur `60` dans `rax`.
Le programme est enregistré dans un fichier d'extension `.s` (par exemple `rax-challenge.s`). Cette extension n'est pas obligatoire, mais c'est la convention pour les fichiers d'assembleur. Passer ensuite ce fichier `.s` au programme de vérification en argument :

```console
hacker@dojo:~$ /challenge/check rax-challenge.s
```

Le fichier `.s` est l'entrée du programme de vérification : ne pas essayer de l'exécuter directement.
Le code peut être écrit dans n'importe quel éditeur de texte, y compris celui de l'espace de travail VSCode de pwn.college.

---
**Erratum :**
Si l'on a déjà vu de l'assembleur x86, on a peut-être rencontré un dialecte un peu différent.
Le dialecte utilisé dans pwn.college est la « syntaxe Intel » (Intel Syntax), la bonne façon d'écrire l'assembleur x86, puisque c'est Intel qui a créé x86.
Certains cours enseignent la syntaxe « AT&T », ce qui sème une confusion considérable.
Nous y reviendrons brièvement dans le prochain module, et on espère ne plus jamais avoir à y penser.
