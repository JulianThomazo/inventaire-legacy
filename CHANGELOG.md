# Changelog

Toutes les modifications notables de ce projet sont consignées ici.
Les numéros de version suivent [SemVer](https://semver.org/lang/fr/).

## [Unreleased]

### Added
- `README.md` : présentation du projet, compilation et contribution.
- `CONTRIBUTING.md` : conventions de branches, de messages de commit (Conventional Commits) et de Pull Requests.
- Licence MIT (`LICENSE`).
- `.gitignore` (secrets, build Java, sauvegardes, fichiers d'IDE) et `.env.example` sans valeur.
- `AUDIT.md` : audit du dépôt repris.

### Removed
- Fichiers de build et de sauvegarde suivis par erreur : `target/Panier.class`, `src/Panier.java.bak` (#8).
- `notes.txt`, remplacé par les issues du milestone v1.0.0 (#10).

### Fixed
- Le total TTC est arrondi au centime, au taux normal de TVA de 20 % (#16).

### Security
- Clé d'API de paiement (`.env`) retirée de tout l'historique avant la publication du dépôt.
