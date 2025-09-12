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
