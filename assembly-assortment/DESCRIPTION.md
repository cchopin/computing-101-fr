On a déjà une connaissance pratique de l'assembleur grâce au parcours suivi jusqu'ici.
Élargissons-la !

Ce module explore les effets de plusieurs instructions d'assembleur différentes, pour apprendre à les reconnaître et à ne pas paniquer en les voyant.
Dans la plupart des défis, on rétro-ingénie un binaire pour comprendre l'effet d'une instruction sur des données, puis on lui fournit une saisie qu'il acceptera.
Dans quelques autres, on utilise soi-même une instruction, en écrivant une petite fonction autour d'elle.
Dans les deux cas, si l'on comprend ce que fait l'instruction, on obtient les flags !

---

## Fiche de cours (ajout, hors traduction)

*Cette fiche n'est pas une traduction. Elle reprend les notions de la présentation « Data » de Yan Shoshitaishvili (Arizona State University), en anglais, dont le lien figure dans les ressources du module. Elle complète cette présentation avec des explications générales, et ne contient aucune solution de défi.*

### 1. Rappels : bits, chiffres et octets

Un bit vaut `0` ou `1`. Un octet regroupe huit bits, et se note souvent en hexadécimal sur deux chiffres (de `0x00` à `0xff`). Un chiffre hexadécimal correspond à quatre bits : `0xF` vaut `1111` en binaire.

Pour convertir, on additionne les puissances de deux correspondant aux bits à `1`. Par exemple, `1011 0010` vaut `128 + 32 + 16 + 2 = 178`.

### 2. Les caractères ASCII et le bit de casse

Une lettre ASCII est un octet dont la valeur dépend de sa place dans l'alphabet :

- majuscules : `0x40` + position dans l'alphabet (`A` vaut `0x41`) ;
- minuscules : `0x60` + position dans l'alphabet (`a` vaut `0x61`).

La différence entre une majuscule et une minuscule tient donc à un seul bit : le bit `0x20`. Ce bit de casse peut être activé (`or`), effacé (`and`) ou inversé (`xor`). Il ne désigne une casse que pour les lettres : sur un chiffre ou un espace, il change le caractère sans en faire une lettre de casse différente.

### 3. Les opérations bit à bit

Les opérations bit à bit s'appliquent à chaque position de bit indépendamment des autres :

| Opération | Résultat à `1` quand | Usage typique |
|---|---|---|
| `and` | les deux bits valent `1` | masquer, c'est-à-dire ne garder que certains bits |
| `or` | au moins un bit vaut `1` | activer des bits |
| `xor` | les deux bits diffèrent | inverser des bits, ou comparer |

`xor` est son propre inverse : appliqué deux fois avec la même valeur, il redonne la valeur d'origine. C'est pour cette raison qu'il sert à chiffrer et déchiffrer des données avec une même clé.

### 4. Les décalages

Un décalage déplace les bits d'une valeur, et laisse des zéros sur le bord opposé :

- `shl` décale vers la gauche : chaque décalage de un position multiplie une valeur non signée par 2 ;
- `shr` décale vers la droite avec remplissage par des zéros : chaque décalage de une position divise une valeur non signée par 2, en supprimant le reste.

Les bits qui sortent par une extrémité sont perdus. Un décalage de `n` positions équivaut donc à une multiplication ou une division par `2^n`, tant qu'aucun bit utile n'est perdu.

### 5. Retenir les instructions

Les instructions arithmétiques et logiques suivent la même forme : la destination vient en premier, puis la source. Par exemple, `add rax, 42` ajoute 42 à `rax`, et `xor rax, 0x20` inverse le bit de casse de la partie de poids faible de `rax`. Quand un programme transforme une valeur avant de la comparer, il suffit d'inverser chaque opération, dans l'ordre inverse, pour retrouver la valeur attendue.
