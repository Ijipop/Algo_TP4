# State (État)

Dans votre application, avez-vous un objet qui change de comportement selon son état interne (ex. : un personnage de jeu qui peut être en marche, en course, en saut, etc.) ?

Vous avez de la difficulté :
- Avec trop de conditions if/elif ou switch/case
- À maintenir le code quand vous ajoutez de nouveaux états
- Avec un code difficile à lire et à déboguer
- Car le comportement est dispersé dans plusieurs endroits

Typiquement applicable pour :
- États d'un personnage de jeu (idle, attaque, défense, mort)
- États d'un lecteur multimédia (lecture, pause, arrêt)
- États d'une commande (en attente, en cours, terminée)
- États d'une connexion réseau (connecté, déconnecté, en reconnexion)

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

## Solution : Utiliser le patron State

Le patron State permet de gérer les différents états d'un objet en encapsulant chaque état dans une classe séparée. L'objet délègue son comportement à l'état courant, ce qui permet :

- D'éliminer les conditions complexes
- De faciliter l'ajout de nouveaux états
- De centraliser la logique de chaque état
- D'améliorer la lisibilité et la maintenabilité

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

# Utilisation
h = Heros(Idle()); h.attaquer()
h.set_etat(Rage()); h.attaquer()
```
