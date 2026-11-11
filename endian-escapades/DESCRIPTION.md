x86 stocke chaque valeur sur plusieurs octets en *petit-boutiste* (l'octet de poids faible d'abord), et ce fait apparaît partout où l'on examine la mémoire brute : dumps hexadécimaux, débogueurs, exploits.
Pour travailler correctement dans ces contextes, il faut comprendre l'ordre des octets.
Ce module vous le rend naturel.

Vous allez déchiffrer une série de programmes qui cachent un mot de passe dans leur propre code compilé, en le relisant dans le désassemblage, quelle que soit la taille choisie : un « quadruple mot », un octet isolé, ou une structure.
À la fin, l'ordre des octets ne vous piégera plus jamais dans un dump hexadécimal, un débogueur ou un exploit (ou nous ajouterons des défis pour y remédier !).
