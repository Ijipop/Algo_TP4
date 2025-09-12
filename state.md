# State (État)

## Code non optimal

```python
class Heros:
    def __init__(self): self.etat = "IDLE"
    def attaquer(self):
        if self.etat == "IDLE": print("Petit coup")
        elif self.etat == "RAGE": print("Gros coup critique!")
        elif self.etat == "STUN": print("Impossible de bouger…")
```

> **Problème :** Les if/elif deviennent vite lourds et difficiles à maintenir si on ajoute de nouveaux états.

## Code optimisé

```python
from abc import ABC, abstractmethod

class Etat(ABC):
    @abstractmethod
    def attaquer(self, heros): ...

class Idle(Etat):
    def attaquer(self, hero): print("Petit coup")

class Rage(Etat):
    def attaquer(self, hero): print("Gros coup critique!")

class Stun(Etat):
    def attaquer(self, hero): print("Impossible de bouger…")

class Heros:
    def __init__(self, etat: Etat): self.etat = etat
    def set_etat(self, etat: Etat): self.etat = etat
    def attaquer(self): self.etat.attaquer(self)

# Usage
h = Heros(Idle()); h.attaquer()
h.set_etat(Rage()); h.attaquer()
```
