# Problème A

Dans votre application, avez-vous un objet partagé donc vous voulez qu'il existe seulement une instance?

Vous avez de la difficulté :

- Avec des conflits de données
- Avec une surcharge mémoire
- Avec des comportements imprévisibles
- Avec des performances dégradées

Typiquement applicable pour:

- Un gestionnaire de configuration
- Un logger
- Une connexion à une base de données
- Un client API

## Solution : Utiliser le patron Singleton

Le patron Singleton assure qu'une classe ne peut posséder qu'une seule instance.
Chaque appel à cette classe renvoie à la même instance partagée, ce qui permet :

- Une centralisation du contrôle
- Une meilleure performance
- Une cohérence dans l’état de l’application

## Exemple de code

```python
class Configuration:
    _instance = None
    _initialized = False

    def __new__(cls):
        if cls._instance is None:
            print("Chargement de la configuration...")
            cls._instance = super().__new__(cls)
        return cls._instance

    def __init__(self):
        if not self._initialized:
            self._initialized = True
            self.params = {"debug": True, "api_url": "https://api.exemple.com"}

    def get(self, key):
        return self.params.get(key)

config1 = Configuration()
config2 = Configuration()
print(config1 is config2)  # True, vu les deux sont exactement les mêmes
```
