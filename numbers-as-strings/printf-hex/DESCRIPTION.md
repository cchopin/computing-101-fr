`\n` désigne un octet ASCII nommé : le saut de ligne, `0x0a`.
Pour des octets quelconques, utilisez un échappement hexadécimal : `\xNN`.

Vous connaissez déjà l'hexadécimal : deux chiffres hexadécimaux décrivent un octet.
Ici, `\x` ouvre l'échappement, et les deux chiffres hexadécimaux suivants donnent la valeur de l'octet à écrire.

```
format bytes:  "\x41"
hex value:      0x41
output byte (ascii):    "A"
```

Attention à l'étape de conversion : la chaîne de format contient des caractères ASCII, et non encore des valeurs hexadécimales numériques.
Pour `\x4a`, votre programme voit quatre octets d'entrée : la barre oblique inverse, `x`, `4` et `a`.
Une fois `\x` reconnu, la conversion utilise les octets ASCII de `4` et de `a`.
Convertissez d'abord chaque caractère hexadécimal ASCII en un nombre de 4 bits, appelé quartet (nibble).
Pour `0` à `9`, retranchez la valeur ASCII de `0` pour obtenir `0` à `9`.
Pour `a` à `f`, retranchez la valeur ASCII de `a` et ajoutez `10` ; pour `A` à `F`, retranchez la valeur ASCII de `A` et ajoutez `10`.
Placez ensuite le premier quartet dans la moitié haute de l'octet (avec un décalage à gauche, que vous avez vu plus tôt !) et le second dans la moitié basse : `(first << 4) | second`.

```
format text:      \ x 4 a
hex digits:           4 10
combined byte:        (4 << 4) | 10 = 0x4a
output byte:          "J"
```

Quand votre parcours rencontre `\xNN`, convertissez les deux chiffres hexadécimaux en un octet, puis écrivez cet octet avec `write`.
Continuez à gérer le texte ordinaire, `%d`, `%s`, `\n`, `\\` et `%%`.

Construisez et soumettez comme précédemment :

```console
hacker@dojo:~$ as -o prog.o prog.s
hacker@dojo:~$ ld -o prog prog.o
hacker@dojo:~$ ./prog 'byte=\x2a'
byte=*
hacker@dojo:~$ /challenge/check prog
```

Décodez l'échappement d'octet hexadécimal et écrivez-le.
