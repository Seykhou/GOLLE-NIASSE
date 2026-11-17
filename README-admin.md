# Administration du site Gollé Niasse

Ce dossier `deploy/` est prêt pour une publication sur **Netlify** ou **GitHub Pages**. Il n’est pas publié depuis cet environnement : la mise en ligne se fait uniquement lorsque vous le décidez, depuis votre dépôt.

Le fichier autonome `../index.html` reste utilisable hors ligne, sans serveur.

## Contenu du dossier

- `index.html` — site public (images en fichiers, textes chargés depuis le JSON)
- `content/site.json` — textes, compétences, options de tarifs et prix indicatifs
- `content/realisations.json` — galerie (titre, légende, image, catégorie)
- `images/` — photographies et illustrations d’exemple (SVG et JPEG)
- `fonts/` — typographies Inter et Cormorant Garamond
- `admin/` — back-office [Decap CMS](https://decapcms.org/) (ancien Netlify CMS)

## 1. Créer le dépôt GitHub

1. Créez un dépôt GitHub vide (public ou privé).
2. Remplacez `OWNER/REPO` dans `admin/config.yml` par l’identifiant réel, par exemple `mon-compte/golle-niasse`.
   ```yaml
   backend:
     name: github
     repo: OWNER/REPO
     branch: main
   ```
3. Envoyez tout le contenu de ce dossier `deploy/` à la racine du dépôt (branche `main`).

## 2. Relier le dépôt à Netlify

1. Connectez-vous à [Netlify](https://app.netlify.com/) et ajoutez un nouveau site à partir du dépôt GitHub.
2. Répertoire de publication : la racine du dépôt (là où se trouve `index.html`). Pas de commande de build nécessaire.
3. Une fois le site déployé, notez l’URL Netlify (par exemple `https://golle-niasse.netlify.app`).

## 3. Activer l’authentification OAuth pour Decap CMS

Decap CMS, avec le backend GitHub, a besoin d’un serveur OAuth pour que vous puissiez vous connecter à `/admin/` sans exposer de jeton.

La voie la plus simple avec Netlify :

1. Dans l’interface Netlify du site : **Site configuration → Access & security → OAuth**.
2. Installez et autorisez l’application **Netlify** sur le compte GitHub du dépôt (si ce n’est pas déjà fait).
3. Ajoutez un fournisseur **GitHub** et suivez les indications Netlify (création d’une GitHub OAuth App si demandé : *Homepage URL* = l’URL du site, *Authorization callback URL* = `https://api.netlify.com/auth/done`).
4. Ouvrez `https://votre-site.netlify.app/admin/` puis connectez-vous avec GitHub.

Sans Netlify, vous pouvez aussi utiliser un petit serveur OAuth (par exemple [decap-cms/oauth-provider](https://github.com/decapcms/decap-cms) / un fournisseur compatible) et renseigner `base_url` dans `admin/config.yml`.

## 4. Utiliser le back-office

- **Textes du site** : titres, chapôs, e-mail, téléphone, liste de compétences.
- **Options de tarifs** : intitulé, description, prix indicatif (laisser le prix vide pour « sur devis »).
- **Réalisations** : titre, légende (par défaut « Illustration d'exemple »), catégorie, image (bibliothèque `images/`).

Chaque enregistrement crée un commit sur la branche `main`. Netlify reconstruit ensuite le site. Le front lit `content/site.json` et `content/realisations.json` ; si le chargement échoue, les contenus déjà présents dans `index.html` (repli intégré) restent affichés.

## 5. GitHub Pages (variante)

Vous pouvez servir le même dossier via GitHub Pages (branche `main`, racine). L’authentification Decap y est moins directe qu’avec Netlify OAuth : prévoyez un fournisseur OAuth externe, ou gérez les fichiers JSON directement dans GitHub.

## Rappels

- Remplacez les illustrations d’exemple par vos pièces réelles avant une mise en ligne publique.
- Ne commitez jamais de jetons d’accès dans le dépôt.
- Le site autonome `index.html` (à la racine du projet, hors `deploy/`) n’utilise pas ce back-office : c’est une copie hors ligne, autonome.
