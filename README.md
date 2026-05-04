
4. **Public** ✓ (obligatoire pour GitHub Pages gratuit).
5. **Add README** ✓.
6. **Create repository**.

### 2. Uploader les fichiers

Dans ton repo fraîchement créé :

1. **Add file** → **Upload files**.
2. Glisse-dépose : `index.html`, `encrypt.html`, `README.md`.
3. **NE PAS uploader `data.json`** — il contient les données en clair, on ne le met jamais sur GitHub.
4. Pour `data.enc.json` : voir étape 4 ci-dessous.

### 3. Activer GitHub Pages

1. Dans le repo : **Settings** (onglet en haut à droite).
2. Menu gauche : **Pages**.
3. **Source** : `Deploy from a branch`.
4. **Branch** : `main` / `/ (root)` → **Save**.
5. Attends 30 secondes. L'URL apparaît en haut : `https://[ton-username].github.io/[nom-du-repo]/`.

### 4. Première encryption

1. Sur ton Mac, ouvre `encrypt.html` (double-clic, ça s'ouvre dans Safari).
2. Colle le contenu de `data.json` dans le grand champ.
3. Tape ton mot de passe — choisis-en un long et stocke-le dans **iCloud Keychain** :
   - Sur Mac : Réglages Système → Mots de passe → ajouter manuel · associer à l'URL GitHub Pages.
   - Sur iPhone : ça apparaîtra ensuite en autofill quand tu tape sur la page.
4. Clique **Chiffrer**.
5. **Copier dans le presse-papier**.
6. Sur GitHub, dans ton repo : **Add file** → **Create new file** → nom : `data.enc.json` → colle → **Commit**.

### 5. Tester

Ouvre ton URL GitHub Pages. Tu vois un écran avec un point doré et un champ. Tape ton mot de passe → la page se débloque.

---

## Workflow d'update (à chaque modif)

Trois options, par ordre de simplicité :

### Option A — Sur Mac (recommandé)

1. Édite `data.json` localement (text editor, VS Code, ce que tu veux).
2. Ouvre `encrypt.html`, colle le nouveau JSON, mot de passe → chiffrer → copier.
3. Sur GitHub.com : ouvre `data.enc.json` dans le repo → bouton crayon ✏️ → remplace tout par ce que tu as copié → **Commit**.
4. ~30 sec après, la page en ligne est à jour.

### Option B — Sur iPhone (app GitHub)

1. App **GitHub** → ton repo → `data.json` → ✏️ → modifie.
2. ⚠️ Comme tu ne peux pas chiffrer sur iPhone facilement, tu commit `data.json` modifié, puis sur Mac tu re-chiffres et push `data.enc.json` à jour.

### Option C — Tout sur Mac

C'est l'option la plus propre. Tu peux même cloner le repo en local avec GitHub Desktop, éditer dans VS Code, voir le résultat tout de suite.

---

## Sécurité — ce qui est protégé

✅ Une personne qui tombe sur l'URL : voit un champ password, voit `data.enc.json` qui est un blob illisible (AES-256 + PBKDF2 250k itérations).
✅ Le code source `index.html` est public mais ne contient aucune donnée sensible — juste le code de déchiffrement.
✅ Les moteurs de recherche : `noindex` dans le head + URL obscure.
✅ Sur tes devices : iCloud Keychain remplit le password automatiquement.

❌ Ce qui n'est pas protégé : si quelqu'un a ton mot de passe **et** l'URL, il a accès. Le mot de passe est ta seule barrière — choisis-en un long (16+ caractères ou phrase de 4 mots).

---

## Fichiers du repo

| Fichier | Rôle | Sur GitHub ? |
|---|---|---|
| `index.html` | Le renderer + login | Oui |
| `encrypt.html` | L'outil de chiffrement | Oui (utile sur Mac, ne révèle rien) |
| `data.enc.json` | Données chiffrées | Oui (illisible sans password) |
| `data.json` | Données en clair | **NON — jamais** |
| `README.md` | Ce doc | Oui |

---

## En cas de problème

- **« mot de passe incorrect » alors que c'est le bon** : probablement que `data.enc.json` n'a pas été régénéré après une modif de `data.json`. Re-fais le cycle encrypt.
- **La page met du temps à se mettre à jour après commit** : GitHub Pages cache. Force le refresh (Cmd+Shift+R).
- **J'ai oublié mon mot de passe** : aucun moyen de récupérer. Tu repars de `data.json`, tu choisis un nouveau mot de passe, tu re-chiffres.
