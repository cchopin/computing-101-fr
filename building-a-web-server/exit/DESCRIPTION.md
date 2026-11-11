Le premier travail consiste à écrire le programme le plus simple possible : il se termine dès qu'on le lance.
Dans ce défi, on utilise l'appel système [exit](https://man7.org/linux/man-pages/man2/_exit.2.html), qui met fin à un processus et renvoie un statut de sortie au système d'exploitation.
Cet appel prend un seul argument : le statut de sortie (`0` indique en général un succès).
Savoir quitter proprement un programme est essentiel, car cela permet au processus de signaler correctement qu'il a terminé.
