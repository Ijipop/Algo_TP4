# Decorator (Décorateur)

Dans votre application, voulez-vous ajouter des fonctionnalités à un objet de manière dynamique et flexible, sans utiliser l'héritage et sans modifier l'objet original ?

Vous avez de la difficulté :
- Avec l'héritage qui crée une explosion de classes pour chaque combinaison
- À ajouter des fonctionnalités de manière flexible
- Avec un code rigide qui ne permet pas de combiner les fonctionnalités
- À maintenir le code quand vous ajoutez de nouvelles fonctionnalités

Typiquement applicable pour :
- Ajout d'effets à des armes (feu, poison, glace)
- Ajout de fonctionnalités à des composants UI (bordure, ombre, animation)
- Ajout de comportements à des objets (logging, cache, validation)
- Ajout de fonctionnalités à des services (authentification, compression)

## Code non optimisé

```python
class Epee: 
    def dmg(self): return 10

class EpeeAvecFeu(Epee):
    def dmg(self): return super().dmg() + 5

class EpeeFeuPoison(EpeeAvecFeu):
    def dmg(self): return super().dmg() + 3  # explosion de classes
```

> **Problème :** L'héritage crée une explosion de classes dès qu'on combine plusieurs effets.

## Solution : Utiliser le patron Decorator

Le patron Decorator permet d'ajouter dynamiquement des fonctionnalités à un objet en l'enveloppant dans des objets décorateurs. Chaque décorateur implémente la même interface que l'objet original, ce qui permet :

- D'ajouter des fonctionnalités de manière flexible et combinable
- D'éviter l'explosion de classes de l'héritage
- De composer les fonctionnalités à l'exécution
- De respecter le principe ouvert/fermé (ouvert à l'extension, fermé à la modification)

## Code optimisé

```python
class Arme:
    def dmg(self): return 0

class Epee(Arme):
    def dmg(self): return 10

class Buff(Arme):
    def __init__(self, base: Arme): self.base = base
    def dmg(self): return self.base.dmg()

class Feu(Buff):
    def dmg(self): return self.base.dmg() + 5

class Poison(Buff):
    def dmg(self): return self.base.dmg() + 3

# Utilisation
arme = Poison(Feu(Epee()))
print(arme.dmg())  # 18 de dommage
```
