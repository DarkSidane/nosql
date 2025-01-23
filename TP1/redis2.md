# Redis : Sorted Sets et Hashes

## Sorted Sets (Ensembles Triés)

### Ajout d'Éléments avec Score
```bash
ZADD score4 19 "Augustin"
ZADD score4 18 "Ines"
ZADD score4 10 "Samir"
ZADD score4 8 "Philippe"
```
- `ZADD` crée un ensemble trié avec un score pour chaque élément
- Les éléments sont automatiquement triés par leur score

### Affichage des Éléments
```bash
ZRANGE score4 0 -1     # Affiche tous les éléments par ordre croissant
ZRANGE score4 0 1      # Affiche les 2 premiers éléments
ZREVRANGE score4 0 -1  # Affiche tous les éléments par ordre décroissant
```
- `ZRANGE` permet de récupérer des éléments par leur position
- `-1` signifie le dernier élément
- `ZREVRANGE` inverse l'ordre de tri

### Rang d'un Élément
```bash
ZRANK score4 "Augustin"  # Trouve la position dans l'ordre croissant
```
- Retourne la position de l'élément dans le classement

## Hashes (Dictionnaires)

### Création d'un Hash
```bash
HSET user:11 username "syoucef"
HSET user:11 age 31
HSET user:11 email samir.youcef@polytechnancy.fr
```
- `HSET` ajoute des champs à un hash (similaire à un dictionnaire)
- Permet de stocker plusieurs attributs pour une même clé

### Récupération des Informations
```bash
HGETALL user:4          # Récupère tous les champs et valeurs
HGET user:4 age         # Récupère une valeur spécifique
HVALS user:4            # Récupère uniquement les valeurs
```
- `HGETALL` retourne tous les champs et leurs valeurs
- `HGET` permet de cibler un champ spécifique

### Incrémentation de Valeur
```bash
HINCRBY user:4 age 4    # Augmente la valeur du champ 'age' de 4
```
- `HINCRBY` incrémente une valeur numérique d'un hash
