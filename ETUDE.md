# Étude préalable : dojo Computing 101 en français

Sources lues (clonées dans `sources/`, `--depth 50`) :

| Dépôt | Commit lu |
|---|---|
| `pwncollege/computing-101` | `38a1402207912c9e5844a66e993ba62d0f616226` |
| `pwncollege/example-dojo` | `10b0b68c57b87c7b963b50e01f279797e10d0c4c` |
| `pwncollege/example-import-dojo` | `9df2ef9d9861e06cdbd982aae745df674b0942b3` |
| `pwncollege/dojo` | `ee1c6e2653d8aa8301b6190a4f5dafb829471418` |

Les chemins ci-dessous sont relatifs à `sources/`. Les numéros de ligne correspondent à ces commits.

---

## 1. Licence de `computing-101`

**Réponse : oui, une traduction dans un dépôt personnel est autorisée**, à condition de conserver la notice de copyright et l'avertissement de la licence.

- `computing-101/LICENSE` : BSD 2-Clause, « Copyright (c) 2022, pwn.college ». La licence autorise la redistribution et la modification, avec ou sans forme binaire, si la notice de copyright, la liste des conditions et l'avertissement sont reproduits. Une traduction est une œuvre dérivée : elle reste couverte par cette licence, donc la notice doit figurer dans le dépôt français (le `LICENSE` d'origine y est copié, voir étape 2).
- Limite à signaler : `computing-101/README.md` indique que le dépôt « n'est plus maintenu ici » et que le développement est passé vers `https://github.com/pwncollege/challenges`. La traduction se base donc sur un état figé (commit ci-dessus).

**Diapositives** (non couvertes par la licence BSD du dépôt) :

- La page d'accueil `https://pwn.college/` indique que « les vidéos et diapositives des cours peuvent être utilisées librement à des fins non commerciales, avec attribution », et demande d'envoyer un courriel si pwn.college sert dans un programme d'enseignement.
- Un résultat de recherche mentionnait une licence CC BY-NC sur un site statique (`static.pwn.college`). **Non vérifié** : ce domaine n'est pas résolu depuis cette machine. Ce point reste à confirmer auprès de pwn.college ou sur le site statique.
- Conséquence pratique : le dépôt français reste **privé** tant que la question des diapositives n'est pas tranchée, et il ne doit pas servir à un usage commercial.

## 2. Déclaration d'un challenge importé

Trois niveaux d'import, décrits dans `example-import-dojo/dojo.yml` et validés dans `site/dojo/utils/dojo.py` (fonction `dojo_from_spec`, lignes 363 et suivantes) :

- **Dojo** : `import: {dojo: <id>}` à la racine. Sans clé `modules`, tous les modules de la source sont copiés (`dojo.py` ~ ligne 540). La source doit être un dojo **officiel** : `filter_by(official=True)`, ligne 406.
- **Module** : une entrée de `modules:` avec `import: {dojo: <id>, module: <id>}`. La clé `dojo` peut être omise si elle est héritée du niveau dojo (`import_ids`, ligne 470 environ).
- **Challenge** : une entrée avec `import: {dojo: <id>, module: <id>, challenge: <id>}`, placée dans `challenges:` ou `resources:` du module. Le champ `id` peut être omis (il prend la valeur de `import.challenge`, `dojo.py` ligne 238).

Pour un challenge importé, `DojoChallenges.__init__` (`site/dojo/models/__init__.py`, ~ lignes 770 à 780) met `challenge=None` et enregistre `path_override = default.path` : les fichiers du challenge (binaires, `run`, `check`) sont donc ceux du dojo source, pas de copies locales.

## 3. Remplacer le nom et la description d'un challenge importé

**Réponse : oui, les deux.** Mécanisme vérifié dans le code de chargement.

**Nom** : champ `name` dans `dojo.yml` (ou `challenge.yml`). Il est **obligatoire en pratique**. Si `name` manque, `load_dojo_subyamls` (`dojo.py` lignes 286-287) donne au challenge importé le nom « id avec tirets remplacés par espaces, en capitales initiales », par exemple `Rax` au lieu de `Your First Register`. Le nom de la source n'est jamais repris dans ce cas.

**Description** : deux sources possibles, avec priorité :

1. `description:` dans `dojo.yml` (ou `module.yml`, `challenge.yml`) : prioritaire.
2. Sinon, fichier `DESCRIPTION.md` dans le dossier `<module>/<challenge>/` : `setdefault_file` (`dojo.py` ligne 283). Ce fichier est lu même quand le challenge est importé, puisque la boucle de `load_dojo_subyamls` ne regarde que l'`id`.
3. Sinon, la description de la source est reprise (`DojoChallenges.__init__`, repli sur `default.description`).

**Priorité générale** : les valeurs de `dojo.yml` l'emportent sur celles de `module.yml` et `challenge.yml` (`setdefault_subyaml`, `dojo.py` ligne 204).

Choix recommandé : `name` explicite dans `dojo.yml`, et `DESCRIPTION.md` par challenge pour les textes longs.

## 4. Description et ressources d'un module

**Description du module** : `description:` dans `dojo.yml` ou `module.yml`, ou fichier `<module>/DESCRIPTION.md` (`setdefault_file`, `dojo.py` ligne 271). Ce fichier est lu pour **tout** module ayant un `id`, importé ou non.

**Nom du module** : `name:` explicite. Pour un module sans `import`, à défaut, `setdefault_name` donne le nom dérivé de l'identifiant (`dojo.py` lignes 189-197). Pour un module importé, `setdefault_name` ne fait rien : le nom retombe sur celui de la source seulement si rien n'est fourni (`DojoModules.__init__`, `models/__init__.py` ~ ligne 589).

**Ressources** (diapositives, vidéos, Markdown) :

- Elles se déclarent dans `resources:` de `dojo.yml` ou `module.yml`. Types vus dans `dojo.py` lignes 115 à 150 : `lecture` (`name`, `video`, `playlist`, `slides`), `markdown` (`content` ou `file`), `header` (`content`).
- **Remplacement, pas fusion** : si un module importé déclare sa liste `resources`, elle remplace celle de la source (`DojoModules.__init__` : `kwargs.pop("resources") or default.resources`). Il faut donc redéclarer les ressources `lecture` (titres, vidéos, diapositives) dans le module français.
- **Piège** : la forme raccourcie `challenges:` ajoute automatiquement un en-tête en anglais `"Challenges"` (`dojo.py` ligne 228). Pour une interface entièrement française, il faut écrire explicitement un `header` avec le libellé français, et ne pas utiliser `challenges:` pour l'en-tête.

## 5. Une validation dans le dojo français compte-t-elle dans le dojo officiel ?

**Réponse : oui, par construction du code, sous réserve d'un test en conditions réelles.**

- Un challenge importé partage la ligne `Challenges` de la source : `challenge = default.challenge` (`DojoChallenges.__init__`, ligne ~ 775).
- Les validations (`Solves`) sont rattachées à `challenge_id` de cette ligne, et la progression compte les `Solves` joints sur `DojoChallenges.challenge_id == Solves.challenge_id` (`models/__init__.py`, ligne 850).
- Le drapeau (flag) est donc le même, et une validation vaut dans les deux dojos.

Limites :

- L'import exige que le challenge source soit `importable`. La valeur par défaut est `True` (`models/__init__.py` lignes 253, 561, 760), mais la valeur réelle en base sur pwn.college ne se lit pas dans le dépôt. À confirmer sur la page du dojo officiel avant la mise en production.
- Ce point n'a pas été testé sur la plateforme : aucun compte n'a été créé et aucun dojo enregistré.

## 6. Procédure d'enregistrement d'un dojo depuis GitHub

Lue dans `site/dojo/pages/dojos.py` (`dojo_create`, ~ ligne 90), `site/dojo/api/v1/dojos.py` (`/create`, ~ ligne 112), `site/dojo/utils/dojo.py` (`dojo_create`, ligne 689) et `site/theme/templates/dojo_create.html` :

1. Créer le dépôt GitHub (public, ou privé avec clé de déploiement).
2. Sur pwn.college, ouvrir la page de création de dojo (`/dojos/create`). Le formulaire affiche :
   - le champ **GitHub Repository**, au format `propriétaire/dépôt` (regex `[\w\-]+/[\w\-]+`) ;
   - une **clé publique** de déploiement (ed25519, générée par `generate_ssh_keypair`, `utils/dojo.py` ligne 611) et la clé privée associée, en champ caché.
3. Ajouter la clé publique dans GitHub, à `https://github.com/<propriétaire>/<dépôt>/settings/keys` (URL donnée dans le message d'erreur, `dojo.py` ligne 726). Nécessaire pour un dépôt privé. Le clonage essaie d'abord HTTPS, puis SSH avec la clé (`dojo_clone`, lignes 644-666).
4. Soumettre : `POST /api/v1/dojos/create` avec `repository`, `public_key`, `private_key` (ou `spec` pour un admin). Limite : **un dojo par jour et par IP** pour un non-admin (`api/v1/dojos.py` ligne 130). Un dépôt déjà enregistré est refusé.
5. Mise à jour après un push :
   - bouton **Update** de la page d'administration du dojo (`site/theme/templates/dojo_admin.html`, ligne 25), ou
   - l'URL `POST /dojo/<id>/update/<update_code>` (affichée dans `dojo_admin.html` ligne 65), appelable depuis un webhook GitHub. Le `update_code` est un secret : il ne doit pas être publié dans le dépôt.
   - Le serveur fait un `fetch` puis un `reset --hard` sur `origin` (`dojo_update`, `utils/dojo.py` lignes 748-770) : tout changement local sur le serveur est écrasé.

Rien n'a été fait sur pwn.college ni sur GitHub.

---

## Points complémentaires relevés pendant l'étude

1. **Module fantôme `advanced-memory`** : `computing-101/dojo.yml` le déclare, mais il n'a plus de dossier. Le commit `5d85bc7 remove advanced-memory module (#67)` l'a supprimé, et `16d0975 memory: move mem-triple-deref to advanced-memory (#66)` avait déplacé son contenu. Dans le dépôt, `advanced-memory` n'a ni `module.yml` ni `DESCRIPTION.md`. **Décision à prendre** : le garder vide (fidèle à `dojo.yml`) ou le retirer. Ce rapport part de l'hypothèse de le garder, comme demandé (« mêmes modules »), mais un module vide s'affichera vide.
2. **Type du dojo** : l'original est `type: welcome`, le français sera `type: hidden`. Le type `hidden` existe bien (`site/dojo/pages/dojos.py` ligne 59). Le type `welcome` sert au parcours d'accueil (`pages/dojos.py` ligne 18) ; un dojo `hidden` n'y figure pas. C'est voulu pour un dojo de traduction privé.
3. **Diapositives** : l'export texte de la première présentation (`1sVyPL92...`) ne fait que 1 049 octets, alors que l'export PDF fait 251 Ko. Le texte seul ne suffira probablement pas : il faudra lire le PDF.
4. **Nom des modules et challenges** : les contraintes de validation sont `NAME_REGEX = ^[\S ]{1,128}$` (accents et parenthèses acceptés, 128 caractères max) et `ID_REGEX = ^[a-z0-9-]{1,32}$` (`site/dojo/utils/dojo.py` lignes 34-36). Les identifiants restent donc en anglais minuscules, comme demandé.
5. **Module `your-first-program`** : c'est le premier module à traduire. Il déclare quatre `lecture` (`Computer Architecture`, `Assembly`, `Data`, `Registers`) avec vidéo, playlist et diapositives. Il faut donc les redéclarer (point 4 ci-dessus).

## Décisions reportées ou dépendant de l'utilisateur

- Licence des diapositives : CC BY-NC (non vérifiée) ou simple « usage non commercial avec attribution » (site officiel, vérifié). À confirmer avant toute diffusion.
- Module `advanced-memory` : garder vide ou retirer (point 1).
