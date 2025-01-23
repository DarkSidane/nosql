# Documentation Redis - Système de Publication/Souscription

## Démarrage du serveur (Terminal 1)

```bash
redis-server
```
Cette commande démarre le serveur Redis en mode standalone sur le port par défaut 6379. Le serveur affiche sa version (7.2.7) et est prêt à accepter les connexions.

## Configuration du Subscriber (Terminal 2)

### Souscription aux canaux spécifiques
```bash
SUBSCRIBE mescours user:1
```
Cette commande permet de souscrire à deux canaux distincts :
- `mescours` : Canal pour les notifications de nouveaux cours
- `user:1` : Canal spécifique à l'utilisateur 1

Le système confirme la souscription avec des messages de type :
1. "subscribe"
2. nom du canal
3. nombre total de souscriptions actives

### Souscription avec pattern matching
```bash
PSUBSCRIBE mes*
```
Cette commande souscrit à tous les canaux commençant par "mes". Elle permet de recevoir les messages de :
- `mescours`
- `mesnotes`
- et tout autre canal commençant par "mes"

## Publication des messages (Terminal 3)

### Connexion et publication
```bash
redis-cli
```
Connection au client Redis en ligne de commande.

### Publication des messages
```bash
PUBLISH mescours "Un nouveau cours sur MongoDB"
PUBLISH user:1 "Bonjour user 1!"
PUBLISH mesnotes "Une nouvelle note est arrivée !"
```
Ces commandes publient des messages sur différents canaux :
- Le premier message est reçu par les souscripteurs du canal `mescours`
- Le deuxième message est reçu par les souscripteurs du canal `user:1`
- Le troisième message est reçu par les souscripteurs du pattern `mes*`

### Gestion des bases de données
```bash
SELECT 0
SELECT 1
```
Ces commandes permettent de basculer entre différentes bases de données Redis :
- `SELECT 0` : Base de données par défaut
- `SELECT 1` : Base de données vide

### Liste des clés
```bash
KEYS *
```
Cette commande affiche toutes les clés présentes dans la base de données actuelle.
Dans la base 0, on trouve 8 clés :
1. "user:11"
2. "demo"
3. "1mars"
4. "autreUtilisateurs"
5. "user:4"
6. "score4"
7. "utilisateur"
8. "mescours"

## Observations importantes

1. Le système Pub/Sub de Redis fonctionne en temps réel
2. Les messages sont distribués immédiatement aux souscripteurs
3. Les patterns de souscription (PSUBSCRIBE) permettent une grande flexibilité
4. Redis maintient plusieurs bases de données indépendantes (SELECT)
5. Les messages ne sont pas persistants : si un client n'est pas connecté au moment de la publication, il ne recevra pas les messages précédents
