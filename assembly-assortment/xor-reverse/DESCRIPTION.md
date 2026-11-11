Ce défi introduit un nouveau type d'opération : le **XOR bit à bit**.
Contrairement à `add` et `sub`, qui font de l'arithmétique, `xor` agit sur chaque bit individuellement.

L'instruction `xor` calcule le *ou exclusif* de deux valeurs : pour chaque position de bit, le résultat vaut `1` si exactement un des deux bits d'entrée vaut `1`, et `0` sinon.
Par exemple :

```
  01100001  (0x61, 'a')
^ 00101010  (0x2a, 42)
---------
  01001011  (0x4b, 75)
```

Dans les schémas et les expressions comme `0x4b ^ 0x2a`, `^` désigne le XOR ; en assembleur, l'instruction s'écrit `xor`.
La syntaxe est la même que pour `add` et `sub` : `xor rax, 42`.

Une propriété clé du XOR est qu'il est **son propre inverse** : appliquer `xor` deux fois avec la même valeur redonne la valeur d'origine.
Si vous voyez :

```
xor    BYTE PTR [rax],0x2a
cmp    BYTE PTR [rax],0x4b
```

Le programme applique un XOR entre l'octet de votre saisie et `0x2a`, puis vérifie que le résultat vaut `0x4b`.
Pour inverser l'opération, appliquez à la cible le même XOR avec la clé : `0x4b ^ 0x2a = 0x61`, soit `'a'`.

À vous : désassemblez le binaire, inversez le XOR, et obtenez le flag !
