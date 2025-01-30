# Tutoriel MongoDB - Prise en main

## Introduction à MongoDB
MongoDB est une base de données NoSQL orientée documents qui stocke les données sous forme de documents JSON. Il est utilisé pour sa flexibilité et ses performances élevées dans la gestion des données massives.

## Installation de MongoDB

### Sous Windows
1. Téléchargez MongoDB depuis le site officiel : [MongoDB Download](https://www.mongodb.com/try/download/community)
2. Installez MongoDB en suivant les instructions fournies.
3. Ajoutez MongoDB au `PATH` de votre système pour pouvoir l'utiliser en ligne de commande.
4. Démarrez MongoDB avec la commande :
   ```bash
   mongod --dbpath "C:\\data\\db"
   ```

### Sous Linux
1. Installez MongoDB via le gestionnaire de paquets :
   ```bash
   sudo apt update
   sudo apt install -y mongodb
   ```
2. Démarrez le service MongoDB :
   ```bash
   sudo systemctl start mongod
   ```

### Sous macOS
1. Installez MongoDB via Homebrew :
   ```bash
   brew tap mongodb/brew
   brew install mongodb-community@5.0
   ```
2. Démarrez le service MongoDB :
   ```bash
   brew services start mongodb-community@5.0
   ```

## Importation de la collection de films
Pour importer une collection de films dans MongoDB, utilisez la commande suivante :
```bash
mongoimport --db lesfilms --collection films --file films.json --jsonArray
```
Cette commande importe le fichier `films.json` dans la base de données `lesfilms`, en créant une collection `films`.

## Vérification des données importées
```javascript
db.films.count()
```
Cette commande retourne le nombre de documents présents dans la collection `films`.

## Affichage d'un document de la collection
```javascript
db.films.findOne()
```
Cette commande affiche un document aléatoire de la collection `films` pour en voir la structure.

## Requêtes courantes sur les films

### 1. Liste des films d'action
```javascript
db.films.find({ genre: "Action" })
```
Affiche tous les films ayant pour genre "Action".

### 2. Nombre de films d'action
```javascript
db.films.count({ genre: "Action" })
```
Compte le nombre de films d'action.

### 3. Films d'action produits en France
```javascript
db.films.find({ genre: "Action", pays: "France" })
```
Affiche les films d'action produits en France.

### 4. Films d'action produits en France en 1963
```javascript
db.films.find({ genre: "Action", pays: "France", annee: 1963 })
```
Affiche les films d'action produits en France en 1963.

### 5. Films d'action réalisés en France sans les grades
```javascript
db.films.find({ genre: "Action", pays: "France" }, { grades: 0 })
```
Affiche les films d'action réalisés en France sans inclure les notes.

### 6. Films d'action réalisés en France sans identifiants
```javascript
db.films.find({ genre: "Action", pays: "France" }, { _id: 0 })
```
Affiche les films d'action réalisés en France sans afficher l'identifiant `_id`.

### 7. Titres et grades des films d'action réalisés en France
```javascript
db.films.find({ genre: "Action", pays: "France" }, { _id: 0, titre: 1, grades: 1 })
```
Affiche uniquement les titres et les notes des films d'action réalisés en France.

### 8. Films d'action réalisés en France avec une note supérieure à 10
```javascript
db.films.find({ genre: "Action", pays: "France", "grades.note": { $gt: 10 } }, { _id: 0, titre: 1, "grades.note": 1 })
```
Affiche les films d'action réalisés en France ayant au moins une note supérieure à 10.

### 9. Films d'action réalisés en France ayant uniquement des notes supérieures à 10
```javascript
db.films.find({ genre: "Action", pays: "France", "grades.note": { $not: { $lte: 10 } } }, { _id: 0, titre: 1, "grades.note": 1 })
```
Affiche les films d'action réalisés en France qui ont uniquement des notes supérieures à 10.

### 10. Différents genres de films disponibles
```javascript
db.films.distinct("genre")
```
Affiche tous les genres de films distincts dans la base de données.

### 11. Différents grades attribués
```javascript
db.films.distinct("grades")
```
Affiche toutes les notes distinctes attribuées aux films.

### 12. Films avec certains artistes
```javascript
db.films.find({ artistes: { $in: ["artist:4", "artist:18", "artist:11"] } })
```
Affiche tous les films dans lesquels jouent au moins un des artistes spécifiés.

### 13. Films sans résumé
```javascript
db.films.find({ resume: { $exists: false } })
```
Affiche tous les films qui ne possèdent pas de résumé.

### 14. Films avec Leonardo DiCaprio en 1997
```javascript
db.films.find({ artistes: "Leonardo DiCaprio", annee: 1997 })
```
Affiche tous les films dans lesquels joue Leonardo DiCaprio en 1997.

### 15. Films avec Leonardo DiCaprio ou tournés en 1997
```javascript
db.films.find({ $or: [{ artistes: "Leonardo DiCaprio" }, { annee: 1997 }] })
```
Affiche tous les films qui ont été tournés en 1997 ou qui incluent Leonardo DiCaprio.

## Conclusion
MongoDB est un outil puissant pour stocker et manipuler des données non structurées. Ce tutoriel vous permet d'effectuer des opérations de base sur une collection de films et d'explorer les capacités de MongoDB.
