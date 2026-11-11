Dans le niveau précédent, on savait que l'entrée faisait exactement 128 octets : on pouvait donc `read` 128 et `write` 128.
Les données réelles sont rarement aussi bien ordonnées : souvent, on ne sait pas à l'avance combien d'octets vont arriver.

Heureusement, `read` le signale.
Quand un appel système se termine, Linux place son résultat dans `rax`.
Pour `read`, ce résultat est *le nombre d'octets réellement lus*.
Si l'on demande de lire 128 octets alors que seuls 50 sont disponibles, `read` en lit 50 et place `50` dans `rax`.

La méthode consiste donc à faire un `read` dans son tampon avec un nombre nettement supérieur à celui attendu, puis à `write` exactement le nombre d'octets renvoyé par `read`.
Il manque seulement à transmettre la valeur de retour de `read` (`rax`) au paramètre de taille de `write` (`rdx`) :

```asm
mov rdx, rax
```

Il faut effectuer cette instruction avant d'écraser `rax` avec le numéro d'appel système de `write` !

Cette fois, le flag est transmis sans remplissage.
Il faut le `read`, `write` exactement ce qui a été lu, puis `exit` avec le code `42` pour obtenir le flag !
