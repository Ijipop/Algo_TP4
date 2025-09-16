# Observer (Observateur)

Dans votre application, avez-vous besoin qu'un objet notifie automatiquement d'autres objets quand son état change, sans que ces objets aient besoin de vérifier constamment ?

Vous avez de la difficulté :

- Avec des performances dégradées à cause des vérifications répétées
- À maintenir la synchronisation entre plusieurs composants
- Avec un couplage fort entre les objets

Typiquement applicable pour :
- Système d'interface utilisateur (barre de vie, notifications)
- Système d'événements (clics, changements de données)
- Architecture MVC (modèle notifie les vues)
- Système de cache et synchronisation de données

## Code non optimisé

```python
class VincentBoss:
    def __init__(self): self.hp = 100
    def tick(self):
        # ... dommages ...
        pass
```

> **Problème :** L'UI doit sonder sans arrêt l'état, ce qui gaspille et complique le code.

## Solution : Utiliser le patron Observer

Le patron Observer établit une relation de dépendance un-à-plusieurs entre objets. Quand l'objet observé change d'état, tous ses observateurs sont automatiquement notifiés, ce qui permet :

- De découpler les objets observés des observateurs
- De notifier automatiquement plusieurs composants
- De faciliter l'ajout/suppression d'observateurs

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

# Utilisation
boss = VincentBoss()
ui = BarreHP()
boss.subscribe(ui)
boss.prendre_dmg(12)  # déclenche update UI
```
