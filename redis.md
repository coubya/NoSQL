# Rapport de TP : Introduction et Utilisation de Redis

## 1. Introduction

Les bases de données **NoSQL** (Not Only SQL) sont une alternative aux bases de données relationnelles traditionnelles. Elles offrent plus de flexibilité et une meilleure scalabilité pour des applications nécessitant un traitement rapide des données.

Parmi ces solutions, **Redis** (Remote Dictionary Server) est une base de données **clé-valeur en mémoire** offrant des performances exceptionnelles. Elle est souvent utilisée pour la mise en cache, la gestion de sessions, le classement et la messagerie en temps réel.

Ce rapport présente une introduction à Redis, son installation, les principales commandes et ses fonctionnalités avancées.

---

## 2. Installation et Configuration

### 2.1 Installation de Redis sur Ubuntu/Debian
```bash
sudo apt update
sudo apt install redis-server
```

### 2.2 Démarrage du serveur Redis
```bash
redis-server
```

### 2.3 Connexion au client Redis (CLI)
```bash
redis-cli
```
> Par défaut, Redis fonctionne sur l'adresse `127.0.0.1:6379`.

---

## 3. Manipulation des Structures de Données

Redis prend en charge plusieurs types de structures de données adaptées à différents cas d'usage.

### 3.1 Opérations de base sur les Clés

#### Définir une clé
```bash
SET utilisateur "Alice"
```

#### Lire une clé
```bash
GET utilisateur
```

#### Supprimer une clé
```bash
DEL utilisateur
```

#### Vérifier l'existence d'une clé
```bash
EXISTS utilisateur
```

### 3.2 Utilisation des Listes

Les listes permettent de stocker des séquences ordonnées d'éléments.

#### Ajouter des éléments
```bash
LPUSH ma_liste "A"
RPUSH ma_liste "B"
```

#### Lire une liste
```bash
LRANGE ma_liste 0 -1
```

#### Supprimer des éléments
```bash
LPOP ma_liste  # Retire le premier élément
RPOP ma_liste  # Retire le dernier élément
```

### 3.3 Gestion des Ensembles (Sets)

Les ensembles stockent des éléments uniques, sans ordre spécifique.

#### Ajouter des éléments
```bash
SADD mon_set "Alice" "Bob" "Charlie"
```

#### Vérifier la présence d'un élément
```bash
SISMEMBER mon_set "Alice"
```

#### Supprimer un élément
```bash
SREM mon_set "Bob"
```

### 3.4 Utilisation des Ensembles Ordonnés (Sorted Sets)

Les **Sorted Sets** permettent d'associer des éléments à des scores.

#### Ajouter des éléments avec score
```bash
ZADD classement 100 "Alice" 200 "Bob"
```

#### Récupérer les éléments triés
```bash
ZRANGE classement 0 -1 WITHSCORES
```

#### Récupérer les éléments en ordre inverse
```bash
ZREVRANGE classement 0 -1 WITHSCORES
```

### 3.5 Manipulation des Hashes

Les **hashes** permettent de stocker des paires clé-valeur organisées.

#### Ajouter des champs
```bash
HSET utilisateur:1 nom "Alice" age 30 email "alice@example.com"
```

#### Lire les champs d'un hash
```bash
HGETALL utilisateur:1
```

#### Incrémenter une valeur numérique
```bash
HINCRBY utilisateur:1 age 1
```

---

## 4. Fonctionnalités Avancées

### 4.1 Publication et Abonnement (Pub/Sub)
Redis permet la communication en temps réel via un modèle **Publish/Subscribe**.

#### Souscrire à un canal
```bash
SUBSCRIBE canal1
```

#### Publier un message
```bash
PUBLISH canal1 "Nouveau message"
```

### 4.2 Persistance des Données
Redis offre plusieurs options de persistance.

#### Sauvegarde périodique (Snapshots - RDB)
```bash
SAVE  # Sauvegarde immédiate
BGSAVE  # Sauvegarde en arrière-plan
```

#### Append-Only File (AOF) pour une récupération plus précise
```bash
CONFIG SET appendonly yes
```

---

## 5. Considérations de Performance

- Redis fonctionne principalement **en mémoire**, assurant une faible latence.
- La persistance via **snapshots RDB** minimise l'utilisation du CPU mais peut entraîner une perte de données récente.
- L'option **AOF** garantit une meilleure durabilité mais peut ralentir les performances.

---

## 6. Conclusion

Redis est un outil performant et flexible, adapté à diverses applications nécessitant une gestion rapide des données en temps réel. Grâce à ses structures variées et ses fonctionnalités avancées, il est utilisé pour la mise en cache, la gestion des sessions et la messagerie en temps réel. Bien que sa gestion en mémoire assure une vitesse élevée, il faut prendre en compte la consommation de mémoire et choisir une approche de persistance adaptée.
