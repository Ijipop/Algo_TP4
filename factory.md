# Factory (Fabrique)

## Code non optimal

```python
def spawner(type_ennemi):
    if type_ennemi == "slime": return Slime()
    if type_ennemi == "squelette": return Squelette()
    if type_ennemi == "vincent_boss": return VincentBoss()  
```

> **Problème :** Le code est couplé et il faut toujours modifier la fonction pour chaque nouvel ennemi.

## Code optimisé

```python
class Ennemi: ...
class Slime(Ennemi): ...
class Squelette(Ennemi): ...
class VincentBoss(Ennemi): ...

class FabriqueEnnemi:
    _registre = {}

    @classmethod
    def enregistrer(cls, nom, constructeur):
        cls._registre[nom] = constructeur

    @classmethod
    def creer(cls, nom, **attributs):
        return cls._registre[nom](**attributs)

# Enregistrement 
EnemyFactory.register("slime", Slime)
EnemyFactory.register("squelette", Squelette)
EnemyFactory.register("vincent_boss", VincentBoss)

# Usage
ennemi = EnemyFactory.create("vincent_boss")
```
