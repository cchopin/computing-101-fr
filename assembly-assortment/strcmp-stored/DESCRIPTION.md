Jusqu'ici, les valeurs que vous avez rétro-ingéniérées étaient intégrées directement dans les instructions, sous forme d'opérandes immédiats.
Ce défi compare en revanche le premier argument du programme à une chaîne codée en dur dans le binaire.
Cette chaîne se trouve dans une autre section du fichier du programme : la section `.rodata` (données en lecture seule), et non dans les instructions elles-mêmes.

Plusieurs méthodes permettent de la trouver :
- La plus familière : utiliser `stepi` jusqu'à l'endroit où la comparaison a lieu, puis examiner avec `x` les données pointées par les registres.
- Utiliser `strings /challenge/reverse-me` pour lister toutes les chaînes affichables du binaire. Il y en a beaucoup, mais l'une d'elles est le mot de passe.
- Utiliser `objdump -s -j .rodata /challenge/reverse-me` pour afficher le contenu brut de la section `.rodata`.

Vous choisissez la méthode qui vous convient !
