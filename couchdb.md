# Rapport de TP : Introduction et Utilisation de CouchDB

## 1. Introduction

Apache CouchDB est une base de données **NoSQL orientée documents**, reposant sur JSON pour le stockage des données, HTTP pour l'accès et **MapReduce** pour les requêtes. Sa flexibilité et sa capacité de réplication le rendent particulièrement adapté aux applications web et mobiles.

Ce rapport couvre l’installation de CouchDB, la manipulation de documents, ainsi que l’utilisation de MapReduce pour des requêtes avancées.

---

## 2. Installation et Configuration

### 2.1 Installation avec Docker
```bash
docker run -d --name couchdb \
  -e COUCHDB_USER=admin \
  -e COUCHDB_PASSWORD=secret \
  -p 5984:5984 \
  couchdb:latest
```
- `COUCHDB_USER` et `COUCHDB_PASSWORD` définissent les identifiants d’administration.
- `-p 5984:5984` expose CouchDB sur le port 5984.

### 2.2 Vérification du bon fonctionnement
```bash
curl -X GET http://admin:secret@localhost:5984/
```
Une réponse contenant `{"couchdb":"Welcome"}` confirme le bon fonctionnement.

---

## 3. Manipulation des Données

### 3.1 Création d’une base de données
```bash
curl -X PUT http://admin:secret@localhost:5984/films
```

### 3.2 Ajout de documents

#### Ajout d’un document unique
```bash
curl -X POST http://admin:secret@localhost:5984/films \
  -H "Content-Type: application/json" \
  -d '{"title":"Inception", "year":2010}'
```

#### Ajout en masse
```bash
curl -X POST http://admin:secret@localhost:5984/films/_bulk_docs \
  -H "Content-Type: application/json" \
  -d @films.json
```

### 3.3 Récupération et mise à jour de documents

#### Lire un document
```bash
curl -X GET http://admin:secret@localhost:5984/films/doc_id
```

#### Modifier un document existant
```bash
curl -X PUT http://admin:secret@localhost:5984/films/doc_id \
  -H "Content-Type: application/json" \
  -d '{"_rev":"1-xxx", "title":"Interstellar", "year":2014}'
```

---

## 4. Utilisation de MapReduce

### 4.1 Vue 1 : Nombre de films par année

#### Fonction Map
```javascript
function (doc) {
  if (doc.year && doc.title) {
    emit(doc.year, 1);
  }
}
```

#### Fonction Reduce
```javascript
function (keys, values, rereduce) {
  return sum(values);
}
```

### 4.2 Vue 2 : Films par acteur

#### Fonction Map
```javascript
function (doc) {
  if (doc.actors) {
    doc.actors.forEach(actor => {
      emit(actor.name, doc.title);
    });
  }
}
```

#### Fonction Reduce
```javascript
function (keys, values) {
  return { count: values.length, films: values };
}
```

### 4.3 Vue 3 : Films par réalisateur

#### Fonction Map
```javascript
function (doc) {
  if (doc.director) {
    emit(doc.director, doc.title);
  }
}
```

#### Fonction Reduce
```javascript
function (keys, values) {
  return values.length;
}
```

---

## 5. Conclusion

CouchDB est une base de données NoSQL puissante et flexible, se démarquant par son API RESTful et son moteur de requêtes MapReduce. Grâce à ses capacités de réplication et d’agrégation, il est particulièrement adapté aux applications web et mobiles nécessitant un stockage efficace et une synchronisation fiable des données.
