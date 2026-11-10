# Audit du dépôt inventaire-legacy

Audit fait sans rien modifier, avec `git log --stat`, `git shortlog -sne --all`, `git ls-files` et `git log --oneline --graph --all`.

## 1\. Secret dans l'historique

* **Où :** le fichier `.env`, ajouté par le commit `0cc0662` (« fix »). Il contient `PAIEMENT\_API\_KEY`, une clé d'API de paiement.
* **Pourquoi c'est un problème :** toute personne qui clone le dépôt peut lire la clé. Supprimer le fichier dans un nouveau commit ne suffit pas : la clé reste dans l'historique, dans le commit `0cc0662`.
* **Ce que je ferai :** avant de publier le dépôt, retirer `.env` de tout l'historique avec `git filter-repo`, ajouter `.env` au `.gitignore` pour qu'il ne revienne pas, et versionner un `.env.example` avec les noms des variables mais sans valeur.

## 2\. Fichiers de build et de sauvegarde

* **Où :** `target/Panier.class` et `src/Panier.java.bak`, ajoutés par le commit `3eb8584` (« fix 2 »).
* **Pourquoi c'est un problème :** `Panier.class` est un fichier compilé, il se régénère avec `javac` et n'a pas sa place dans les sources. `Panier.java.bak` est une copie de sauvegarde inutile, puisque Git garde déjà toutes les versions de `Panier.java`. Ces fichiers encombrent le dépôt et peuvent ne plus correspondre au code.
* **Ce que je ferai :** les supprimer avec `git rm`, et ajouter `target/` et `\*.bak` au `.gitignore`.

## 3\. Liste de tâches dans notes.txt

* **Où :** `notes.txt`, ajouté par le commit `c742c1f` (« final VRAI »). Il contient « à faire : README, tests, version 1.0 ».
* **Pourquoi c'est un problème :** une liste de tâches dans un fichier n'est ni suivie, ni attribuée, ni reliée au code qui la réalise.
* **Ce que je ferai :** transformer ces tâches en issues GitHub rattachées au milestone v1.0.0, puis supprimer `notes.txt`.

## 4\. Messages de commit illisibles

* **Où :** sur `main` : `init`, `modifs`, `fix`, `fix 2`, `final`, `final VRAI` ; sur les branches : `wip`, `taux`, `ça marche ???`.
* **Pourquoi c'est un problème :** aucun message ne dit ce qui a changé ni pourquoi. Certains sont même trompeurs : « fix » n'a rien corrigé, il a ajouté le secret. Relire l'historique ne permet pas de comprendre le projet.
* **Ce que je ferai :** ne pas réécrire ces anciens messages, mais imposer les Conventional Commits (`feat:`, `fix:`, `docs:`, `chore:`…) dans `CONTRIBUTING.md` pour tous les nouveaux commits.

## 5\. Un développeur sous deux identités

* **Où :** `git shortlog -sne --all` affiche trois auteurs : `Marc Durand <marc.durand@perso.example>` (4 commits), `m.durand <m.durand@ecole.example>` (2 commits) et `Léa Martin <lea.martin@ecole.example>` (3 commits).
* **Pourquoi c'est un problème :** `Marc Durand` et `m.durand` sont la même personne. Il y a deux développeurs pour trois identités, donc les statistiques de contribution sont fausses et on ne sait pas facilement qui a fait quoi.
* **Ce que je ferai :** unifier les deux identités avec un fichier `.mailmap` (approfondissement), et demander dans `CONTRIBUTING.md` de configurer `user.name` et `user.email` une fois pour toutes.

## 6\. Branche morte et branches en conflit

* **Où :** `test-marc` ne contient qu'un commit `wip`, qui ajoute un commentaire `// essai` à `Stock.java`. `nouvelle-tva` et `arrondi` modifient toutes les deux la ligne `return total \* 1.20;` de `src/Panier.java`.
* **Pourquoi c'est un problème :** `test-marc` est abandonnée et encombre le dépôt. `nouvelle-tva` (taux de 1.055) et `arrondi` (arrondi au centime) vont entrer en conflit, et rien n'indique laquelle est la bonne.
* **Ce que je ferai :** vérifier que `test-marc` ne contient rien d'utile (`git log main..test-marc`) avant de la supprimer. Pour le conflit, demander au commanditaire quel taux de TVA appliquer, puis fusionner par Pull Request en justifiant la version retenue.

## 7\. Fichiers manquants

* **Où :** à la racine du dépôt.
* **Pourquoi c'est un problème :** sans `README.md`, on ne sait pas ce que fait le projet ni comment le compiler. Sans `CONTRIBUTING.md`, il n'y a aucune règle commune. Sans `LICENSE`, personne n'a le droit de réutiliser le code. Sans `.gitignore`, les fichiers de build et les secrets peuvent revenir. Enfin, aucune version n'a jamais été publiée : il n'y a aucun tag.
* **Ce que je ferai :** ajouter `README.md`, `CONTRIBUTING.md`, `LICENSE` et `.gitignore`, puis livrer la version 1.0.0 avec un tag annoté, un `CHANGELOG.md` et des notes de version.

## Si le dépôt avait déjà été public

Retirer `.env` de l'historique n'aurait pas suffi : la clé aurait déjà pu être copiée par n'importe qui (clones, forks, copies en cache). Il aurait fallu, dans cet ordre :

1. **Révoquer** la clé tout de suite auprès du service de paiement, pour qu'elle ne fonctionne plus.
2. **La remplacer** par une nouvelle clé, gardée hors du dépôt.
3. **Purger** l'historique avec `git filter-repo`, puis republier avec un push forcé.
4. **Prévenir** l'équipe, qui doit supprimer ses anciens clones et recloner le dépôt.

L'ordre compte : purger d'abord laisserait la clé valide pendant tout le nettoyage.

