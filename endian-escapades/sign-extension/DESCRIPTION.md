Vous avez vu que `byte`, `word`, `dword` et `qword` indiquent combien d'octets une instruction lit ou écrit.
Ajoutons une subtilité : une petite valeur peut être recopiée dans un registre plus grand, soit comme non signée, soit comme signée.

Si l'octet est non signé, remplir les octets de poids fort avec des zéros convient : `0x7f` devient `0x000000000000007f`.
Mais un octet signé utilise le complément à deux.
L'octet `0xff` vaut `-1`. L'étendre sur 64 bits doit donc remplir les nouveaux bits de poids fort avec des `1` : `0xffffffffffffffff`.

C'est ce qu'on appelle l'**extension de signe**.
Elle recopie le bit de signe dans les nouveaux bits de poids fort, au lieu de les mettre à zéro.
Sur x86-64, la forme à utiliser ici est :

```asm
movsx rax, BYTE PTR [rdi]
```

Cette instruction lit un octet à l'adresse contenue dans `rdi`, le traite comme un nombre signé, et place la valeur signée sur 64 bits dans `rax`.

Écrivez une fonction nommée `solve` qui reçoit dans `rdi` un pointeur vers un octet.
Chargez cet octet comme une valeur signée sur 8 bits, étendez-la sur 64 bits, renvoyez-la dans `rax`, et exportez-la avec `.global solve`.

Construisez-la en bibliothèque partagée, puis transmettez-la au correcteur :

```console
hacker@dojo:~$ as -o solve.o solve.s
hacker@dojo:~$ ld -shared -o solve.so solve.o
hacker@dojo:~$ /challenge/check solve.so
```
