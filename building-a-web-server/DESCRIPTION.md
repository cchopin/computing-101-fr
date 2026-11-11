Maintenant qu'on sait écrire et déboguer de l'assembleur, passons à quelque chose de concret !
Dans ce module, on acquiert les compétences nécessaires pour construire un serveur web à partir de zéro : on part d'un programme simple, puis on gère des requêtes HTTP GET et POST multiples.
Bon courage !

----

Au fil du module, garder sous la main la [table des appels système](https://x64.syscall.sh/).

---

## Fiche de cours (ajout, hors traduction)

*Cette fiche n'est pas une traduction. Elle est rédigée à partir des cinq présentations de Connor Nelson (Arizona State University), en anglais : « Introduction », « Linux Processes », « Network System Calls », « HTTP » et « Multiprocessing ». Les liens vers les vidéos et les diapositives figurent dans les ressources du module. Elle ne contient aucune solution de défi.*

### 1. Ce que doit faire le serveur

Un serveur web est un programme qui attend des connexions réseau, lit une requête envoyée par un client, puis renvoie une réponse. Pour cela, il ne fait que combiner des appels système : demander au système d'exploitation de créer un point de communication, de l'attacher à une adresse, d'accepter des connexions, de lire et d'écrire des données, et de fermer les connexions.

Chaque défi ajoute une étape à cette chaîne. Les défis reprennent les notions vues précédemment : registres, arguments des appels système et mémoire.

### 2. Les processus Linux

Un programme en cours d'exécution est un *processus*. Le système lui associe plusieurs informations :

- un **identifiant de processus** (PID), et celui de son processus parent (PPID) ;
- des **identifiants d'utilisateur** (réel, effectif et sauvegardé), qui déterminent les droits du processus ;
- une **table de descripteurs de fichiers** : les numéros 0, 1 et 2 désignent l'entrée standard, la sortie standard et la sortie d'erreur, et les suivants désignent les fichiers et sockets ouverts ;
- une **carte mémoire** : zones du code, des données, des bibliothèques partagées et de la pile.

Un *descripteur de fichier* est un petit entier non négatif. Quand un appel comme `open` réussit, il renvoie le plus petit numéro libre dans la table. Les appels suivants utilisent ce numéro pour désigner le fichier.

### 3. Lire et écrire avec les appels système

Quatre appels suffisent pour la plupart des défis de ce module :

- `read` lit jusqu'à un nombre donné d'octets depuis un descripteur, dans un tampon en mémoire ;
- `write` écrit des octets depuis un tampon vers un descripteur ;
- `open` ouvre un fichier par son chemin et renvoie un descripteur ;
- `close` ferme un descripteur.

Les arguments sont placés dans les registres dans un ordre fixé, et le numéro de l'appel dans `rax`. La table des appels système, référencée dans le module, donne ces numéros pour chaque architecture. Il faut la consulter plutôt que de les mémoriser.

### 4. Ouvrir un port : socket, bind, listen, accept

Pour accepter une connexion réseau, on enchaîne quatre appels :

1. **`socket`** crée un point de communication et renvoie un descripteur. Ses arguments sont le domaine (par exemple IPv4, nommé `AF_INET`), le type (par exemple TCP, nommé `SOCK_STREAM`) et le protocole, en général `0`.
2. **`bind`** attache le socket à une adresse. Cette adresse est une structure `sockaddr_in` qui contient la famille, le port et l'adresse IPv4. Les nombres du port et de l'adresse sont écrits dans l'ordre réseau, c'est-à-dire en gros-boutisme (*big-endian*). Une fonction comme `htons` convertit un nombre de l'ordre de la machine vers l'ordre réseau.
3. **`listen`** fait passer le socket en mode passif, prêt à recevoir des connexions. Son paramètre `backlog` fixe le nombre de connexions en attente.
4. **`accept`** prend une connexion en attente et renvoie un **nouveau** descripteur, dédié à ce client. Le socket d'origine reste disponible pour les connexions suivantes.

Une fois la connexion acceptée, le serveur lit la requête et répond sur le nouveau descripteur, pas sur celui qui écoute.

### 5. HTTP : requête et réponse

HTTP est un protocole texte. Un client envoie une requête composée :

- d'une **ligne de requête** : méthode (`GET`, `POST`...), chemin de la ressource, version du protocole ;
- d'**en-têtes**, un par ligne, sous la forme `Nom: valeur` ;
- d'une **ligne vide**, marquée par `\r\n\r\n`, qui signale la fin des en-têtes ;
- pour une requête POST, d'un **corps** dont la longueur est indiquée par l'en-tête `Content-Length`.

La réponse suit le même principe : une **ligne de statut** (version, code, message, par exemple `200 OK`), des en-têtes, une ligne vide, puis le corps éventuel. Le serveur doit donc envoyer exactement ces éléments, dans l'ordre, pour que le client les comprenne.

Ce qui est lu depuis le socket est un flux d'octets : une seule lecture peut ne pas contenir toute la requête, et il faut donc savoir où se termine la requête pour la traiter.

### 6. Plusieurs clients : fork

L'appel `fork` duplique le processus appelant. L'appelant devient le *parent*, la copie devient l'*enfant*. Après `fork` :

- le parent reçoit le PID de l'enfant comme valeur de retour ;
- l'enfant reçoit `0` comme valeur de retour, ce qui permet de savoir dans quel processus on se trouve.

Les descripteurs sont copiés : parent et enfant partagent le même socket d'écoute et la même connexion. Chaque processus doit donc fermer les descripteurs dont il n'a pas besoin. Sinon, la connexion ne se ferme jamais côté client. C'est ce modèle, un processus enfant par connexion pendant que le parent continue d'écouter, qui permet de servir plusieurs clients à la fois.

### Pour aller plus loin

- Les pages de manuel Linux, avec la commande `man` (par exemple `man 2 accept`), donnent la référence complète de chaque appel système (en anglais).
- La spécification HTTP, publiée par l'IETF, décrit le format exact des requêtes et des réponses (en anglais).
