# Computing 101 (français)

Traduction française du dojo **Computing 101** de [pwn.college](https://pwn.college), un cours d'initiation à l'informatique bas niveau : programmes, processeur, langage d'assemblage, registres, mémoire, outils de débogage et serveur web.

Ce dépôt ne contient ni le code des défis, ni leurs vérifications. Ils sont importés depuis le dojo officiel `computing-101`, avec les mêmes environnements, les mêmes vérifications et les mêmes flags. Seuls les textes sont traduits : noms et descriptions des dojos, modules et défis, ressources.

## Crédit et licence

- Contenu d'origine : © 2022 pwn.college, dépôt [`pwncollege/computing-101`](https://github.com/pwncollege/computing-101), commit `38a1402207912c9e5844a66e993ba62d0f616226`.
- Les textes traduits sont dérivés de ce contenu. Le fichier [LICENSE](LICENSE) reproduit la licence BSD 2-Clause d'origine, qui doit être conservée avec toute copie ou modification.
- Ce dépôt ne reprend aucune diapositive ni vidéo. Les liens vers les vidéos et les diapositives sont ceux de l'original, signalés comme « en anglais ».
- Diapositives : le site de pwn.college indique que les vidéos et diapositives des cours peuvent être utilisées librement à des fins non commerciales, avec attribution, et demande d'écrire à l'équipe si pwn.college sert dans un programme d'enseignement. Une mention de licence CC BY-NC apparaît dans un résultat de recherche, mais elle n'a pas été vérifiée sur la source : à confirmer avant toute diffusion.
- Le dépôt d'origine n'est plus maintenu à cette adresse. Le développement se poursuit sur [`pwncollege/challenges`](https://github.com/pwncollege/challenges).

Ce dépôt reste **privé** tant que la question des diapositives n'est pas tranchée.

## Contenu

- `dojo.yml` : déclaration du dojo, de ses modules et de leurs défis. Chaque module et chaque défi importe le contenu officiel (`import`).
- `DESCRIPTION.md` : description du dojo.
- Un dossier par module (`your-first-program/`, etc.) contenant `module.yml` (nom, ressources, défis) et `DESCRIPTION.md` (description du module, avec une fiche de cours ajoutée).
- Un sous-dossier par défi, contenant `DESCRIPTION.md`.
- [GLOSSAIRE.md](GLOSSAIRE.md) : termes techniques et traductions retenues.
- `verifier.py` : contrôles automatiques (structure, code, liens, typographie).

## Installer le dojo sur pwn.college

Procédure vérifiée dans le code de pwn.college (`site/dojo/pages/dojos.py`, `site/dojo/api/v1/dojos.py`, `site/dojo/utils/dojo.py`) :

1. Créer le dépôt GitHub, par exemple `<votre-compte>/computing-101-fr`, et y pousser ce contenu.
2. Sur pwn.college, ouvrir `/dojos/create`. Le formulaire fournit une clé de déploiement publique et privée.
3. Ajouter la clé publique dans le dépôt, à `https://github.com/<votre-compte>/computing-101-fr/settings/keys`. Ce n'est obligatoire que pour un dépôt privé.
4. Renseigner le dépôt au format `propriétaire/dépôt` et valider. Un compte non administrateur peut créer un dojo par jour.
5. Pour mettre à jour après un push, utiliser le bouton **Update** de la page d'administration du dojo, ou appeler l'URL de mise à jour depuis un webhook GitHub. Le serveur fait un `fetch` puis un `reset --hard` sur la branche distante : toute modification faite directement sur le serveur est écrasée.

Le dojo est déclaré `type: hidden`. Il n'apparaît donc pas dans la liste publique.

**Point à vérifier avant l'installation** : l'import ne fonctionne que si le dojo `computing-101` est officiel sur pwn.college, et si ses défis sont importables. Ces deux valeurs se lisent dans la base de pwn.college, pas dans ce dépôt.

## Commit de référence

La traduction est basée sur `pwncollege/computing-101` au commit `38a1402207912c9e5844a66e993ba62d0f616226`.

## Sources locales

Le dossier `sources/` (ignoré par git) contient les dépôts de référence clonés pour l'étude : `computing-101`, `example-dojo`, `example-import-dojo`, `dojo`. Il contient aussi les diapositives téléchargées dans `sources/slides/`.

## Étude préalable

Voir [ETUDE.md](ETUDE.md).
