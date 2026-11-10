# inventaire-legacy

## Qu'est-ce que c'est ?

Un petit programme Java de gestion d'inventaire, repris par Julian Thomazo et Titouan Rousseau. Il contient deux classes :

- `Panier` : additionne des prix hors taxes et calcule le total TTC ;
- `Stock` : gère les entrées et les sorties de stock, et refuse une sortie supérieure à la quantité disponible.

## Comment je le lance ?

Prérequis : un JDK 17 ou plus récent (`javac -version`). Le projet n'utilise pas Maven.

```bash
javac -d target src/*.java
java -cp target Panier
```

La première commande compile les sources dans `target/`, qui est ignoré par Git. La seconde lance l'exemple de `Panier.main`, qui affiche le total TTC d'un panier de test.

Configuration : copier `.env.example` en `.env` et renseigner les valeurs. Le fichier `.env` ne doit jamais être commité.

## Comment je contribue ?

Les règles de branches, de messages de commit et de Pull Requests sont dans [CONTRIBUTING.md](CONTRIBUTING.md). La branche `main` est protégée : toute modification passe par une Pull Request relue.

## Licence

MIT, voir [LICENSE](LICENSE).
