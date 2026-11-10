# Contribuer à inventaire-legacy

Ces règles s'appliquent à tous les membres de l'équipe. Ce qui n'est pas écrit ici se discute dans une issue, puis s'ajoute à ce fichier par Pull Request.

## Avant de commencer

- Configurer son identité une fois pour toutes, avec son vrai nom et une seule adresse :
  `git config --global user.name "Prénom Nom"` et `git config --global user.email "adresse"`.
- Tout travail part d'une issue. Si elle n'existe pas, la créer d'abord, avec un label, un critère de fin et le milestone en cours.

## Branches

- `main` est protégée : aucun push direct, aucun push forcé. Elle doit toujours compiler.
- Une branche par issue, créée depuis `main` à jour :

  ```bash
  git switch main
  git pull
  git switch -c <type>/<description>
  ```

- Nom : `<type>/<description-courte>`, en minuscules, mots séparés par des tirets, sans accents.
  - `feature/` : nouvelle fonctionnalité (ex. `feature/remise-panier`) ;
  - `fix/` : correction (ex. `fix/conflit-tva`) ;
  - `docs/` : documentation (ex. `docs/changelog`) ;
  - `chore/` : maintenance, nettoyage (ex. `chore/retirer-build`).
- La branche est supprimée après sa fusion.

## Messages de commit

Format Conventional Commits :

```
type(portée): résumé à l'impératif, court

Corps facultatif : pourquoi ce changement.

Closes #12
```

- Types : `feat`, `fix`, `docs`, `test`, `refactor`, `chore`.
- Résumé en français, à l'impératif, 72 caractères maximum, sans point final.
- Un commit = un changement cohérent. Interdits : `fix`, `modifs`, `final`, `wip`…
- Exemples : `fix(panier): arrondit le total TTC au centime`, `docs: ajoute le changelog de la v1.0.0`.

## Pull Requests

- Une Pull Request par issue, petite et focalisée.
- Titre au format Conventional Commits, par exemple `chore: retire les fichiers de build`.
- Description : ce qui change, pourquoi, comment le vérifier, et `Closes #<numéro>` pour fermer l'issue à la fusion.
- Avant d'ouvrir la Pull Request, vérifier que le projet compile : `javac -d target src/*.java`.
- Une approbation de l'autre membre de l'équipe est obligatoire. On ne fusionne jamais sa propre Pull Request sans cette approbation.
- Fusion par commit de fusion (« Create a merge commit »), puis suppression de la branche.

## Revues

- Le relecteur commente avec les Conventional Comments, en citant la ligne concernée dans le diff :
  - `issue (blocking): …` : doit être corrigé avant la fusion ;
  - `suggestion (non-blocking): …` : amélioration possible ;
  - `question: …` : demande d'explication ;
  - `nitpick (non-blocking): …` : détail de forme ;
  - `praise: …` : ce qui est bien fait.
- L'auteur répond à chaque commentaire, corrige par de nouveaux commits sur la même branche, puis redemande une revue.

## Secrets

- Ne jamais commiter de secret (clé d'API, mot de passe). Les valeurs vont dans `.env`, ignoré par Git ; `.env.example` liste les variables sans valeur.
- Un secret commité par erreur se signale immédiatement : il faut le révoquer, le remplacer, puis purger l'historique.
