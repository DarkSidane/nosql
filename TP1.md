# Tutoriel Redis : Commandes de Base

## Configuration Initiale

### Démarrage du Serveur Redis
```bash
redis-server
```
- Lance le serveur Redis sur le port par défaut 6379
- Affiche les informations de configuration et de démarrage

### Connexion au Client Redis
```bash
redis-cli
```
- Ouvre une interface en ligne de commande pour interagir avec Redis

## Commandes de Chaînes (Strings)

### Définir une Valeur
```bash
SET demo "Bonjour"
SET user:1234 "Samir"
```
- `SET` crée ou met à jour une clé avec une valeur
- Permet d'utiliser des clés structurées comme `user:1234`

### Supprimer une Clé
```bash
DEL user:1234
```
- Supprime complètement une clé et sa valeur

## Manipulation de Nombres

### Incrémenter et Décrémenter
```bash
SET 1mars 0
INCR 1mars  # Résultat : 1, 2, 3, 4, 5
DECR 1mars  # Résultat : 4, 3
```
- `INCR` augmente la valeur de 1
- `DECR` diminue la valeur de 1

## Gestion de l'Expiration des Clés

### Définir une Expiration
```bash
SET macle mavaleur
EXPIRE macle 120  # Expire dans 120 secondes
TTL macle  # Affiche le temps restant
```
- `EXPIRE` définit un délai d'expiration pour une clé
- `TTL` (Time To Live) montre le temps restant avant expiration

## Listes

### Manipulation de Liste
```bash
RPUSH mescours "BDA"
RPUSH mescours "Services Web"
LRANGE mescours 0 -1  # Affiche tous les éléments
LPOP mescours  # Supprime le premier élément
```
- `RPUSH` ajoute un élément à la fin de la liste
- `LRANGE` permet de visualiser les éléments
- `LPOP` supprime et retourne le premier élément

## Ensembles (Sets)

### Gestion d'Ensembles
```bash
SADD utilisateur "Augustin" "Ines" "Samir" "Marc"
SMEMBERS utilisateur  # Liste tous les membres
SREM utilisateur "Marc"  # Supprime un élément
SADD autreUtilisateurs "Antoine" "Philippe"
SUNION utilisateur autreUtilisateurs  # Union de deux ensembles
```
- `SADD` ajoute des éléments uniques à un ensemble
- `SMEMBERS` affiche tous les éléments
- `SREM` supprime un élément
- `SUNION` combine deux ensembles sans doublons

## Ressources
https://www.youtube.com/watch?v=qeEY4giT3RM
