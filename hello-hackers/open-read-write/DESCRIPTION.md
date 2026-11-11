Jusqu'ici, le programme n'a interagi qu'avec l'entrée et la sortie standard. Et s'il fallait lire un fichier sur le disque ?
Pour accéder à un fichier, il faut d'abord l'*ouvrir* avec l'appel système `open`.

L'appel système `open` (numéro `2`) prend un pointeur vers une chaîne contenant le nom de fichier, et renvoie un tout nouveau descripteur de fichier qui désigne ce fichier :

```c
open("/flag", 0);
```

Le second argument précise des modes et des droits supplémentaires pour le fichier, mais `0` demande la valeur par défaut : lecture seule.

Les registres de `open` suivent la même convention :

| Registre | Rôle |
|----------|------|
| `rax`    | `2` (numéro d'appel système de `open`) |
| `rdi`    | pointeur vers la chaîne du nom de fichier en mémoire |
| `rsi`    | `0` (lecture seule) |

Quand `open` se termine, `rax` contient le numéro du nouveau descripteur de fichier (fd).
Rappelons que le descripteur 0 est l'entrée standard, le descripteur 1 la sortie standard et le descripteur 2 l'erreur standard.
Les autres fichiers ouverts reçoivent des descripteurs à partir de 3 !
On utilise ce fd comme premier argument de `read`, comme on l'a fait pour l'entrée standard, sauf que `read` lit cette fois dans le fichier.

Comment charger le nom de fichier en mémoire ?
Dans ce défi, le chemin vers le flag (`/flag`) est transmis comme premier argument du programme.
On sait déjà le charger : `mov rdi, [rsp+16]`.

Le programme doit :

1. charger dans `rdi` un pointeur vers le nom de fichier (stocké en `[rsp+16]`, le premier argument) ;
2. placer `0` dans `rsi`, pour l'accès en lecture seule par défaut ;
3. `open` le fichier (appel système `2`) ;
4. `read` depuis le fd renvoyé vers la mémoire. Le fd renvoyé par `open` se trouve dans `rax` ; il faut le placer dans `rdi` pour le premier argument de `read` (à faire *avant* de placer le numéro d'appel système de `write`). Lire un nombre d'octets nettement suffisant, le flag est court ;
5. `write` vers la sortie standard exactement le nombre d'octets renvoyé par `read` (`mov rdx, rax`, comme dans le défi précédent) ;
6. `exit` avec le code `42` (appel système `60`).

----
**DÉBOGAGE :**
En cas de problème ?
Utiliser `strace` pour voir les appels système en action : il montre exactement les arguments reçus par chaque appel et ce qu'il renvoie.
Si `open` renvoie `-1`, vérifier le pointeur vers le nom de fichier.
Si `read` renvoie `0`, le descripteur renvoyé par `open` est peut-être incorrect.
