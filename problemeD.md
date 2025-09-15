# Problème D

Dans votre application, avez-vous une classe qui effectue une tâche (ex. : Tri, calcul, affichage), mais vous voulez pouvoir changer dynamiquement son comportement sans modifier son code?

Vous avez de la difficulté :

- Avec trop de conditions if/else ou match/case
- Avec un code difficile à maintenir ou étendre
- Car c'est impossible de changer le comportement à l’exécution

Typiquement applicable pour:

- Différents algorithmes de tri
- Plusieurs politiques de tarification
- Divers formats d’export (PDF, CSV, HTML)

## Solution : Utiliser le patron Strategy

Le patron Strategy permet d’encapsuler des comportements interchangeables dans des classes séparées.
Vous pouvez injecter dynamiquement la strategie souhaite dans votre object principal, sans modifier sa structure. Ce qui permet :

- De séparer les algorithmes du code métier
- De rendre le comportement interchangeable à l’exécution
- De faciliter les tests et l’extension du système

## Exemple de code

```python
# Stratégies de tri
class TriStrategy:
    def trier(self, data): pass

class TriAscendant(TriStrategy):
    def trier(self, data): return sorted(data)

class TriDescendant(TriStrategy):
    def trier(self, data): return sorted(data, reverse=True)

class TriParLongueur(TriStrategy):
    def trier(self, data): return sorted(data, key=len)

# Contexte : liste de données
class Liste:
    def __init__(self, data, strategy: TriStrategy):
        self.data = data
        self.strategy = strategy

    def trier(self):
        return self.strategy.trier(self.data)

# Utilisation
liste1 = Liste(["pomme", "banane", "kiwi"], TriAscendant())
print(liste1.trier())  # ['banane', 'kiwi', 'pomme']

liste2 = Liste(["pomme", "banane", "kiwi"], TriParLongueur())
print(liste2.trier())  # ['kiwi', 'pomme', 'banane']

```
