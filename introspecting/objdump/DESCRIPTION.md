Dans le module précédent, vous avez écrit des programmes en assembleur, puis vous les avez construits en exécutables.
Mais que faire quand quelqu'un vous donne un programme et que vous voulez comprendre ce qu'il fait ?
C'est là qu'intervient le **désassemblage** : il consiste à convertir le code machine binaire d'un exécutable *en retour* en instructions d'assembleur lisibles.

Il existe des outils bien plus puissants, que vous découvrirez plus tard, mais on commence avec l'un des outils de désassemblage les plus courants : `objdump`.
Pour un fichier binaire, `objdump -d` désassemble les sections exécutables et affiche les instructions d'assembleur :

```console
hacker@dojo:~$ objdump -d -M intel /tmp/your-program

/tmp/your-program:     file format elf64-x86-64


Disassembly of section .text:

0000000000401000 <_start>:
  401000:	48 c7 c7 39 05 00 00 	mov    rdi,0x539
  401007:	48 c7 c7 00 00 00 00 	mov    rdi,0
  40100e:	48 c7 c0 3c 00 00 00 	mov    rax,0x3c
  401015:	0f 05                	syscall
```

Quelques points sont à noter.
D'abord, par défaut, `objdump` utilise la mauvaise syntaxe d'assembleur, c'est pourquoi on passe l'option `-M intel`.
N'oubliez pas cette option !
Lire de l'assembleur dans une autre syntaxe que la syntaxe Intel peut prêter à confusion, et nuire à votre santé.

Ensuite, `objdump` affiche les octets bruts de chaque instruction (par exemple, les valeurs hexadécimales `0f 05` correspondent à l'instruction `syscall`), à côté de l'assembleur lisible.
Ce sont les valeurs réellement stockées dans la mémoire de l'ordinateur pour représenter les instructions.
Pour des raisons mathématiques, elles sont écrites en base 16 (hexadécimal), et non en base 10 (décimal), celle que nous utilisons pour compter.
Si ce point n'est pas clair, parcourez la première moitié environ du module [Traiter les données](https://pwn.college/fundamentals/data-dealings), puis revenez ici !

Enfin, les valeurs placées dans les registres sont *aussi* écrites en hexadécimal.
Cela peut rendre le programme difficile à comprendre.
Ici, on voit que le programme place la valeur `0x3c` dans `rax`, ce qui vaut `60` en décimal : c'est le numéro de l'appel système `exit` !
Juste avant, il place `0` dans `rdi`, qui sera le code de retour du programme.

Plus intéressant encore, juste avant *cela*, il place `0x539` dans `rdi`. Cette valeur n'est pas visible de l'extérieur, car `rdi` est aussitôt remis à `0`.
Ce « secret » est anodin, mais en lisant le code d'un logiciel, on peut en extraire beaucoup d'autres, dont certains sont sensibles pour la sécurité.

Ce défi entraîne à cette extraction de secrets, avec un binaire situé dans `/challenge/disassemble-me`.
Utilisez `objdump` pour le désassembler et trouvez le nombre chargé dans `rdi` avant d'être effacé.
Soumettez ensuite ce nombre avec `/challenge/submit-number`.
Le nombre est affiché en hexadécimal dans le désassemblage, mais `/challenge/submit-number` accepte aussi bien l'hexadécimal (par exemple `0x539`) que le décimal (par exemple `1337`).
Bon courage !
