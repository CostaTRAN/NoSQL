# Tutoriel MongoDB - Prise en main

## Introduction à MongoDB
MongoDB est une base de données NoSQL orientée documents qui stocke les données sous forme de documents JSON. Il est utilisé pour sa flexibilité et ses performances élevées dans la gestion des données massives.

## Installation de MongoDB

### Sous Windows
1. Téléchargez MongoDB depuis le site officiel : [MongoDB Download](https://www.mongodb.com/try/download/community)
2. Installez MongoDB et MongoDBCompass en suivant les instructions fournies.

Ce TP a été réalisé avec MongoDB sur Windows via **MongoDBCompass**. Vous devez créer une nouvelle connexion. Ensuite, ajoutez la collection "films" et importez le fichier ***films.json***. Après avoir réalisé ces étapes, vous pouvez ouvrir un terminal MongoDB en appuyant sur le bouton "Open MongoDB shell" afin de pouvoir tester vos requêtes.

### 1. Vérifier que le données ont été importées, en les comptant par exemple.
```js
db.films.count()
```
Résultat :
```
278
```
Cette commande retourne le nombre de documents présents dans la collection `films`.

### 2. Avant de commencer à interroger la base de données lesfilms, il est essentiel de bien comprendre la structure d’un document dans notre collection de films. Pour ce faire, nous allons utiliser la fonction findOne(), qui permet de récupérer un seul document.
```js
db.films.findOne()
```
Résultat :
```js
{
  _id: 'movie:11',
  title: 'La Guerre des étoiles',
  year: 1977,
  genre: 'Aventure',
  summary: "Il y a bien longtemps, dans une galaxie très lointaine...",
  country: 'US',
  ...
}
```

### 3. Afficher la liste des films d’action.
```js
db.films.find({ genre: "Action" })
```
Résultat :
```js
_id: 'movie:24',
  title: 'Kill Bill : Volume 1',
  year: 2003,
  genre: 'Action',
  summary: "Au cours d'une cérémonie de mariage en plein désert, un commando fait irruption dans la chapelle et tire sur les convives. Laissée pour morte, la Mariée enceinte retrouve ses esprits après un coma de quatre ans. Celle qui a auparavant exercé les fonctions de tueuse à gages au sein du Détachement International des Vipères Assassines n'a alors plus qu'une seule idée en tête : venger la mort de ses proches en éliminant tous les membres de l'organisation criminelle, dont leur chef Bill qu'elle se réserve pour la fin.",
  country: 'US',
  director: {
    _id: 'artist:138',
    last_name: 'Tarantino',
    first_name: 'Quentin',
    birth_date: 1963
  },
  ...
```

### 4. Afficher le nombre de films d’action.
```js
db.films.countDocuments ({ genre: "Action" })
```
Résultat :
```js
36
```

### 5. Afficher les films d’action produits en France.
```js
db.films.find({ genre: "Action", country: "FR" })
```
Résultat :
```js
{
  _id: 'movie:25253',
  title: 'Les tontons flingueurs',
  year: 1963,
  genre: 'Action',
  summary: "Sur son lit de mort, le Mexicain fait promettre à son ami d'enfance, Fernand Naudin, de veiller sur ses intérêts et sa fille Patricia. Fernand découvre alors qu'il se trouve à la tête d'affaires louches dont les anciens dirigeants entendent bien s'emparer. Mais, flanqué d'un curieux notaire et d'un garde du corps, Fernand impose d'emblée sa loi. Cependant, le belle Patricia lui réserve quelques surprises !",
  country: 'FR',
  ...
}
```

### 6. Afficher les films d’action produits en France en 1963.
```js
db.films.find({ genre: "Action", country: "FR", year: 1963 })
```
Résultat :
```js
{
  _id: 'movie:25253',
  title: 'Les tontons flingueurs',
  year: 1963,
  genre: 'Action',
  summary: "Sur son lit de mort, le Mexicain fait promettre à son ami d'enfance, Fernand Naudin, de veiller sur ses intérêts et sa fille Patricia. Fernand découvre alors qu'il se trouve à la tête d'affaires louches dont les anciens dirigeants entendent bien s'emparer. Mais, flanqué d'un curieux notaire et d'un garde du corps, Fernand impose d'emblée sa loi. Cependant, le belle Patricia lui réserve quelques surprises !",
  country: 'FR',
  ...
}
```

### 7. Jusqu’à maintenant, à chaque fois qu’un document est renvoyé, tous ses attributs sont affichés. Pour n’afficher que certains attributs, on utilise un filtre. Pour afficher les films d’action réalisés en France, sans les grades, le filtre est passé comme deuxième paramètre de la fonction find() comme ceci :
```js
db.films.find({ genre: "Action", country: "FR" }, { grades: 0 })
```
Résultat :
```js
{
  _id: 'movie:25253',
  title: 'Les tontons flingueurs',
  year: 1963,
  genre: 'Action',
  summary: "Sur son lit de mort, le Mexicain fait promettre à son ami d'enfance, Fernand Naudin, de veiller sur ses intérêts et sa fille Patricia. Fernand découvre alors qu'il se trouve à la tête d'affaires louches dont les anciens dirigeants entendent bien s'emparer. Mais, flanqué d'un curieux notaire et d'un garde du corps, Fernand impose d'emblée sa loi. Cependant, le belle Patricia lui réserve quelques surprises !",
  country: 'FR',
  director: {
    _id: 'artist:18563',
    last_name: 'Lautner',
    first_name: 'Georges',
    birth_date: 1926
  },
  ...
}
```

### 8. Vous avez certainement remarqué que les identifiants des documents sont affichés même si un filtre est appliqué aux résultats. Pour ne pas les afficher, vous pouvez ajouter sa valeur est la mettre à zéro, comme ceci (cette requête nous renvois tous les films d’action réalisés en France sans les identifiants) :
```js
db.films.find({ genre: "Action", country: "FR" }, { _id: 0 })
```
Résultat :
```js
{
  title: 'Les tontons flingueurs',
  year: 1963,
  genre: 'Action',
  summary: "Sur son lit de mort, le Mexicain fait promettre à son ami d'enfance, Fernand Naudin, de veiller sur ses intérêts et sa fille Patricia. Fernand découvre alors qu'il se trouve à la tête d'affaires louches dont les anciens dirigeants entendent bien s'emparer. Mais, flanqué d'un curieux notaire et d'un garde du corps, Fernand impose d'emblée sa loi. Cependant, le belle Patricia lui réserve quelques surprises !",
  country: 'FR',
  director: {
    _id: 'artist:18563',
    last_name: 'Lautner',
    first_name: 'Georges',
    birth_date: 1926
  },
  ...
}
```

### 9. Afficher les titres et les grades des films d'action réalisés en France
```js
db.films.find({ genre: "Action", country: "FR" }, { _id: 0, title: 1, grades: 1 })
```
Résultat :
```js
{
  title: "L'Homme de Rio",
  grades: [
    {
      note: 4,
      grade: 'D'
    },
    {
      note: 30,
      grade: 'E'
    },
    {
      note: 34,
      grade: 'E'
    },
    {
      note: 28,
      grade: 'F'
    }
  ]
}
...
```

### 10. Les filtres affichés jusqu’à présent sont par valeur exacte. Afficher les titres et les notes des films d’action réalisés en France et ayant obtenus une note supérieur à 10. Remarquez que même si des films ont obtenu certaines notes inférieures à 10 sont affichées.
```js
db.films.find({ genre: "Action", country: "FR", "grades.note": { $gt: 10 } }, { _id: 0, title: 1, "grades.note": 1 })
```
Résultat :
```js
{
  title: "L'Homme de Rio",
  grades: [
    {
      note: 4
    },
    {
      note: 30
    },
    {
      note: 34
    },
    {
      note: 28
    }
  ]
}
...
```

### 11. Si l’on souhaite afficher que des films ayant que des notes supérieures à 10, on doit préciser que les notes obtenues sont toutes supérieures à 40. La requête suivante permet d’afficher les films d’action réalisés en France ayant obtenus que des notes supérieures à 10.
```js
db.films.find({ genre: "Action", country: "FR", "grades.note": { $not: { $lte: 10 } } }, { _id: 0, title: 1, "grades.note": 1 })
```
Résultat :
```js
{
  title: 'Nikita',
  grades: [
    {
      note: 88
    },
    {
      note: 36
    },
    {
      note: 74
    },
    {
      note: 62
    }
  ]
}
{
  title: 'Les tontons flingueurs',
  grades: [
    {
      note: 35
    },
    {
      note: 48
    },
    {
      note: 64
    },
    {
      note: 42
    }
  ]
}
```

### 12. Afficher les différents genres de la base lesfilms.
```js
db.films.distinct("genre")
```
Résultat :
```js
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
### 13. Afficher les différents grades attribués.
```js
db.films.distinct("grades.grade")
```
Résultat :
```js
[ 'A', 'B', 'C', 'D', 'E', 'F' ]
```

### 14. Afficher tous les films dans lesquels joue au moins un des artistes suivants : [”artist:4”, ”artist:18”, ”artist:11”].
```js
db.films.find({ actors: { $in: ["artist:4", "artist:18", "artist:11"] } })
```
Résultat :
```js
```

### 15. Afficher tous les films qui n’ont pas de résumé.
```js
db.films.find({ $or: [ { summary: { $exists: false } }, { summary: "" } ] })
```
Résultat :
```js
{
  _id: 'movie:181812',
  title: 'Star Wars, épisode IX',
  year: 2019,
  genre: 'Science-Fiction',
  summary: '',
  country: 'GB',
  ...
}
```

### 16. Afficher tous les films tournés avec Leonardo DiCaprio en 1997.
```js
db.films.find({ "actors.last_name": "DiCaprio", "actors.first_name": "Leonardo", year: 1997 })
```
Résultat :
```js
{
  _id: 'movie:597',
  title: 'Titanic',
  year: 1997,
  genre: 'Drame',
  summary: 'Southampton, 10 avril 1912. Le paquebot le plus grand et le plus moderne du monde, réputé pour son insubmersibilité, le « Titanic », appareille pour son premier voyage. 4 jours plus tard, il heurte un iceberg. À son bord, un artiste pauvre et une grande bourgeoise tombent amoureux.',
  country: 'US',
  ...
}
```

### 17. Afficher les films tournés avec Leonardo DiCaprio ou 1997.
```js
db.films.find({ $or: [{ "actors.last_name": "DiCaprio", "actors.first_name": "Leonardo" }, { year: 1997 }] })
```
Résultat :
```js
{
  _id: 'movie:281957',
  title: 'The Revenant',
  year: 2015,
  genre: 'Western',
  summary: 'Dans une Amérique profondément sauvage, le trappeur Hugh Glass est sévèrement blessé et laissé pour mort par un traître de son équipe, John Fitzgerald. Avec sa seule volonté pour unique arme, Glass doit affronter un environnement hostile, un hiver brutal et des tribus guerrières, dans une inexorable lutte pour sa survie, portée par un intense désir de vengeance. [ le film est le remake de :  Le Convoi sauvage (1971)  ]',
  country: 'CA',
  director: {
    _id: 'artist:223',
    last_name: 'González Iñárritu',
    first_name: 'Alejandro',
    birth_date: 1963
  },
  actors: [
    {
      last_name: 'Hardy',
      first_name: 'Tom',
      birth_date: 1977
    },
    {
      last_name: 'DiCaprio',
      first_name: 'Leonardo',
      birth_date: 1974
    },
    ...
  ]
  ...
}
```

## Conclusion
MongoDB est un outil puissant pour stocker et manipuler des données non structurées. Ce tutoriel vous permet d'effectuer des opérations de base sur une collection de films et d'explorer les capacités de MongoDB.
