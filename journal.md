
Compilation : java -cp target Panier affiche 18.599999999999998 au lieu de 18,60 euros, car le calcul en double n'est pas arrondi au centime. Preuve utile pour le conflit du J3.

## J3 — Trancher le conflit (Titouan)

### Prédiction, écrite le 13/11/2026 à 16:00, avant la fusion

- Ce que je pense voir : un conflit dans `src/Panier.java`, sur la ligne `return total * 1.20;`, que les deux branches modifient. `arrondi` la remplace par `Math.round(total * 1.20 * 100) / 100.0` et `nouvelle-tva` par `total * 1.055`. La première fusion (`arrondi`) passera sans conflit, la seconde (`nouvelle-tva`) s'arrêtera sur cette ligne.
- Version qui doit l'emporter : `arrondi` (taux de 20 %, arrondi au centime).
- Pourquoi : 20 % est le taux normal de TVA, 5,5 % est un taux réduit réservé à certains produits (alimentation, livres), et rien dans le projet n'indique que l'inventaire en contient. L'arrondi corrige le bug déjà relevé : le programme affiche `18.599999999999998` au lieu de 18,60 €.