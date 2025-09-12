# Travail Pratique 4 - Patrons de Conception

## Informations Générales

**Étudiants :** Natacha Meyer & Jean-François Lefebvre  
**Groupe :** 24606  
**Date :** 9/9/25

**Pondération :** 20%  
**Date de l'énoncé :** 5 septembre 2025  
**Date de remise :** 19 septembre 2025 23h59  
**Remise :** Via Omnivox  
**Équipe :** En équipe de deux. Un seul dépôt par équipe

---

## Partie 1 : Problèmes et Solutions

Cette section présente des problèmes courants en développement et les patrons de conception qui peuvent les résoudre.

---

## Patrons de Conception

### 1. State (État)

#### Code non optimal

```python
class Heros:
    def __init__(self): self.etat = "IDLE"
    def attaquer(self):
        if self.etat == "IDLE": print("Petit coup")
        elif self.etat == "RAGE": print("Gros coup critique!")
        elif self.etat == "STUN": print("Impossible de bouger…")
```

> **Problème :** Les if/elif deviennent vite lourds et difficiles à maintenir si on ajoute de nouveaux états.

#### Code optimisé

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

# Usage
h = Heros(Idle()); h.attaquer()
h.set_etat(Rage()); h.attaquer()
```

---

### 2. Factory (Fabrique)

#### Code non optimal

```python
def spawner(type_ennemi):
    if type_ennemi == "slime": return Slime()
    if type_ennemi == "squelette": return Squelette()
    if type_ennemi == "vincent_boss": return VincentBoss()  
```

> **Problème :** Le code est couplé et il faut toujours modifier la fonction pour chaque nouvel ennemi.

#### Code optimisé

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

---

### 3. Decorator (Décorateur)

#### Code non optimisé

```python
class Epee: 
    def dmg(self): return 10

class EpeeAvecFeu(Epee):
    def dmg(self): return super().dmg() + 5

class EpeeFeuPoison(EpeeAvecFeu):
    def dmg(self): return super().dmg() + 3  # explosion de classes
```

> **Problème :** L'héritage crée une explosion de classes dès qu'on combine plusieurs effets.

#### Code optimisé

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

---

### 4. Observer (Observateur)

#### Code non optimisé

```python
class VincentBoss:
    def __init__(self): self.hp = 100
    def tick(self):
        # ... dommages ...
        pass
```

> **Problème :** L'UI doit sonder sans arrêt l'état, ce qui gaspille et complique le code.

#### Code optimisé

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

---

### 5. Adapter (Adaptateur)

#### Code non optimisé

```python
# Le moteur attend KeyboardAPI.up()/down()/left()/right()
class Gamepad:
    def axis(self): return (1, 0)

def bouger(moteur, input_device):
    x, y = input_device.axis()
    if x > 0: moteur.right()
    
```

> **Problème :** Chaque nouvel appareil oblige à réécrire du code spécifique un peu partout.

#### Code optimisé

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

---

## Partie 2 : Analyse des Similitudes et Différences

Discutons des patrons qui se ressemblent et qui peuvent nous mélanger. C'est important de comprendre les différences sinon on va se retrouver avec du code moyen moyen!

### Décorateur vs Façade vs Adaptateur

Ces trois-là, ils ont l'air pareils parce qu'ils touchent tous à l'interface des objets, mais c'est pas pour les mêmes raisons :

#### **Décorateur**
- **Pourquoi on l'utilise :** Pour ajouter des fonctionnalités à un objet sans le casser
- **Comment ça marche :** On prend l'objet original et on ajoute des fonctionnalités par-dessus
- **Exemple concret :** Comme mettre des mods sur une arme (feu + poison)
- **Le truc important :** L'interface reste pareille, on peut juste faire plus de choses

#### **Façade**
- **Pourquoi on l'utilise :** Pour simplifier un bordel de code compliqué
- **Comment ça marche :** On cache toute la complexité derrière une interface simple
- **Exemple concret :** Comme un GameEngine qui gère audio, vidéo et input d'un coup
- **Le truc important :** L'interface est différente et plus simple que ce qu'il y a en dessous

#### **Adaptateur**
- **Pourquoi on l'utilise :** Pour faire fonctionner des composants qui sont pas compatibles
- **Comment ça marche :** On traduit les appels d'une interface vers une autre
- **Exemple concret :** Faire qu'un gamepad fonctionne avec une API de clavier
- **Le truc important :** L'interface de sortie est différente de celle d'entrée

#### **Où on se mélange :**
- **Décorateur vs Façade :** Le décorateur garde la même interface, la façade la simplifie
- **Façade vs Adaptateur :** La façade simplifie, l'adaptateur traduit entre des interfaces qui matchent pas

### Composite vs Décorateur

Ces deux-là utilisent la composition, mais c'est pas pour les mêmes affaires :

#### **Composite**
- **Pourquoi on l'utilise :** Pour traiter des objets individuels et des collections de la même façon
- **Structure :** Comme un arbre (dossiers qui contiennent des fichiers et d'autres dossiers)
- **Le principe :** Uniformité - on peut traiter un objet individuel ou une collection d'objets de la même façon
- **Exemple concret :** Système de fichiers, interfaces utilisateur hiérarchiques

#### **Décorateur**
- **Pourquoi on l'utilise :** Pour ajouter des fonctionnalités à un objet existant
- **Structure :** Comme une chaîne d'enveloppes (arme → feu → poison)
- **Le principe :** Ajout de fonctionnalités - même interface avec plus de comportements
- **Exemple concret :** Ajouter des effets dynamiquement

#### **Ce qui est pareil :**
- Les deux utilisent la composition
- Les deux permettent d'ajouter des comportements
- Les deux peuvent avoir des objets qui contiennent d'autres objets du même type

#### **Ce qui est différent :**
- **Composite :** Structure en arbre, traitement uniforme
- **Décorateur :** Structure linéaire, ajout de fonctionnalités
- **Composite :** Gère des collections d'objets
- **Décorateur :** Ajoute des fonctionnalités à un objet à la fois

#### **Où on se mélange :**
- On peut utiliser un décorateur sur un composite, mais ils font des choses différentes
- Le composite gère la structure, le décorateur gère les fonctionnalités
