Vous avez appris à lire et à écrire des registres, à parcourir la mémoire, et à empaqueter votre code sous forme de fonction dans une bibliothèque partagée.
Maintenant, réunissons ces compétences pour construire de vrais algorithmes.
Quand vous devez déboguer une de ces soumissions `.so`, reportez-vous à [Écrire depuis une bibliothèque partagée](/computing-101/control-flow/callee-write) pour la méthode de débogage des bibliothèques partagées.

Au fil de ce module, nous enrichirons progressivement nos solutions jusqu'à obtenir un code qui répond à un besoin très courant des programmes : transformer du texte en nombres.
Chemin faisant, nous apprendrons à écrire du code assembleur réutilisable, à comprendre le lien entre texte et nombres, et à raisonner sur les algorithmes (et sur leurs limites !).
Vous gagnerez en connaissances, et en flags !

---

## Fiche de cours (ajout, hors traduction)

*Cette fiche n'est pas une traduction. Elle reprend les notions de la présentation « Assembly » de Yan Shoshitaishvili (Arizona State University), en anglais, dont le lien figure dans les ressources du module, et l'enrichit d'explications générales. Elle ne contient aucune solution de défi.*

### 1. Le texte que reçoit un programme

Les programmes reçoivent une grande partie de leurs données sous forme de texte. Un nombre passé en argument est donc une suite d'octets ASCII : `12345` est constitué des caractères `'1'`, `'2'`, `'3'`, `'4'`, `'5'`, soit `0x31` à `0x35`. Pour l'utiliser comme nombre, il faut le convertir.

### 2. Chiffres et valeurs

En ASCII, les chiffres se suivent : `'0'` vaut `0x30`, `'9'` vaut `0x39`. Ainsi, la valeur d'un chiffre est son caractère moins `'0'`, et le caractère d'une valeur est la valeur plus `'0'`.

### 3. Convertir un nombre décimal en valeur

Un nombre à plusieurs chiffres se calcule en parcourant ses chiffres de gauche à droite : le total courant est multiplié par dix, puis le chiffre suivant est ajouté. Cette méthode est simplement la règle de position des chiffres décimaux appliquée pas à pas.

### 4. Le sens inverse : de la valeur au texte

Pour afficher un nombre, on divise par dix : le reste donne le dernier chiffre, et le quotient continue. Les chiffres sortent donc à l'envers, et doivent être remis dans l'ordre (par exemple avec une pile, qui inverse naturellement l'ordre).

### 5. Division et instruction `div`

Sur x86, `div` divise la valeur formée par `rdx:rax` (128 bits) par l'opérande donné, et place le quotient dans `rax` et le reste dans `rdx`. Il faut donc mettre `rdx` à zéro avant une division de petits nombres, faute de quoi la moitié haute de la valeur contient des données inattendues.

### 6. Les caractères de contrôle et les chaînes

Une chaîne C se termine par un octet nul (`0x00`), qui n'a rien à voir avec le caractère `'0'`. La fin d'une chaîne se détecte donc par un test sur `0`, et les chiffres se détectent par un test sur l'intervalle `'0'` à `'9'`.

### 7. Les signes

Un nombre négatif s'écrit avec le caractère `'-'` (`0x2d`) en tête. Pour l'interpréter, on retient le signe, on convertit la partie positive, puis on change le signe du résultat. Dans l'autre sens, on écrit le signe puis la valeur absolue.

### 8. Les arguments de la ligne de commande

Au lancement, la pile contient `argc`, puis les pointeurs vers `argv[0]`, `argv[1]`, et ainsi de suite, puis un pointeur nul. Chaque argument est une chaîne terminée par un nul. C'est ce qui permet de lire des nombres passés en argument.

### 9. Le format `printf`

Un formateur parcourt une chaîne de format : les caractères ordinaires sont recopiés, la barre oblique inverse introduit des échappements (`\n`, `\\`, `\xNN`), et le signe pour cent introduit des marqueurs (`%d`, `%s`, `%%`). Pour chaque marqueur, le formateur consomme la prochaine valeur d'argument. C'est la même logique que celle de la fonction `printf` de la bibliothèque standard C.
