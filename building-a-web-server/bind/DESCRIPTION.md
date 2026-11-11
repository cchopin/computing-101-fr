Après la création d'un socket, l'étape suivante consiste à lui donner une identité réseau.
Dans ce défi, on utilise l'appel système [bind](https://man7.org/linux/man-pages/man2/bind.2.html) pour rattacher le socket à une adresse IP et à un numéro de port précis.
L'appel demande le descripteur du socket, un pointeur vers une `struct sockaddr` (en pratique une `struct sockaddr_in` pour IPv4, qui contient la famille d'adresses, le port et l'adresse IP) et la taille de cette structure.
Le rattachement est indispensable : il garantit que le serveur écoute sur une adresse connue, donc accessible aux clients.
