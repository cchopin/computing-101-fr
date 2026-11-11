Les données réelles sont désordonnées.
Un nombre inclus dans une chaîne plus longue n'est pas toujours suivi d'un NUL bien net : il peut être suivi d'un espace, d'une lettre, d'une virgule ou de tout autre caractère : `"42abc"`, `"100 200"`, `"7,"`.

Un `atoi` correct lit les chiffres jusqu'à rencontrer quelque chose qui *n'est pas* un chiffre, puis s'arrête, quel que soit ce caractère (y compris 0x00).
Au lieu de « s'arrêter sur l'octet 0 », la règle devient : « s'arrêter au premier octet qui n'est pas compris entre `'0'` et `'9'` ».

Un test rapide et sans ramification pour un caractère `c` : calculer `c - 0x30` (on retranche `0x30`), puis vérifier que le résultat est dans l'intervalle `0`-`9` à l'aide d'une comparaison *non signée*.
Tout ce qui n'est pas un chiffre (ponctuation, lettres, espace, et même l'octet 0 : soustraire `'0'` à 0 donne un nombre négatif en complément à deux, donc très grand lorsqu'on le lit comme non signé) tombe hors de cet intervalle.

Pour effectuer une comparaison non signée, utiliser l'instruction `ja`, qui signifie « **j**ump if **a**bove » (sauter si la dernière comparaison est supérieure, en non signé).
Il faut faire le `cmp` (voir les leçons précédentes du dojo), puis :

```
ja  done

...

done:
   ret
```

Pour le reste, conserver la solution du niveau précédent : un `'-'` au début signifie toujours un nombre négatif, le calcul reste le même, et on obtient le flag une fois la solution correcte !
