Dans ce dernier défi, le serveur doit gérer à la fois les requêtes GET et POST dans un seul programme.
Après avoir lu la requête avec [read](https://man7.org/linux/man-pages/man2/read.2.html), le serveur examine les premiers caractères pour savoir s'il s'agit d'une requête GET ou POST.
Selon le type de requête, il traite les données en conséquence, puis renvoie une réponse adaptée avec [write](https://man7.org/linux/man-pages/man2/write.2.html).
Tout au long du processus, [fork](https://man7.org/linux/man-pages/man2/fork.2.html) sert à traiter chaque connexion en parallèle, pour que le serveur puisse gérer plusieurs requêtes en même temps.
Une fois ce défi terminé, on dispose d'un serveur web simple mais fonctionnel, capable de traiter différents types de requêtes HTTP.
