Dans ce niveau, on travaille avec le contrôle du flot d'exécution. Il s'agit d'utiliser des instructions qui contrôlent, directement ou indirectement, le registre spécial `rip`, le pointeur d'instruction. On utilisera des instructions comme `jmp`, `call`, `cmp` et leurs variantes pour obtenir le comportement demandé.

Dans ce niveau, votre code sera testé plusieurs fois avec des valeurs dynamiques ! Il sera exécuté dans des conditions variées, pour vérifier que la logique tient dans des usages ordinaires.

Le dernier type de saut est le saut indirect, souvent utilisé dans la vraie vie pour implémenter les instructions switch. Un switch est un cas particulier de si-sinon, qui ne décide qu'à partir de nombres pour choisir où le flot va aller.

Voici un exemple :

```
switch(number):
  0: jmp do_thing_0
  1: jmp do_thing_1
  2: jmp do_thing_2
  default: jmp do_default_thing
```

Le switch de cet exemple porte sur `number`, qui peut valoir 0, 1 ou 2. Si `number` n'est aucun de ces nombres, c'est le cas default qui s'exécute. On peut y voir une forme réduite de si-sinon. En x86, on manipule déjà des nombres, il n'est donc pas surprenant de pouvoir écrire des conditions sur une valeur exacte. Si l'on connaît l'intervalle des nombres, un switch se prête très bien à cela.

Prenons par exemple une table de sauts. Une table de sauts est une zone contiguë de mémoire qui contient des adresses vers lesquelles sauter.

Dans l'exemple ci-dessus, la table de sauts pourrait ressembler à ceci :

```
[0x1337] = address of do_thing_0
[0x1337+0x8] = address of do_thing_1
[0x1337+0x10] = address of do_thing_2
[0x1337+0x18] = address of do_default_thing
```

Avec cette table, on réduit fortement le nombre de `cmps`. Il suffit maintenant de vérifier si `number` est supérieur à 2. Si c'est le cas, on exécute toujours :

```
jmp [0x1337+0x18]
```

Sinon :

```
jmp [jump_table_address + number * 8]
```

En s'appuyant sur ce qui précède, il faut implémenter la logique suivante :
```plaintext
if rdi is 0:
  jmp 0x40301e
else if rdi is 1:
  jmp 0x4030da
else if rdi is 2:
  jmp 0x4031d5
else if rdi is 3:
  jmp 0x403268
else:
  jmp 0x40332c
```

Il faut respecter les contraintes suivantes :

- On suppose que `rdi` n'est JAMAIS négatif.
- Utiliser au plus 1 instruction `cmp`.
- Utiliser au plus 3 sauts (de n'importe quel type).
- Le nombre sur lequel effectuer le switch est fourni dans `rdi`.
- L'adresse de base de la table de sauts est fournie dans `rsi`.

Voici un exemple de table :

```
[0x40427c] = 0x40301e (addrs will change)
[0x404284] = 0x4030da
[0x40428c] = 0x4031d5
[0x404294] = 0x403268
[0x40429c] = 0x40332c
```
