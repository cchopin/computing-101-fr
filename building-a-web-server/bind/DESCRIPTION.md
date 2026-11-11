Après la création d'un socket, l'étape suivante consiste à lui donner une identité réseau.
Dans ce défi, on utilise l'appel système [bind](https://man7.org/linux/man-pages/man2/bind.2.html) pour rattacher le socket à une adresse IP et à un numéro de port précis.
L'appel demande le descripteur du socket, un pointeur vers une `struct sockaddr`, et la taille de cette structure.
Dans [Endian Escapades](/computing-101/endian-escapades), on a vu que x86 range les valeurs sur plusieurs octets en petit-boutisme en mémoire, et que les contextes hors processeur, comme les protocoles réseau, peuvent utiliser un autre ordre des octets.
Pour les structures de socket IP, les nombres sur plusieurs octets utilisent le gros-boutisme : l'octet de poids fort vient en premier.
Cette convention s'appelle **ordre des octets du réseau** (network byte order).
Pour un numéro de port sur 16 bits, le port `80` (`0x0050`) est donc représenté par les octets `00 50`.
En IPv4, la structure est une `struct sockaddr_in`, et `bind` lit les 16 octets situés à ce pointeur comme une suite de champs :

```text
bytes 0..1    address family, `AF_INET` (`2`)
bytes 2..3    port, in network byte order (`00 50` for port 80)
bytes 4..7    address (`0.0.0.0` is four zero bytes)
bytes 8..15   padding
```

Dans [Ouvrir le flag, avec rip](/computing-101/hello-hackers/open-read-write-rip-relative), on a utilisé des octets stockés et passé leur adresse à un appel système.
Ici, il faut construire les octets de `sockaddr_in` sur la pile, passer l'adresse de la pile comme pointeur, et passer `16` comme taille.
Sur x86 64 bits, écrire la valeur `0x5000` stocke les octets `00 50`, qui correspondent au port `80` dans l'ordre des octets du réseau.
Le rattachement est indispensable : il garantit que le serveur écoute sur une adresse connue, donc accessible aux clients.
