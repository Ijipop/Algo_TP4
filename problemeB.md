# Problème B

Dans votre application, est-ce qu'il fonctionne dans plusieurs environnements (ex. : Windows, macOS, Linux), et/ou avec plus de styles d'interfaces (ex. : Sombre vs clair)? Chaque environnement vient avec leurs propres versions des composants, mais vous voulez que votre code reste indépendant de ces implémentations.

Vous avez de la difficulté :

- Avec le couplage fort entre le code client et les classes concrètes
- À changer de thème ou de plateforme
- Avec la maintenance complexe, si tu veux ajouter un nouveau style

## Solution : Utiliser le patron Abstract Factory

Le patron Abstract Factory permet de créer des familles d’objets liés sans connaître leurs classes concrètes.
Vous définissez une interface de fabrique abstraite, et chaque environnement fournit sa propre implémentation.

## Exemple de code

```python
# Composants abstraits
class Button:
    def render(self): pass

class Checkbox:
    def render(self): pass

# Composants concrets pour Windows
class WindowsButton(Button):
    def render(self): print("🟦 Bouton Windows")

class WindowsCheckbox(Checkbox):
    def render(self): print("🟦 Checkbox Windows")

# Composants concrets pour Mac
class MacButton(Button):
    def render(self): print("🍎 Bouton Mac")

class MacCheckbox(Checkbox):
    def render(self): print("🍎 Checkbox Mac")

# Fabrique abstraite
class GUIFactory:
    def create_button(self): pass
    def create_checkbox(self): pass

# Fabriques concrètes
class WindowsFactory(GUIFactory):
    def create_button(self): return WindowsButton()
    def create_checkbox(self): return WindowsCheckbox()

class MacFactory(GUIFactory):
    def create_button(self): return MacButton()
    def create_checkbox(self): return MacCheckbox()

# Code client indépendant de l’implémentation
def build_interface(factory: GUIFactory):
    button = factory.create_button()
    checkbox = factory.create_checkbox()
    button.render()
    checkbox.render()

# Utilisation
build_interface(WindowsFactory())  # 🟦 Bouton Windows, 🟦 Checkbox Windows
build_interface(MacFactory())      # 🍎 Bouton Mac, 🍎 Checkbox Mac

```
