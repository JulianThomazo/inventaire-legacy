# Inventaire 1.0.0

Première version publiée du projet.

## Nouveau

- Le panier calcule le total toutes taxes comprises, au taux normal de TVA de 20 %.
- Le stock refuse une sortie supérieure à la quantité disponible.

## Corrigé

- Le total du panier est arrondi au centime : il affiche 18,60 € au lieu de 18,599999999999998.

## Sécurité

- Aucune clé d'accès n'est stockée dans le projet. La configuration se fait dans un fichier local `.env`, à partir du modèle `.env.example`.

## Limites connues

- Pas encore de tests automatisés.
- Les montants sont calculés en nombres à virgule flottante ; un calcul exact au centime est prévu (#17).

## Installer et lancer

Avec un JDK 17 ou plus récent :

    javac -d target src/*.java
    java -cp target Panier