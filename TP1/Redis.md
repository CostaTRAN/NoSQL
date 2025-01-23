# Redis et les bases de données NoSQL : Un guide complet

## Introduction aux bases de données NoSQL

Les **bases de données NoSQL** (Not Only SQL) sont des systèmes de gestion de bases de données conçus pour répondre à des besoins spécifiques que les bases de données relationnelles traditionnelles ne peuvent pas satisfaire efficacement. Elles sont particulièrement adaptées pour gérer de grandes quantités de données non structurées ou semi-structurées, et pour offrir une haute disponibilité et une évolutivité horizontale.

### Les quatre familles principales de bases de données NoSQL

1. **Bases de données clé-valeur** : Ces bases de données stockent les données sous forme de paires clé-valeur. Elles sont simples et rapides, idéales pour des opérations de lecture et d'écriture rapides. Exemples : Redis, DynamoDB.

2. **Bases de données documentaires** : Ces bases de données stockent les données sous forme de documents, souvent au format JSON ou BSON. Elles sont flexibles et permettent de stocker des données hiérarchiques. Exemples : MongoDB, CouchDB.

3. **Bases de données colonnes** : Ces bases de données stockent les données dans des colonnes plutôt que dans des lignes. Elles sont optimisées pour les opérations de lecture et d'écriture sur de grandes quantités de données. Exemples : Cassandra, HBase.

4. **Bases de données orientées graphes** : Ces bases de données sont conçues pour stocker et manipuler des données sous forme de graphes, avec des nœuds, des arêtes et des propriétés. Elles sont idéales pour les applications nécessitant des relations complexes entre les données. Exemples : Neo4j, Amazon Neptune.

## Redis : Une base de données clé-valeur performante

### Définition

Redis (Remote Dictionary Server) est une base de données clé-valeur open-source, écrite en C. Elle est souvent utilisée comme un magasin de données en mémoire, un cache et un agent de messagerie. Redis prend en charge divers types de données, notamment les chaînes de caractères, les listes, les ensembles, les ensembles ordonnés, les hachages, les bits, les hyperloglogs et les géospatiaux.

### Caractéristiques de Redis
- Performance : Redis est extrêmement rapide car il fonctionne en mémoire.
- Flexibilité : Il prend en charge une variété de types de données et de structures.
- Persistance : Bien que principalement en mémoire, Redis offre des options de persistance pour sauvegarder les données sur disque.
- Réplication : Redis prend en charge la réplication maître-esclave pour la haute disponibilité.
- Évolutivité : Redis peut être mis à l'échelle horizontalement en utilisant le partitionnement (sharding).

### Installation de Redis

#### Sur Linux (Ubuntu/Debian)

```bash
# Mettre à jour les paquets
sudo apt update

# Installer Redis
sudo apt install redis
```

### Utilisation de Redis
#### Démarrer le serveur Redis

```bash
redis-server
```

#### Ouvrir le client Redis
```bash
redis-cli
```

### Commandes de Base
#### Opérations CRUD
Les opérations CRUD (Create, Read, Update, Delete) sont fondamentales dans toute base de données. Voici comment les réaliser avec Redis :
##### Create (Créer) :
```bash
SET demo "Bonjour"
```
##### Read (Lire) :
```bash
GET demo
```
##### Update (Mettre à jour) :
```bash
SET demo "Bonjour, monde!"
```
##### Delete (Supprimer) :
```bash
DEL demo
```

### Exemples de Commandes
#### Définir une valeur pour une clé :
```bash
set user:1234 "Samir"
```

#### Récupérer une valeur par sa clé :
```bash
get user:1234
```

#### Supprimer une clé :
```bash
del user:1234
```

#### Incrémenter une valeur :
```bash
set 1mars 0
incr 1mars
incr 1mars
incr 1mars
incr 1mars
incr 1mars
```

#### Décrémenter une valeur :
```bash
decr 1mars
decr 1mars
decr 1mars
```

#### Définir une clé avec une valeur et vérifier son temps de vie (TTL) :
```bash
set macle mavaleur
ttl macle
-1
```
#### Définir une durée d'expiration pour une clé :

```bash
expire macle 120
ttl macle
```
#### Supprimer une clé :
```bash
del macle
```

### Opérations sur les Listes
#### Ajouter des éléments à une liste :
```bash
RPUSH mesCours "BDA"
RPUSH mesCours "Services Web"
```
#### Récupérer tous les éléments d'une liste :
```bash
LRANGE mesCours 0 -1
```
#### Récupérer le premier élément d'une liste :
```bash
LRANGE mesCours 0 0
```

#### Récupérer le deuxième élément d'une liste :
```bash
LRANGE mesCours 1 1
```

#### Supprimer le premier élément d'une liste :
```bash
LPOP mesCours
```

#### Ajouter plusieurs éléments à une liste :
```bash
RPUSH mesCours "Services Web"
RPUSH mesCours "Services Web"
RPUSH mesCours "Services Web"
RPUSH mesCours "Services Web"
RPUSH mesCours "Services Web"
```

### Opérations sur les Ensembles
#### Ajouter des éléments à un ensemble :
```bash
SADD utilisateurs "Augustin"
SADD utilisateurs "Ines"
SADD utilisateurs "Samir"
SADD utilisateurs "Marc"
SADD utilisateurs "Marc"
```
#### Récupérer tous les éléments d'un ensemble :
```bash
SMEMBERS utilisateurs
```

#### Supprimer un élément d'un ensemble :
```bash
SREM utilisateurs "Marc"
```
#### Union de deux ensembles :
```bash
SADD autresUtilisateurs "Antoine"
SADD autresUtilisateurs "Philippe"
SUNION utilisateurs autresUtilisateurs
```

### Ensembles ordonnés
Les ensembles ordonnés sont des ensembles où chaque élément est associé à un score. Les éléments sont triés par leur score, ce qui permet des opérations de tri et de classement efficaces.

#### Ajouter des éléments à un ensemble ordonné avec des scores :
```bash
ZADD score4 19 "Augustin"
ZADD score4 18 "Ine"
ZADD score4 10 "Samir"
ZADD score4 8 "Philippe"
```
#### Récupérer tous les éléments d'un ensemble ordonné par score croissant :
```bash
ZRANGE score4 0 -1
```

#### Récupérer les deux premiers éléments d'un ensemble ordonné par score croissant :
```bash
ZRANGE score4 0 1
```

#### Récupérer tous les éléments d'un ensemble ordonné par score décroissant :
```bash
ZREVRANGE score4 0 -1
```
#### Obtenir le rang d'un élément dans un ensemble ordonné :
```bash
ZRANK score4 "Augustin"
```

### Opérations sur les Hachages
Les hachages sont des structures de données qui permettent de stocker des paires clé-valeur. Ils sont utiles pour représenter des objets avec plusieurs champs.

#### Définir plusieurs champs pour un hachage :
```bash
HSET user:11 username "syoucef"
HSET user:11 age 31
HSET user:11 email samir.youcef@polytechnancy.fr
```

#### Récupérer tous les champs et valeurs d'un hachage :
```bash
HGETALL user:11
```

#### Définir plusieurs champs pour un hachage en une seule commande :
```bash
HMSET user:4 username "Augustin" age 5 email augustin@gmail.fr
```

#### Récupérer tous les champs et valeurs d'un hachage :
```bash
HGETALL user:4
```

#### Incrémenter la valeur d'un champ dans un hachage :
```bash
HINCRBY user:4 age 4
```
#### Récupérer la valeur d'un champ spécifique dans un hachage :
```bash
HGET user:4 age
```
#### Récupérer toutes les valeurs d'un hachage :
```bash
HVALS user:4
```

### Opérations de Pub/Sub (Publication/Abonnement)
Redis supporte un modèle de messagerie de type publication/abonnement (Pub/Sub). Ce modèle permet à un client de s'abonner à un ou plusieurs canaux et de recevoir des messages publiés sur ces canaux par d'autres clients.

#### S'abonner à un ou plusieurs canaux :
```bash
SUBSCRIBE mescours user:1
```

#### Publier un message sur un canal :
```bash
PUBLISH mescours "Un nouveau cours sur MongoDB"
PUBLISH user:1 "Bonjour user1"
```

#### S'abonner à un motif de canaux (tous les canaux commençant par "mes") :
```bash
PSUBSCRIBE mes*
```

#### Publier des messages sur différents canaux :
```bash
PUBLISH mesnotes "Une nouvelle note est arrivée!"
PUBLISH notes "Une nouvelle note est arrivée!"
```

### Opérations sur les Bases de Données
Redis permet de sélectionner différentes bases de données (par défaut, il y en a 16) et de lister les clés dans chaque base de données.

#### Sélectionner la base de données 1 et lister toutes les clés :
```bash
SELECT 1
KEYS *
```

## Fonctionnalités Avancées de Redis
### 1. **Réplication**
Redis supporte la réplication maître-esclave, ce qui permet de créer des copies de la base de données principale (maître) sur une ou plusieurs bases de données secondaires (esclaves). Cela assure une haute disponibilité et une tolérance aux pannes.
#### Configuration de la Réplication :
Pour configurer un esclave Redis, vous devez modifier le fichier de configuration redis.conf de l'esclave pour inclure l'adresse IP et le port du maître.
```bash
slaveof <maître_ip> <maître_port>
```

#### Vérification de la Réplication :
Vous pouvez vérifier l'état de la réplication en utilisant la commande INFO :
```bash
INFO replication
```

### 2. Persistance
Redis offre plusieurs options de persistance pour sauvegarder les données sur disque, garantissant ainsi la durabilité des données en cas de redémarrage du serveur.

#### RDB (Redis Database File) :
Redis peut sauvegarder des instantanés de la base de données à des intervalles réguliers.
```bash
SAVE
```
ou
```bash
BGSAVE
```

#### AOF (Append-Only File) :
Redis peut enregistrer chaque opération de modification dans un fichier journal, ce qui permet une restauration plus fine.
```bash
CONFIG SET appendonly yes
```

### 3. Transactions
Redis supporte les transactions, permettant d'exécuter plusieurs commandes de manière atomique. Cela garantit que toutes les commandes dans une transaction sont exécutées sans interruption.

#### Début de la Transaction :
```bash
MULTI
```

#### Commandes dans la Transaction :
```bash
SET user:1001 "Alice"
INCR user:1001:login_count
```

#### Exécution de la Transaction :
```bash
EXEC
```

#### Annulation de la Transaction :
```bash
DISCARD
```

### 4. Scripts Lua
Redis permet l'exécution de scripts Lua directement sur le serveur, offrant ainsi une flexibilité accrue pour les opérations complexes.

#### Chargement et Exécution d'un Script Lua :
```bash
EVAL "return redis.call('SET', KEYS[1], ARGV[1])" 1 "mykey" "myvalue"
```

#### Exemple de Script Lua :
```lua
local value = redis.call('GET', KEYS[1])
value = tonumber(value) + 1
redis.call('SET', KEYS[1], value)
return value
```

#### Exécution du Script :
```bash
EVAL "local value = redis.call('GET', KEYS[1]); value = tonumber(value) + 1; redis.call('SET', KEYS[1],
```

## Conclusion
**Redis** est une base de données clé-valeur puissante et flexible, idéale pour les applications nécessitant des performances élevées et une faible latence. Avec ses divers types de données et ses fonctionnalités avancées, Redis est un choix populaire pour le caching, les sessions utilisateur, les files d'attente de messages et bien plus encore.
