# Factory (Fabrique)

Dans votre application, devez-vous créer des objets de différents types selon des paramètres ou des conditions, mais vous voulez éviter de coupler votre code aux classes concrètes ?

Vous avez de la difficulté :
- Avec du code qui connaît directement les classes concrètes à instancier
- À maintenir le code quand vous ajoutez de nouveaux types
- Avec un couplage fort entre le code client et les classes créées
- À centraliser la logique de création d'objets

Typiquement applicable pour :
- Création d'ennemis dans un jeu selon le niveau
- Création de documents selon le format (PDF, Word, etc.)
- Création d'objets selon la configuration système
- Création de composants UI selon le thème

## Code non optimal

```python
def spawner(type_ennemi):
    if type_ennemi == "slime": return Slime()
    if type_ennemi == "squelette": return Squelette()
    if type_ennemi == "vincent_boss": return VincentBoss()  
```

> **Problème :** Le code est couplé et il faut toujours modifier la fonction pour chaque nouvel ennemi.

## Solution : Utiliser le patron Factory

Le patron Factory encapsule la logique de création d'objets dans une classe ou méthode dédiée. Le code client n'a pas besoin de connaître les classes concrètes, ce qui permet :

- De découpler le code client des classes concrètes
- De centraliser la logique de création
- De faciliter l'ajout de nouveaux types sans modifier le code existant
- D'améliorer la maintenabilité et l'extensibilité

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

# Utilisation
ennemi = EnemyFactory.create("vincent_boss")
```
