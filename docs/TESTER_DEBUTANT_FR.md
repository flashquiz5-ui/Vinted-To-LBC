# Comment tester l'application (guide débutant, pas à pas)

## Important : état actuel du projet
Pour l'instant, ce dépôt contient surtout un **plan MVP** (`docs/MVP_PLAN_FR.md`).
Il n'y a pas encore l'extension Chrome codée.

👉 Donc aujourd'hui, tu ne peux pas encore faire un test "réel" de publication automatique.
Tu peux seulement valider le plan, puis coder le MVP.

---

## 1) Ce que tu pourras tester dès que le MVP est codé
L'objectif de test sera :
1. Sélectionner des annonces Vinted.
2. Cliquer sur "Publier sur Leboncoin".
3. Vérifier le passage des annonces en statut `in_progress` puis `published` (ou `needs_manual_fix`).
4. Vérifier l'intervalle 30–60 secondes entre annonces.

---

## 2) Pré-requis (simples)
- Google Chrome installé.
- Un compte Vinted connecté dans Chrome.
- Un compte Leboncoin connecté dans Chrome.
- 2 à 3 annonces de test (pas 150 au début).

---

## 3) Installer une extension locale dans Chrome (mode développeur)
Quand le code existera, fais exactement ça :

1. Ouvre Chrome.
2. Va sur `chrome://extensions`.
3. Active **Mode développeur** (interrupteur en haut à droite).
4. Clique sur **Charger l'extension non empaquetée**.
5. Sélectionne le dossier du projet de l'extension (ex: `extension/`).
6. Vérifie qu'elle apparaît dans la liste des extensions.

Si Chrome refuse le chargement :
- regarde les erreurs en rouge sur la carte de l'extension,
- corrige puis clique sur **Actualiser**.

---

## 4) Test manuel recommandé (ordre exact)

### Test A — Connexion/session
- Ouvre Vinted et Leboncoin dans le même profil Chrome.
- Vérifie que tu es connecté sur les deux.

Résultat attendu :
- pas d'erreur `AUTH_VINTED_MISSING` ni `AUTH_LBC_MISSING`.

### Test B — Sélection des annonces
- Va sur ta page annonces Vinted.
- Sélectionne 2 annonces.
- Vérifie le cadre vert sur les annonces sélectionnées.

Résultat attendu :
- les 2 annonces passent en file `queued`.

### Test C — Publication unitaire
- Lance une seule annonce vers Leboncoin.

Résultat attendu :
- statut `in_progress` puis `published`.
- si échec, code précis (`MAP_CATEGORY_UNRESOLVED`, `UPLOAD_PHOTO_FAILED`, etc.).

### Test D — Publication batch
- Lance 2–3 annonces avec pause 30–60 secondes.

Résultat attendu :
- traitement séquentiel stable,
- pas de flood,
- statuts finaux visibles annonce par annonce.

---

## 5) Conseils anti-ban (très pratique)
- Commence toujours petit: 2–3 annonces.
- Garde une pause réelle entre publications (30–60 s minimum).
- Évite de relancer 20 fois d'affilée si erreur CAPTCHA.
- Si un CAPTCHA apparaît: intervention manuelle, puis reprise.

---

## 6) Plan de test ultra simple pour toi (sans technique)
À chaque nouvelle version, fais ce mini-check :

- [ ] Je vois mes annonces Vinted dans l'outil.
- [ ] Je peux en sélectionner 2.
- [ ] Le bouton "Publier sur Leboncoin" démarre bien.
- [ ] Au moins 1 annonce se publie correctement.
- [ ] Si erreur, j'ai un code clair et compréhensible.

Si ces 5 points sont OK, le MVP progresse dans le bon sens.

---

## 7) Ce qu'il faut coder ensuite pour que tu puisses tester "en vrai"
1. Base extension Chrome MV3 (`manifest.json`, background, content scripts).
2. Récupération des annonces Vinted + sélection visuelle.
3. Remplissage formulaire Leboncoin pour 1 annonce.
4. Dashboard statuts + codes d'erreur.
5. Batch avec délai configurable.

Une fois ces 5 blocs codés, tu pourras faire un test complet de bout en bout.
