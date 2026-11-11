Pour que le serveur puisse gérer plusieurs clients à la fois, on introduit la concurrence avec l'appel système [fork](https://man7.org/linux/man-pages/man2/fork.2.html).
Lorsqu'un client se connecte, [fork](https://man7.org/linux/man-pages/man2/fork.2.html) crée un processus enfant dédié à cette connexion.
Pendant ce temps, le processus parent retourne immédiatement accepter d'autres connexions.
Dans ce modèle, l'enfant utilise [read](https://man7.org/linux/man-pages/man2/read.2.html) et [write](https://man7.org/linux/man-pages/man2/write.2.html) pour dialoguer avec le client, tandis que le parent continue d'écouter.
Ce modèle concurrent est une notion clé pour construire des serveurs capables de passer à l'échelle.
