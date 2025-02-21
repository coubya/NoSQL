# Rapport de TP : Introduction et Utilisation de MongoDB

## 1. Introduction

MongoDB est une base de données **NoSQL orientée documents** conçue pour stocker et manipuler des données semi-structurées au format JSON/BSON. Elle offre une flexibilité et une scalabilité adaptées aux applications nécessitant un accès rapide aux données.

Ce rapport présente l’installation de MongoDB, l’importation de données et l’exécution des principales requêtes sur une base de films.

---

## 2. Installation et Configuration

### 2.1 Installation avec Docker
```bash
docker run --name mongodb -d -p 27017:27017 -v $(pwd)/data:/data/db mongo:latest
```
- `--name mongodb` : Nom du conteneur.
- `-d` : Exécution en arrière-plan.
- `-p 27017:27017` : Exposition du port 27017.
- `-v $(pwd)/data:/data/db` : Persistance des données.

### 2.2 Vérification du bon fonctionnement
```bash
docker ps
```

---

## 3. Importation des Données

### 3.1 Transférer le fichier JSON vers le conteneur
```bash
docker cp films.json mongodb:/films.json
```

### 3.2 Connexion au conteneur
```bash
docker exec -it mongodb bash
```

### 3.3 Importation des données dans MongoDB
```bash
mongoimport --db lesfilms --collection films --file /films.json --jsonArray
```

---

## 4. Requêtes MongoDB

### 4.1 Vérification de l’importation
```javascript
db.films.count()
```

### 4.2 Examiner la structure d’un document
```javascript
db.films.findOne()
```

### 4.3 Requêtes de base

#### a. Afficher les films d’action
```javascript
db.films.find({ genre: "Action" })
```

#### b. Compter les films d’action
```javascript
db.films.count({ genre: "Action" })
```

#### c. Films d’action produits en France
```javascript
db.films.find({ genre: "Action", country: "FR" })
```

#### d. Films d’action en France en 1963
```javascript
db.films.find({ genre: "Action", country: "FR", year: 1963 })
```

#### e. Exclure certains attributs (ex : grades)
```javascript
db.films.find({ genre: "Action", country: "FR" }, { grades: 0 })
```

### 4.4 Requêtes avancées

#### a. Afficher les titres et notes des films d’action en France
```javascript
db.films.find({ genre: "Action", country: "FR" }, { _id: 0, title: 1, grades: 1 })
```

#### b. Films avec au moins une note supérieure à 10
```javascript
db.films.find({ "grades.note": { $gt: 10 } })
```

#### c. Films où **toutes** les notes sont supérieures à 10
```javascript
db.films.find({ grades: { $not: { $elemMatch: { note: { $lte: 10 } } } } })
```

### 4.5 Requêtes sur les acteurs et années

#### a. Films avec Leonardo DiCaprio en 1997
```javascript
db.films.find({ actors: "Leonardo DiCaprio", year: 1997 })
```

#### b. Films avec Leonardo DiCaprio **ou** en 1997
```javascript
db.films.find({ $or: [ { actors: "Leonardo DiCaprio" }, { year: 1997 } ] })
```

---

## 5. Gestion du Conteneur MongoDB

### 5.1 Arrêter MongoDB
```bash
docker stop mongodb
```

### 5.2 Supprimer le conteneur
```bash
docker rm mongodb
```

---

## 6. Conclusion

MongoDB est une solution performante et flexible pour le stockage et la gestion de données semi-structurées. Grâce à sa syntaxe intuitive et à ses performances, il est utilisé dans de nombreuses applications nécessitant une base de données évolutive et rapide.
