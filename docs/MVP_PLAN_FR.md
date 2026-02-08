# Vinted → Leboncoin — Plan MVP (local, gratuit, stable)

## Décision produit immédiate
- **Oui, il faut un clic principal**: bouton **"Publier sur Leboncoin"**.
- Flux cible:
  1. L'utilisateur ouvre l'extension sur Vinted.
  2. Il sélectionne des annonces (encadré vert).
  3. Il clique sur **Publier sur Leboncoin**.
  4. Assistant pas-à-pas (première utilisation), puis tableau de bord des envois.

## Contraintes imposées
- Priorité: **Vinted vers Leboncoin**.
- Exécution **100% locale** (pas d'hébergement cloud, budget 0€).
- 1 compte Vinted + 1 compte Leboncoin.
- Sessions/cookies stockés en local.
- Copie fidèle: titre, description, prix, photos, catégorie, marque, taille, état.
- Photos: max 10, pas de retouche, pas de réordonnancement.
- Envoi multi-annonces avec temporisation **30 à 60 s** entre publications.
- Stabilité > vitesse.

## Architecture recommandée (MVP)

### 1) Extension Chrome (Manifest V3)
Pourquoi:
- Plus simple pour interagir avec l'utilisateur déjà connecté aux deux sites.
- Évite un backend payant.
- Données locales via `chrome.storage.local`.

Modules:
- `content-vinted`: lit les annonces et permet la sélection visuelle.
- `content-lbc`: remplit le formulaire de dépôt Leboncoin.
- `background`: orchestre la file de publication + délais.
- `popup/options`: assistant de configuration puis dashboard.

### 2) Moteur de mapping
- Table de correspondance locale:
  - catégories Vinted → catégories Leboncoin,
  - tailles/attributs,
  - marques (normalisation).
- Si ambiguïté: passage en correction manuelle.

### 3) Gestion d'erreurs (obligatoire)
Codes proposés:
- `AUTH_VINTED_MISSING`
- `AUTH_LBC_MISSING`
- `SCRAPE_ITEM_NOT_FOUND`
- `SCRAPE_PHOTO_LIMIT_EXCEEDED`
- `MAP_CATEGORY_UNRESOLVED`
- `MAP_SIZE_UNRESOLVED`
- `FORM_FIELD_REQUIRED_MISSING`
- `UPLOAD_PHOTO_FAILED`
- `LBC_RATE_LIMIT_SUSPECTED`
- `LBC_CAPTCHA_REQUIRED`
- `PUBLISH_SUBMIT_FAILED`
- `PUBLISH_CONFIRMATION_TIMEOUT`

Statuts job:
- `queued`, `in_progress`, `published`, `needs_manual_fix`, `failed`.

## Workflow détaillé
1. Scan Vinted et extraction des annonces.
2. Sélection multi-annonces (UI encadré vert).
3. Prévalidation des données obligatoires.
4. Mapping vers modèle Leboncoin.
5. Publication séquentielle avec délai configurable (30-60 s).
6. En cas d'erreur, arrêt de l'annonce + correction manuelle + reprise.

## MVP en 4 étapes
1. **Base extension + sélection visuelle**
2. **Extraction annonce + photos (local)**
3. **Remplissage formulaire Leboncoin + publication unitaire**
4. **Batch 30 annonces + statuts + codes d'erreur**

## Ce qu'on ne fait pas dans le MVP
- Synchronisation "vendu" inter-plateformes.
- Logs avancés/audit/alerte email.
- Multi-utilisateurs.
- Infrastructure SaaS.

## Réponse claire à ta question "un clic ou non ?"
- **Oui: un clic principal est idéal.**
- Mais pour rester stable et éviter les blocages, ce clic doit lancer un pipeline avec:
  - validation automatique,
  - pauses anti-robot,
  - demandes de correction manuelle quand nécessaire.


## Guide de test (débutant)
- Voir `docs/TESTER_DEBUTANT_FR.md` pour les étapes exactes de test manuel.

- Publication GitHub (débutant): `docs/PUBLIER_SUR_GITHUB_FR.md`.
