Les ordinateurs reçoivent souvent leurs données sous forme de *texte*.
Quand vous passez `12345` à un programme comme argument de la ligne de commande, votre code ne reçoit pas le *nombre* 12345 : il reçoit cinq octets ASCII distincts, `'1'`, `'2'`, `'3'`, `'4'`, `'5'`.
Le processeur ne peut pas additionner ou multiplier directement ce texte : il faut d'abord transformer ces caractères en les nombres qu'ils représentent.
Cette tâche est traditionnellement confiée à une fonction nommée `atoi` (ASCII to integer, « ASCII vers entier ») : nous allons la construire pas à pas, en commençant ici par un seul chiffre.

L'idée clé tient à la façon dont les chiffres sont codés.
Vous avez déjà vu l'ASCII dans les niveaux précédents ; nous parlons ici des *nombres* en ASCII, c'est-à-dire de la représentation textuelle des valeurs numériques.
En ASCII, le caractère `'0'` est l'octet `0x30`, `'1'` vaut `0x31`, et ainsi de suite jusqu'à `'9'`, à `0x39`.
Les chiffres se suivent, donc la *valeur* d'un caractère chiffre est simplement ce caractère moins `'0'` :

```
'7'  ->  0x37 - 0x30  =  7
```

Dans ce niveau, vous devez écrire une fonction qui convertit une chaîne de texte contenant un seul chiffre en le nombre correspondant.
Votre fonction, qui doit s'appeler `atoi_digit`, reçoit dans `rdi` un pointeur vers un caractère chiffre, et doit renvoyer la valeur de ce chiffre (de `0` à `9`) dans `rax`.

Comme vous écrivez une fonction `atoi_digit` plutôt qu'une fonction `solve`, vous devez ajouter `.global atoi_digit` pour que le défi puisse la trouver !
Construisez ensuite une bibliothèque partagée et transmettez-la au correcteur :

```console
hacker@dojo:~$ as -o your-solve.o your-solve.s
hacker@dojo:~$ ld -shared -o your-solve.so your-solve.o
hacker@dojo:~$ /challenge/check your-solve.so
```

Décodez le chiffre, renvoyez sa valeur, et récupérez le flag.
