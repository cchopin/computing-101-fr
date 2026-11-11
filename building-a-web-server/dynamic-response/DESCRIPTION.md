Dans ce défi, le serveur gère un contenu dynamique, en fonction des requêtes HTTP GET.
On utilise d'abord l'appel système [read](https://man7.org/linux/man-pages/man2/read.2.html) pour recevoir la requête HTTP envoyée par le client sur le socket.
En examinant la ligne de requête, et en particulier le chemin de l'URL, on sait ce que demande le client.
Ensuite, on utilise l'appel système [open](https://man7.org/linux/man-pages/man2/open.2.html) pour ouvrir le fichier demandé, puis [read](https://man7.org/linux/man-pages/man2/read.2.html) pour lire son contenu.
On renvoie le contenu du fichier au client avec l'appel système [write](https://man7.org/linux/man-pages/man2/write.2.html).
C'est une étape importante vers l'interactivité : le serveur adapte sa réponse au lieu de renvoyer toujours le même message.
