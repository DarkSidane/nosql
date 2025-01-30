# Requêtes MongoDB sur une Collection de Films

Ce document présente une série de requêtes MongoDB effectuées sur une collection de films, avec les explications détaillées, la syntaxe et les résultats correspondants.

## Installation et Import

Pour commencer, importez la collection de films avec la commande suivante :

```bash
./mongoimport --db lesfilms --collection films films.json --jsonArray
```

Cette commande:
- Crée une base de données nommée "lesfilms"
- Crée une collection nommée "films"
- Importe les données du fichier films.json
- Le flag --jsonArray indique que le fichier contient un tableau JSON

## Structure des Documents

Un document dans la collection a la structure suivante :

```javascript
{
  _id: 'movie:11',          // Identifiant unique du film
  title: 'La Guerre des étoiles',  // Titre du film
  year: 1977,               // Année de sortie
  genre: 'Aventure',        // Genre du film
  summary: "...",           // Résumé du film
  country: 'US',            // Pays de production
  director: {               // Informations sur le réalisateur
    _id: 'artist:1',
    last_name: 'Lucas',
    first_name: 'George',
    birth_date: 1944
  },
  actors: [                 // Liste des acteurs
    {
      last_name: 'Hamill',
      first_name: 'Mark',
      birth_date: 1951
    },
    // ...
  ],
  grades: [                 // Notes et évaluations
    {
      note: 93,            // Note numérique
      grade: 'C'           // Grade alphabétique
    },
    // ...
  ]
}
```

## Requêtes

### 1. Vérification des données importées

Pour compter le nombre total de documents :
```javascript
db.films.count()
```

Cette commande compte tous les documents dans la collection films.

### 2. Afficher un seul document

```javascript
db.films.findOne()
```

Cette commande retourne le premier document de la collection. Dans notre cas, elle retourne "La Guerre des étoiles".

### 3. Liste des films d'action

```javascript
db.films.find({ "genre": "Action" })
```

Cette requête recherche tous les films dont le genre est "Action". 
Résultats trouvés incluent :
- Kill Bill : Volume 1 (2003)
- Minority Report (2002)
- Gladiator (2000)
- Terminator (1984)
- Batman Begins (2005)
- Terminator 2: Judgment Day (1991)
Et d'autres...

### 4. Nombre de films d'action

```javascript
db.films.count({ "genre": "Action" })
```

Résultat : **36 films** d'action dans la base

### 5. Films d'action français

```javascript
db.films.find({ 
    "genre": "Action", 
    "country": "FR" 
})
```

Résultat : 3 films trouvés
1. "L'Homme de Rio" (1964)
   - Réalisé par Philippe de Broca
   - Avec Jean-Paul Belmondo
2. "Nikita" (1990)
   - Réalisé par Luc Besson
   - Avec Anne Parillaud, Jean Reno
3. "Les tontons flingueurs" (1963)
   - Réalisé par Georges Lautner
   - Avec Lino Ventura

### 6. Films d'action français de 1963

```javascript
db.films.find({ 
    "genre": "Action", 
    "country": "FR", 
    "year": 1963 
})
```

Résultat : Un seul film
- "Les tontons flingueurs"
  - Réalisateur : Georges Lautner
  - Acteurs principaux : Lino Ventura, Bernard Blier, Francis Blanche
  - Notes : [35 (C), 48 (F), 64 (C), 42 (F)]

### 7. Films d'action français sans les notes

```javascript
db.films.find(
    { "genre": "Action", "country": "FR" },  // Critères de recherche
    { "grades": 0 }                          // Exclusion du champ grades
)
```

Cette requête retourne les mêmes films que la requête 5, mais sans les informations de notes.

### 8. Films d'action français sans les identifiants

```javascript
db.films.find(
    { "genre": "Action", "country": "FR" },  // Critères de recherche
    { "_id": 0 }                            // Exclusion de l'identifiant
)
```

Même résultat que la requête 5, mais sans les identifiants (_id).

### 9. Titres et notes des films d'action français

```javascript
db.films.find(
    { "genre": "Action", "country": "FR" },
    { "_id": 0, "title": 1, "grades": 1 }    // Projection : uniquement titre et notes
)
```

Résultats :
1. L'Homme de Rio
   - Notes : [4 (D), 30 (E), 34 (E), 28 (F)]
2. Nikita
   - Notes : [88 (F), 36 (C), 74 (D), 62 (D)]
3. Les tontons flingueurs
   - Notes : [35 (C), 48 (F), 64 (C), 42 (F)]

### 10. Films avec note supérieure à 10

```javascript
db.films.find(
    { 
        "genre": "Action", 
        "country": "FR",
        "grades.note": { $gt: 10 }           // Note supérieure à 10
    },
    { "_id": 0, "title": 1, "grades": 1 }
)
```

Cette requête trouve tous les films ayant au moins une note supérieure à 10.

### 11. Films avec toutes les notes supérieures à 10

```javascript
db.films.find(
    { 
        "genre": "Action", 
        "country": "FR",
        "grades": { $not: { $elemMatch: { note: { $lte: 10 } } } }
    },
    { "_id": 0, "title": 1, "grades": 1 }
)
```

Résultats :
- Nikita
- Les tontons flingueurs

### 12. Liste des genres

```javascript
db.films.distinct("genre")
```

Résultat : 
```javascript
[
  'Action',          'Adventure',
  'Aventure',        'Comedy',
  'Comédie',         'Crime',
  'Drama',           'Drame',
  'Fantastique',     'Fantasy',
  'Guerre',          'Histoire',
  'Horreur',         'Musique',
  'Mystery',         'Mystère',
  'Romance',         'Science Fiction',
  'Science-Fiction', 'Thriller',
  'War',             'Western'
]
```

### 13. Liste des grades

```javascript
db.films.distinct("grades.grade")
```

Résultat : `[ 'A', 'B', 'C', 'D', 'E', 'F' ]`

### 14. Films avec Leonardo DiCaprio en 1997

```javascript
db.films.find({
    "actors": { 
        $elemMatch: { 
            "last_name": "DiCaprio",
            "first_name": "Leonardo"
        }
    },
    "year": 1997
})
```

Résultat : Un film
- "Titanic"
  - Réalisateur : James Cameron
  - Année : 1997
  - Genre : Drame

### 15. Films avec Leonardo DiCaprio ou de 1997

```javascript
db.films.find({
    $or: [
        { "actors": { 
            $elemMatch: { 
                "last_name": "DiCaprio",
                "first_name": "Leonardo"
            }
        }},
        { "year": 1997 }
    ]
})
```

Résultats incluent :
1. Films avec DiCaprio :
   - Titanic (1997)
   - Inception (2010)
   - Django Unchained (2012)
   - The Revenant (2015)
2. Films de 1997 :
   - Jackie Brown
   - Le monde perdu : Jurassic Park
   - Starship Troopers
   - Volte/Face
   - On connaît la chanson

## Notes sur les Opérateurs MongoDB

- `$gt` (greater than) : Sélectionne les documents où la valeur est supérieure au critère
- `$lte` (less than or equal) : Sélectionne les documents où la valeur est inférieure ou égale au critère
- `$in` : Sélectionne les documents où la valeur correspond à une des valeurs du tableau
- `$exists` : Vérifie l'existence d'un champ
- `$or` : Effectue une opération logique OU entre les conditions
- `$not` : Inverse le critère de sélection
- `$elemMatch` : Filtre les documents contenant un élément de tableau correspondant à tous les critères

## Projections

Dans MongoDB, les projections (second paramètre de find) permettent de :
- Inclure des champs spécifiques (valeur : 1)
- Exclure des champs spécifiques (valeur : 0)
- Ne pas retourner l'identifiant (_id : 0)

Par exemple : `{ "_id": 0, "title": 1, "grades": 1 }` ne retourne que le titre et les notes.
