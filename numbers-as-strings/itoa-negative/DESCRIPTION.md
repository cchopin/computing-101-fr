Votre `itoa` gère les nombres positifs ou nuls.
Mais une somme peut être négative (votre `atoi` lit bien les nombres négatifs), et un nombre négatif s'écrit avec un `-` en tête.

L'astuce consiste à retirer d'abord le signe, puis à laisser faire le travail déjà réalisé pour le reste :

- Si la valeur d'entrée est négative, écrivez un `'-'`, avancez le pointeur du tampon d'un caractère (par exemple `add rsi, 1`), et appliquez `neg` à la valeur pour obtenir sa grandeur.
- Vous pouvez comparer avec `cmp rdi, 0`, puis utiliser `jl is_negative` (`jl` saute si la valeur de gauche de la comparaison précédente est **l**ess que celle de droite, en signé).
- Exécutez votre boucle de chiffres existante sur cette grandeur (désormais positive ou nulle).
- La longueur totale est le nombre de chiffres écrits, plus `1` pour le signe.

Un nombre positif ou nul n'a pas de signe : il s'affiche donc exactement comme précédemment.

Étendez `itoa(value, buf)` pour gérer aussi les valeurs négatives.
La convention d'appel reste la même : la valeur dans `rdi`, le tampon dans `rsi`, et la longueur totale renvoyée dans `rax`.
Pensez à `.global itoa`.

Construisez et soumettez comme précédemment :

```console
hacker@dojo:~$ as -o your-solve.o your-solve.s
hacker@dojo:~$ ld -shared -o your-solve.so your-solve.o
hacker@dojo:~$ /challenge/check your-solve.so
```

Gérez le signe et renvoyez la longueur.
