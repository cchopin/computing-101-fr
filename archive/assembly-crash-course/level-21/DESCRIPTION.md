Dans ce niveau, on travaille avec le contrôle du flot d'exécution. Il s'agit d'utiliser des instructions qui contrôlent, directement ou indirectement, le registre spécial `rip`, le pointeur d'instruction. On utilisera des instructions comme `jmp`, `call`, `cmp` et leurs variantes pour obtenir le comportement demandé.

Dans ce niveau, votre code sera testé plusieurs fois avec des valeurs dynamiques ! Il sera exécuté dans des conditions variées, pour vérifier que la logique tient dans des usages ordinaires.

Dans les niveaux précédents, on a découvert la boucle for, qui itère un *nombre* de fois connu avant ou pendant l'exécution. Mais que faire lorsqu'on veut itérer jusqu'à ce qu'une condition soit remplie ?

Une seconde structure de boucle existe pour ce besoin : la boucle while. Dans une boucle while, on itère jusqu'à ce qu'une condition soit remplie.

Par exemple, supposons une zone de mémoire contenant des nombres consécutifs, et que l'on veuille calculer la moyenne de tous les nombres jusqu'à en trouver un supérieur ou égal à `0xff` :

```plaintext
average = 0
i = 0
while x[i] < 0xff:
  average += x[i]
  i += 1
average /= i
```

En s'appuyant sur ce qui précède, il faut faire ceci :

Compter les octets non nuls consécutifs dans une zone contiguë de mémoire, où :
- `rdi` = adresse mémoire du 1er octet
- `rax` = nombre d'octets non nuls consécutifs

De plus, si `rdi = 0`, alors il faut définir `rax = 0` (ce point sera vérifié) !

Exemple de cas de test, avec :
- `rdi = 0x1000`
- `[0x1000] = 0x41`
- `[0x1001] = 0x42`
- `[0x1002] = 0x43`
- `[0x1003] = 0x00`

Alors `rax = 3` doit être défini.
