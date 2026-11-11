Maintenant que le serveur peut établir des connexions, il faut apprendre à envoyer des données.
Dans ce défi, le but est d'envoyer une réponse HTTP fixe (`HTTP/1.0 200 OK\r\n\r\n`) à tout client qui se connecte.
On utilise l'appel système [write](https://man7.org/linux/man-pages/man2/write.2.html), qui demande un descripteur de fichier, un pointeur vers un tampon de données et le nombre d'octets à écrire.
Cet exercice est important, car il apprend à formater des données et à les transmettre sur le réseau.
