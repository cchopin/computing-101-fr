Une fois le socket à l'écoute, il est temps d'accepter activement les connexions entrantes.
Dans ce défi, on utilise l'appel système [accept](https://man7.org/linux/man-pages/man2/accept.2.html), qui attend qu'un client se connecte.
Lorsqu'une connexion est établie, il renvoie un nouveau descripteur de socket, dédié à la communication avec ce client, et remplit une structure d'adresse (comme une `struct sockaddr_in`) avec les informations du client.
Cette étape transforme le serveur, d'un simple écouteur passif, en un interlocuteur actif.
