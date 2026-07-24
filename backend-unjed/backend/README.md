# Backend UNJED-BENIN — déploiement sur PythonAnywhere

## 1. Créer le compte
Va sur https://www.pythonanywhere.com, crée un compte payant ("Hacker plan", ~5$/mois,
nécessaire pour que le site tourne en continu et accepte les appels depuis GitHub Pages).

## 2. Envoyer les fichiers
Dans l'onglet **Files** de PythonAnywhere, crée un dossier `unjed-backend` et uploade
tous les fichiers de ce dossier (`app.py`, `models.py`, `emailer.py`, `seed_admins.py`,
`requirements.txt`).

Ou, plus simple, via un terminal Bash PythonAnywhere (onglet **Consoles** > **Bash**) :
```bash
git clone <url-de-ton-repo-backend> unjed-backend
cd unjed-backend
```

## 3. Installer les dépendances
Dans une console Bash PythonAnywhere :
```bash
cd unjed-backend
pip install --user -r requirements.txt
```

## 4. Créer les comptes admin
Toujours dans la console Bash :
```bash
python3 seed_admins.py
```
Suis les instructions (nom d'utilisateur + mot de passe pour chacun des 2-3 admins).
Les mots de passe ne sont jamais écrits dans un fichier, juste hashés en base.

## 5. Configurer l'application Web
Onglet **Web** > **Add a new web app** > choisis **Flask** > Python 3.10+.
Dans la section **Code**, indique le chemin vers `app.py`.
PythonAnywhere génère un fichier WSGI — édite-le pour qu'il importe ton app :
```python
import sys
path = '/home/TON-USERNAME/unjed-backend'
if path not in sys.path:
    sys.path.append(path)
from app import app as application
```

## 6. Variables d'environnement (secrets)
Onglet **Web** > section **Environment variables**, ajoute :

| Nom | Valeur |
|---|---|
| `SECRET_KEY` | une longue chaîne aléatoire (ex: générée avec `python3 -c "import secrets; print(secrets.token_hex(32))"`) |
| `MAIL_SERVER` | `smtp.gmail.com` (si tu utilises Gmail) |
| `MAIL_PORT` | `587` |
| `MAIL_USERNAME` | ton adresse email dédiée de l'ONG |
| `MAIL_PASSWORD` | un **mot de passe d'application** Gmail (pas ton mot de passe normal — à créer dans les paramètres de sécurité Google) |
| `MAIL_FROM` | la même adresse que `MAIL_USERNAME` |
| `ADMIN_NOTIFY_EMAIL` | l'adresse qui doit recevoir les notifications (nouveaux abonnés, etc.) |

Ces variables ne sont JAMAIS écrites dans le code — c'est exactement pour ça qu'elles
sont ici, dans la configuration du serveur.

## 7. Recharger l'app
Bouton vert **Reload** en haut de l'onglet **Web**.

## 8. Mettre à jour le site (GitHub Pages)
Dans `administration.html` et `components.js`, remplace :
```js
var API_BASE = "https://VOTRE-USERNAME.pythonanywhere.com";
```
par ta vraie adresse PythonAnywhere (ex: `https://theophile.pythonanywhere.com`).

## 9. Tester
- Va sur `https://TON-USERNAME.pythonanywhere.com/api/health` → doit répondre `{"status":"ok"}`
- Va sur `administration.html` de ton site, connecte-toi avec un des comptes créés à l'étape 4.

## Prochaine étape : paiement (FedaPay/Kkiapay)
Le fichier `app.py` contient un emplacement commenté (`Emplacement réservé : PAIEMENT
ADHESION`) prêt à être complété une fois que tu auras créé ton compte marchand.
