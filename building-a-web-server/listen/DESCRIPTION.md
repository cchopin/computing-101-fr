Une fois le socket lié à une adresse, il faut le préparer à accepter les connexions entrantes.
L'appel système [listen](https://man7.org/linux/man-pages/man2/listen.2.html) transforme le socket en socket passif, qui attend les demandes de connexion des clients.
Il prend le descripteur du socket et un paramètre backlog, qui fixe le nombre maximal de connexions mises en attente.
Cette étape est essentielle : sans elle, le serveur ne pourrait recevoir aucune tentative de connexion.
