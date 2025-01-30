# TP2 - Prise en main de MongoDB

## 1. Importation de la collection de films
```bash
mongoimport --db lesfilms --collection films --file films.json --jsonArray
```

## 2. Vérification des données importées
```javascript
db.films.count()
```

## 3. Affichage d'un document de la collection
```javascript
db.films.findOne()
```

## 4. Liste des films d'action
```javascript
db.films.find({ genre: "Action" })
```

## 5. Nombre de films d'action
```javascript
db.films.count({ genre: "Action" })
```

## 6. Films d'action produits en France
```javascript
db.films.find({ genre: "Action", pays: "France" })
```

## 7. Films d'action produits en France en 1963
```javascript
db.films.find({ genre: "Action", pays: "France", annee: 1963 })
```

## 8. Films d'action réalisés en France sans les grades
```javascript
db.films.find({ genre: "Action", pays: "France" }, { grades: 0 })
```

## 9. Films d'action réalisés en France sans identifiants
```javascript
db.films.find({ genre: "Action", pays: "France" }, { _id: 0 })
```

## 10. Titres et grades des films d'action réalisés en France sans identifiants
```javascript
db.films.find({ genre: "Action", pays: "France" }, { _id: 0, titre: 1, grades: 1 })
```

## 11. Films d'action réalisés en France avec une note supérieure à 10
```javascript
db.films.find({ genre: "Action", pays: "France", "grades.note": { $gt: 10 } }, { _id: 0, titre: 1, "grades.note": 1 })
```

## 12. Films d'action réalisés en France ayant uniquement des notes supérieures à 10
```javascript
db.films.find({ genre: "Action", pays: "France", "grades.note": { $not: { $lte: 10 } } }, { _id: 0, titre: 1, "grades.note": 1 })
```

## 13. Différents genres de la base lesfilms
```javascript
db.films.distinct("genre")
```

## 14. Différents grades attribués
```javascript
db.films.distinct("grades")
```

## 15. Films avec au moins un des artistes ["artist:4", "artist:18", "artist:11"]
```javascript
db.films.find({ artistes: { $in: ["artist:4", "artist:18", "artist:11"] } })
```

## 16. Films sans résumé
```javascript
db.films.find({ resume: { $exists: false } })
```

## 17. Films tournés avec Leonardo DiCaprio en 1997
```javascript
db.films.find({ artistes: "Leonardo DiCaprio", annee: 1997 })
```

## 18. Films tournés avec Leonardo DiCaprio ou en 1997
```javascript
db.films.find({ $or: [{ artistes: "Leonardo DiCaprio" }, { annee: 1997 }] })
```
