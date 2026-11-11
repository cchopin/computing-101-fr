Ce défi étend encore les capacités du serveur en gérant les requêtes HTTP POST de façon concurrente.
Les requêtes POST sont plus complexes : elles contiennent à la fois des en-têtes et un corps de message.
On utilise de nouveau [fork](https://man7.org/linux/man-pages/man2/fork.2.html) pour gérer plusieurs connexions, et [read](https://man7.org/linux/man-pages/man2/read.2.html) pour recevoir la requête entière.
Là encore, on lit le chemin de l'URL pour déterminer le fichier concerné, mais au lieu de lire ce fichier, on y écrit les données reçues dans la requête POST.
Pour cela, il faut connaître la longueur des données reçues.
La méthode évidente consiste à lire l'en-tête `Content-Length`, qui indique exactement cette longueur.
Une autre possibilité consiste à utiliser la valeur de retour de [read](https://man7.org/linux/man-pages/man2/read.2.html) pour connaître la longueur totale de la requête, puis à repérer la fin des en-têtes (qui se terminent par `\r\n\r\n`) et à en déduire la longueur du corps. Cette méthode, qui semble plus compliquée, peut être plus simple à implémenter.
Enfin, le serveur renvoie simplement une réponse `200 OK` au client, pour indiquer que la requête POST a réussi.
