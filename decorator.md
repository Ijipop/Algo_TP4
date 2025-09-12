# Decorator (Décorateur)

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

# Usage 
arme = Poison(Feu(Epee()))
print(arme.dmg())  # 18 de dommage
```
