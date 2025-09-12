# Problème E

Dans votre application, gérez-vous des structures hiérarchiques ou manipulez-vous des objets organisés en arborescence?

Vous avez de la difficulté :

- Devoir traiter différemment les objets simples et les groupes
- Devoir dupliquer du code pour chaque niveau
- Vouloir appliquer des opérations récursives (affichage, suppression, export) sans complexité

Typiquement applicable pour:

- Fichiers et dossiers
- Éléments d’interface graphique
- Composants d’un document (titres, paragraphes, images)
- Menus imbriqués

## Solution : Utiliser le patron Composite

Le patron Composite permet de structurer des objets en arborescence, où chaque élément peut être une feuille (objet simple) ou un composite (objet contenant d’autres objets). Tous les éléments partagent une interface commune, ce qui permet de les manipuler de manière uniforme, peu importe leur nature.

## Exemple de code

```python
# Interface commune
class Composant:
    def afficher(self, indent=0): pass

# Feuille : fichier
class Fichier(Composant):
    def __init__(self, nom):
        self.nom = nom

    def afficher(self, indent=0):
        print(" " * indent + f"📄 {self.nom}")

# Composite : dossier
class Dossier(Composant):
    def __init__(self, nom):
        self.nom = nom
        self.elements = []

    def ajouter(self, composant):
        self.elements.append(composant)

    def afficher(self, indent=0):
        print(" " * indent + f"📁 {self.nom}")
        for e in self.elements:
            e.afficher(indent + 2)

# Construction de l’arborescence
racine = Dossier("Documents")
racine.ajouter(Fichier("CV.pdf"))
racine.ajouter(Fichier("rapport.docx"))

photos = Dossier("Photos")
photos.ajouter(Fichier("vacances.jpg"))
photos.ajouter(Fichier("anniversaire.png"))

racine.ajouter(photos)

# Affichage
racine.afficher()
```
