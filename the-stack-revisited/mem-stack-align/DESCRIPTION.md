Dans le niveau précédent, vous avez lu `envp[0]`, un pointeur placé sur la pile par le noyau, qui pointe *dans* la zone des chaînes située au-dessus des tableaux de pointeurs.
La même disposition s'applique ici :

```text
     Address    │ Contents
   +────────────────────────+
   │ rsp + 0    │ 1         │ ◀────   argc
   +────────────────────────+
   │ rsp + 8    │ rsp + 128 │──────┐  argv[0]: pointer to the program name
   +────────────────────────+      │  
   │ rsp + 16   │ 0         │      │  NULL (end of argv)
   +────────────────────────+      │  
   │ rsp + 24   │ rsp + 200 │───┐  │  envp[0]: pointer to the first env var
   +────────────────────────+   │  │ 
   │ rsp + 32   │ 0         │   │  │  NULL (end of envp)
   +────────────────────────+   │  │
                                │  │ 
  ┌─────────────────────────────┘  │
  │                                │
  │   Address   │ Contents         │
  │ +──────────────────────────+   │
  │ │ rsp + 128 │ "/tmp/..."   │◀──┘ the program name
  │ +──────────────────────────+
  │ │ ...       │ ...          │
  │ +──────────────────────────+
  └▸│ rsp + 200 │ "FOO=..."    │ ◀─ the first env var
    +──────────────────────────+
```

Mais d'où viennent les adresses réelles (`rsp`, ou l'adresse réelle vers laquelle pointe `rsp+200`, etc.) ?

Au lancement de votre programme, le noyau **remplit la pile à l'envers** à partir d'une adresse de départ choisie.
Il écrit d'abord les chaînes d'environnement (la zone « croît » vers les adresses plus petites), puis les chaînes d'arguments, d'autres métadonnées, puis les tableaux de pointeurs `envp[]` et `argv[]`, et enfin `argc` du côté gauche de la structure.
C'est là que `rsp` finit par pointer.

Cela a une conséquence intéressante : **plus vous ajoutez d'octets dans l'environnement (ou dans les arguments du programme), plus le noyau repousse le reste vers la gauche de la pile**.
Un octet d'environnement supplémentaire fait donc descendre `rsp` vers une adresse plus petite : la zone des chaînes d'arguments se décale d'un octet vers la gauche, et `argv[0]` (un pointeur dans cette zone) contient une valeur plus petite d'un octet.

Ici, `env -i` signifie « exécuter la commande suivante avec un environnement vide » : les couples `NOM=VALEUR` placés après `-i` sont les seules chaînes d'environnement reçues par le programme enfant.
Dans ce défi, commencez par une référence avec `env -i /challenge/program` pour voir à quelle adresse le programme attend `argv[0]`.
Relancez ensuite le programme avec exactement une variable d'environnement, dont la valeur contient le bon nombre de `x`, afin de décaler `argv[0]` jusqu'à cette adresse.
Utilisez `env -i` pour les deux exécutions, afin que les variables de votre shell n'arrivent pas elles aussi sur la pile et ne faussent pas votre comptage :

```text
hacker@dojo:~$ env -i /challenge/program
hacker@dojo:~$ env -i FOO=xxxxxxxx /challenge/program
```

N'oubliez pas que la chaîne complète de l'environnement est placée sur la pile : `FOO=`, la valeur et l'octet nul final comptent donc dans le décalage.
Vous ne modifiez pas le programme lui-même : vous changez seulement la façon dont il est lancé, ce qui influence l'emplacement de ses données !
