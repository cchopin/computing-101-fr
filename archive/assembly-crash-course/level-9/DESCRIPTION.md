Ce niveau porte sur les registres : il faut modifier leur contenu ou le lire.

Les valeurs sont placées dynamiquement en mémoire avant chaque exécution, donc elles changent d'une fois à l'autre. Il faut effectuer un calcul sur les registres. Le niveau indique quels registres sont préremplis et où placer le résultat. Dans la plupart des cas, c'est `rax`.

Ce niveau porte sur les opérations au niveau des bits. Il faut manipuler directement les bits d'un registre ou d'un emplacement mémoire. On aura probablement besoin des instructions logiques de x86 : `and`, `or`, `xor`.

En utilisant uniquement les instructions suivantes :
- `and`
- `or`
- `xor`

Il faut implémenter la logique suivante :

```plaintext
if x is even then
  y = 1
else
  y = 0
```

Où :
- `x = rdi`
- `y = rax`
