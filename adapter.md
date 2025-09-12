# Adapter (Adaptateur)

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

pad = Gamepad(); kb = KeyboardAPI()
input_adapte = GamepadAsKeyboard(pad, kb)
input_adapte.tick()  
```
