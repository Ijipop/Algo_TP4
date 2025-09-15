# Partie 2 : Analyse des Similitudes et Différences

Discutons des patrons qui se ressemblent et qui peuvent nous mélanger. C'est important de comprendre les différences sinon on va se retrouver avec du code moyen moyen!

## Décorateur vs Façade vs Adaptateur

Ces trois-là, ils ont l'air pareils parce qu'ils touchent tous à l'interface des objets, mais c'est pas pour les mêmes raisons :

### **Décorateur**

- **Pourquoi on l'utilise :** Pour ajouter des fonctionnalités à un objet sans le casser
- **Comment ça marche :** On prend l'objet original et on ajoute des fonctionnalités par-dessus
- **Exemple concret :** Comme mettre des mods sur une arme (feu + poison)
- **Le truc important :** L'interface reste pareille, on peut juste faire plus de choses

### **Façade**

- **Pourquoi on l'utilise :** Pour simplifier un bordel de code compliqué
- **Comment ça marche :** On cache toute la complexité derrière une interface simple
- **Exemple concret :** Comme un GameEngine qui gère audio, vidéo et input d'un coup
- **Le truc important :** L'interface est différente et plus simple que ce qu'il y a en dessous

### **Adaptateur**

- **Pourquoi on l'utilise :** Pour faire fonctionner des composants qui sont pas compatibles
- **Comment ça marche :** On traduit les appels d'une interface vers une autre
- **Exemple concret :** Faire qu'un gamepad fonctionne avec une API de clavier
- **Le truc important :** L'interface de sortie est différente de celle d'entrée

### **Où on se mélange :**

- **Décorateur vs Façade :** Le décorateur garde la même interface, la façade la simplifie
- **Façade vs Adaptateur :** La façade simplifie, l'adaptateur traduit entre des interfaces qui matchent pas

---

## Composite vs Décorateur

Ces deux-là utilisent la composition, mais c'est pas pour les mêmes affaires :

### **Composite**

- **Pourquoi on l'utilise :** Pour traiter des objets individuels et des collections de la même façon
- **Structure :** Comme un arbre (dossiers qui contiennent des fichiers et d'autres dossiers)
- **Le principe :** Uniformité - on peut traiter un objet individuel ou une collection d'objets de la même façon
- **Exemple concret :** Système de fichiers, interfaces utilisateur hiérarchiques

### **Décorateur**

- **Pourquoi on l'utilise :** Pour ajouter des fonctionnalités à un objet existant
- **Structure :** Comme une chaîne d'enveloppes (arme → feu → poison)
- **Le principe :** Ajout de fonctionnalités - même interface avec plus de comportements
- **Exemple concret :** Ajouter des effets dynamiquement

### **Ce qui est pareil :**

- Les deux utilisent la composition
- Les deux permettent d'ajouter des comportements
- Les deux peuvent avoir des objets qui contiennent d'autres objets du même type

### **Ce qui est différent :**

- **Composite :** Structure en arbre, traitement uniforme
- **Décorateur :** Structure linéaire, ajout de fonctionnalités
- **Composite :** Gère des collections d'objets
- **Décorateur :** Ajoute des fonctionnalités à un objet à la fois

### **Où on se mélange :**

- On peut utiliser un décorateur sur un composite, mais ils font des choses différentes
- Le composite gère la structure, le décorateur gère les fonctionnalités

---

## Strategy vs State

Ces deux-là utilisent une interface commune pour encapsuler des comportements, mais ce n’est pas pour les mêmes affaires :

### **Strategy**

- **Pourquoi on l'utilise :** Pour choisir dynamiquement un algorithme ou une logique métier parmi plusieurs options possibles
- **Structure :** Un objet principal délègue une tâche à un objet stratégie injecté
- **Le principe :** Flexibilité externe – le client décide quelle stratégie utiliser
- **Exemple concret :** Choisir entre un tri ascendant, descendant ou par longueur

### **State**

- **Pourquoi on l'utilise :** Pour faire évoluer le comportement d’un objet en fonction de son état interne
- **Structure :** L’objet contient une référence à son état courant, qui peut changer au fil du temps
- **Le principe :** Évolution interne – l’objet change de comportement en changeant d’état
- **Exemple concret :** Lecteur audio qui passe de lecture à pause ou arrêt

### **Ce qui est pareil :**

- Les deux utilisent une interface commune pour encapsuler des comportements
- Les deux permettent de changer le comportement à l’exécution
- Les deux reposent sur des classes interchangeables

### **Ce qui est différent :**

- **Strategy :** Le client choisit la stratégie à utiliser
- **State :** L’objet lui-même gère la transition entre états
- **Strategy :** Ne change pas tout seul de stratégie
- **State :** Change automatiquement de comportement selon l’état interne
- **Strategy :** Vise à varier une logique métier
- **State :** Vise à modéliser un cycle de vie ou une machine à états

### **Où on se mélange :**

- Les deux utilisent des classes interchangeables avec une interface commune
- Mais Strategy est externe à l’objet, State est interne et modifie l’objet lui-même

---

## Factory vs Singleton

Ces deux-là concernent la création d’objets, mais ce n’est pas pour les mêmes affaires :

### **Factory**

- **Pourquoi on l'utilise :** Pour centraliser et encapsuler la logique de création d’objets, souvent pour décider dynamiquement quelle classe concrète instancier
- **Structure :** Une méthode ou une classe fabrique qui retourne des instances, parfois selon des paramètres ou un contexte
- **Le principe :** Abstraction de la création – le code client ne connaît pas les détails de l’instanciation
- **Exemple concret :** Une fabrique qui crée un `BoutonWindows` ou un `BoutonMac` selon le système d’exploitation

### **Singleton**

- **Pourquoi on l'utilise :** Pour garantir qu’il n’existe qu’une seule instance d’une classe dans toute l’application
- **Structure :** Une classe qui contrôle sa propre instanciation et fournit un point d’accès global à son unique instance
- **Le principe :** Unicité – un seul objet partagé partout
- **Exemple concret :** Un gestionnaire de configuration ou un logger unique

### **Ce qui est pareil :**

- Les deux sont des patrons de création
- Les deux peuvent contrôler l’instanciation d’objets
- Les deux peuvent cacher les détails de création au code client

### **Ce qui est différent :**

- **Factory :** Peut créer autant d’instances que nécessaire, potentiellement de types différents
- **Singleton :** Ne crée qu’une seule instance pour toute l’application
- **Factory :** Vise la flexibilité dans le choix de l’implémentation
- **Singleton :** Vise le contrôle et la centralisation d’une ressource unique
- **Factory :** Le client peut demander plusieurs objets indépendants
- **Singleton :** Tous les appels renvoient le même objet

### **Où on se mélange :**

- Une Factory peut retourner un Singleton, ce qui peut brouiller les pistes
- Les deux peuvent sembler similaires si on ne regarde que la méthode `get_instance()` ou `create()`
- La différence clé :
  - Factory → "Quel type d’objet veux-tu ?"
  - Singleton → "Voici toujours le même objet"
  