Ce niveau porte sur les registres : il faut modifier leur contenu ou le lire.

Les valeurs sont placées dynamiquement en mémoire avant chaque exécution, donc elles changent d'une fois à l'autre. Il faut effectuer un calcul sur les registres. Le niveau indique quels registres sont préremplis et où placer le résultat, en général dans `rax`.

Autre fonctionnalité intéressante de x86 : on peut accéder séparément aux octets de poids faible de chaque registre.

Chaque registre de x86_64 fait 64 bits. Dans les niveaux précédents, on a utilisé le registre complet, comme `rax`, `rdi` ou `rsi`.

On peut aussi accéder aux octets de poids faible de chaque registre, avec d'autres noms.

Par exemple, les 32 bits de poids faible de `rax` sont accessibles avec `eax`, les 16 bits avec `ax`, et les 8 bits avec `al`.

```
MSB                                    LSB
+----------------------------------------+
|                   rax                  |
+--------------------+-------------------+
                     |        eax        |
                     +---------+---------+
                               |   ax    |
                               +----+----+
                               | ah | al |
                               +----+----+
```

Cet accès aux parties basses des registres est possible pour presque tous les registres.

Avec une seule instruction de déplacement, il faut placer `0x42` dans les 8 bits de poids fort de `ax`.
