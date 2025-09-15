# Problème C

Dans votre application, travaillez-vous avec un système composé de plusieurs modules ou services? Chaque module a ses propres appels et sa propre logique.

Vous avez de la difficulté :

- Avec la maintenance
- Avec la lecture du code
- Avec un système fragile face aux changements internes

Typiquement applicable pour:

- une application e-commerce avec plusieurs services
- un système multimédia avec gestion audio, vidéo, sous-titres
- une Interface graphique avec plusieurs composants bas niveau

## Solution : Utiliser le patron Façade

Le patron Façade propose une interface unifiée et simplifiée qui masque la complexité des sous-systèmes.
Vous créez une classe qui regroupe les appels internes et expose une API claire au reste de l’application, ce qui permet :

- De réduire le couplage entre le client et les sous-systèmes
- De simplifier l’utilisation du système
- De centraliser la logique métier

## Exemple de code

```python
# Sous-systèmes complexes
class AuthService:
    def login(self, user):
        print(f"🔐 {user} connecté")

class PaymentService:
    def pay(self, user, amount):
        print(f"💳 Paiement de {amount}€ effectué pour {user}")

class NotificationService:
    def notify(self, user, message):
        print(f"📧 Notification à {user} : {message}")

# Façade
class ECommerceFacade:
    def __init__(self):
        self.auth = AuthService()
        self.payment = PaymentService()
        self.notification = NotificationService()

    def acheter(self, user, montant):
        self.auth.login(user)
        self.payment.pay(user, montant)
        self.notification.notify(user, "Merci pour votre achat !")

# Code client simplifié
app = ECommerceFacade()
app.acheter("Natacha", 49.99)
```
