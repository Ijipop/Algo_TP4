# Observer (Observateur)

## Code non optimisé

```python
class VincentBoss:
    def __init__(self): self.hp = 100
    def tick(self):
        # ... dommages ...
        pass
```

> **Problème :** L'UI doit sonder sans arrêt l'état, ce qui gaspille et complique le code.

## Code optimisé

```python
class Sujet:
    def __init__(self): self.obs = []
    def subscribe(self, o): self.obs.append(o)
    def notify(self): 
        for o in self.obs: o.update(self)

class VincentBoss(Sujet):
    def __init__(self):
        super().__init__(); self.hp = 100
    def prendre_dmg(self, n):
        self.hp = max(0, self.hp - n)
        self.notify()

class BarreHP:
    def update(self, boss: VincentBoss):
        print(f"UI> HP Boss Vincent: {boss.hp}")

boss = VincentBoss()
ui = BarreHP()
boss.subscribe(ui)
boss.prendre_dmg(12)  # déclenche update UI
```
