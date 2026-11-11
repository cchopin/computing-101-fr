Jusqu'ici, le serveur ne traitait qu'une seule requête GET avant de se terminer.
Il faut maintenant le modifier pour qu'il traite plusieurs requêtes GET les unes après les autres.
Cela consiste à placer la séquence accept, read, write et close dans une boucle.
À chaque connexion, le serveur accepte le client, traite sa requête GET, puis ferme proprement la session du client, tout en restant actif pour la requête suivante.
Cette approche itérative est indispensable pour construire un serveur persistant.
