# Redis Tutorial Guide

## Basic Key-Value Operations

### Setting and Getting Simple Keys
```bash
# Set a simple key-value pair
SET demo "Bonjour"

# Set a key with a user identifier
SET user:1234 "Samir"

# Delete a specific key
DEL user:1234
```
> *Note*: These commands demonstrate basic key-value storage and deletion in Redis.

### Numeric Incrementation and Decrementation
```bash
# Set a numeric key and initialize
SET 1mars 0

# Increment a numeric key
INCR 1mars  # Increases value to 1, 2, 3, 4, 5

# Decrement a numeric key
DECR 1mars  # Reduces value to 4, 3
```
> *Note*: `INCR` and `DECR` are atomic operations useful for counters and unique identifiers.

## Key Expiration

### Setting Expiration on Keys
```bash
# Set a key with a value
SET macle "mavaleur"

# Check time-to-live (TTL) initially
TTL macle  # Returns -1 (no expiration)

# Set expiration to 120 seconds
EXPIRE macle 120  # Returns 1 (success)

# Check remaining TTL
TTL macle  # Returns remaining seconds

# Delete the key
DEL macle
```
> *Note*: Expiration is useful for temporary data like cache or session management.

## List Operations

### Creating and Manipulating Lists
```bash
# Push items to the right of a list
RPUSH mescours "BDA"
RPUSH mescours "Services Web"

# Retrieve list contents
LRANGE mescours 0 -1  # Show all items
LRANGE mescours 0 0   # Show first item
LRANGE mescours 1 1   # Show second item

# Remove and return the first item
LPOP mescours

# Multiple list operations
RPUSH mescours "Services Web"  # Add multiple times
```
> *Note*: Lists in Redis are ordered collections allowing efficient push/pop operations.

## Set Operations

### Managing Sets
```bash
# Add unique members to a set
SADD utilisateur "Augustin"
SADD utilisateur "Ines"
SADD utilisateur "Samir"
SADD utilisateur "Marc"

# Show all set members
SMEMBERS utilisateur

# Remove a member from a set
SREM utilisateur "Marc"

# Create another set
SADD autreUtilisateurs "Antoine"
SADD autreUtilisateurs "Philippe"

# Union of two sets
SUNION utilisateur autreUtilisateurs
```
> *Note*: Sets store unique, unordered elements and support set operations like union.

## Common Pitfalls and Tips

- Always specify both key and value for `SET`
- Be careful with key names (avoid spaces, special characters)
- Use appropriate data structures for your use case
- Remember that Redis is an in-memory database

## Recommended Practices

1. Use meaningful key namespaces (e.g., `user:1234`)
2. Leverage Redis data structures for efficient data management
3. Be mindful of memory usage with large datasets
4. Use expiration for temporal data

## Learning Resources

- [Official Redis Documentation](https://redis.io/documentation)
- [Redis Command Reference](https://redis.io/commands)
