Dans le défi précédent, on a lu la valeur située à `[rsp]` : tout en haut de la pile.
Mais la pile contient beaucoup d'autres données, et on peut accéder à chacune d'elles en ajoutant un *décalage* (offset) à `rsp`.

Par exemple, `[rsp+8]` lit la valeur de 8 octets placée juste *après* `[rsp]`, `[rsp+16]` lit la suivante, et ainsi de suite.
En général, `[rsp+N]` lit la mémoire à l'adresse `rsp+N` :

```text
    Adresse    | Contenu
  +---------------------------+
  | rsp        | valeur 0     |  <-- [rsp]
  +---------------------------+
  | rsp+8      | valeur 1     |  <-- [rsp+8]
  +---------------------------+
  | rsp+16     | valeur 2     |  <-- [rsp+16]
  +---------------------------+
  | ...        | ...          |
  +---------------------------+
```

Ces décalages sont des multiples de 8.
C'est parce que beaucoup de valeurs placées sur la pile, comme les nombres ou les adresses mémoire, occupent 8 octets (64 bits), donc les valeurs consécutives sont séparées de 8 octets.
Mais il s'agit surtout d'une convention : en réalité, la pile est, comme toute zone de mémoire, une suite continue d'octets individuels. Pour l'instant, on la traite comme une suite de valeurs de 8 octets (64 bits).

Dans ce défi, une valeur secrète a été cachée sur la pile, à un décalage de 128 octets par rapport à `rsp`.
Lire la valeur située à `[rsp+128]` et l'utiliser comme code de retour !
