# Adapter (Adaptateur)

Dans votre application, devez-vous utiliser une classe ou un service existant qui a une interface incompatible avec ce que votre code attend ?

Vous avez de la difficulté :
- Avec des interfaces incompatibles entre différents composants
- À intégrer des bibliothèques tierces avec des interfaces différentes
- Avec du code qui doit être réécrit pour chaque nouveau composant
- À maintenir la cohérence entre différents systèmes

Typiquement applicable pour :
- Intégration de bibliothèques tierces avec des interfaces différentes
- Adaptation d'APIs existantes à de nouveaux besoins
- Intégration de systèmes legacy avec du code moderne
- Conversion entre différents formats de données

## Code non optimisé

```python
# Le moteur attend KeyboardAPI.up()/down()/left()/right()
class Gamepad:
    def axis(self): return (1, 0)

def bouger(moteur, input_device):
    x, y = input_device.axis()
    if x > 0: moteur.right()
    
```

> **Problème :** Chaque nouvel appareil oblige à réécrire du code spécifique un peu partout.

## Solution : Utiliser le patron Adapter

Le patron Adapter permet de faire fonctionner des classes avec des interfaces incompatibles en créant une classe intermédiaire qui traduit les appels. L'adaptateur agit comme un pont entre deux interfaces, ce qui permet :

- D'intégrer des composants existants sans les modifier
- De réutiliser du code legacy avec du code moderne
- De découpler les interfaces incompatibles
- De faciliter l'intégration de bibliothèques tierces

## Code optimisé

```python
class KeyboardAPI:
    def up(self): print("↑")
    def down(self): print("↓")
    def left(self): print("←")
    def right(self): print("→")

class Gamepad:
    def axis(self): return (1, 0)  # x, y

class GamepadAsKeyboard:  # Adaptateur
    def __init__(self, pad: Gamepad, kb: KeyboardAPI):
        self.pad, self.kb = pad, kb
    def tick(self):
        x, y = self.pad.axis()
        if y < 0: self.kb.up()
        if y > 0: self.kb.down()
        if x < 0: self.kb.left()
        if x > 0: self.kb.right()

# Utilisation
pad = Gamepad(); kb = KeyboardAPI()
input_adapte = GamepadAsKeyboard(pad, kb)
input_adapte.tick()  
```
