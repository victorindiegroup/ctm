# CTM — Carte · Tarif · Millésime

Application d'élaboration des cartes de vins pour Indie Group.

## Contenu du dossier

```
ctm/
├── index.html              ← l'app (utilisée par Victor, Raph, Pierre Alexis, Lou)
├── produits.json           ← le catalogue produits + établissements + fournisseurs
└── README.md               ← ce fichier
```

---

## Installation initiale (première fois)

### Étape 1 — Créer le repo sur GitHub

1. Connecte-toi sur <https://github.com> avec ton compte `victorindiegroup`
2. En haut à droite → icône **+** → **"New repository"**
3. Nom : `ctm` · **Public** (pour que GitHub Pages fonctionne gratuitement) · Ne pas cocher "Add a README"
4. Clique **"Create repository"**

### Étape 2 — Uploader les fichiers

1. Sur la page du repo vide, clique sur **"uploading an existing file"** (lien bleu au milieu)
2. Glisse-dépose les 3 fichiers : `index.html`, `produits.json`, `README.md`
3. En bas, clique **"Commit changes"**

### Étape 3 — Activer GitHub Pages

1. Onglet **"Settings"** du repo (tout à droite dans la barre du haut)
2. Menu de gauche → **"Pages"**
3. Sous "Build and deployment" :
   - Source : **"Deploy from a branch"**
   - Branch : **`main`** · dossier **`/` (root)**
   - Clique **Save**
4. Attends 1-2 minutes, rafraîchis la page. L'URL de ton app apparaît en haut :
   `https://victorindiegroup.github.io/ctm/`

### Étape 4 — Installation sur téléphone (optionnel)

L'app s'ouvre dans n'importe quel navigateur. Pour l'avoir comme une vraie app :

**iPhone** (Safari obligatoire) : ouvrir l'URL → icône Partager (carré avec flèche) → **"Sur l'écran d'accueil"**

**Android** (Chrome) : ouvrir l'URL → menu 3 points → **"Installer l'application"**

---

## Utilisation quotidienne

### Se connecter comme tel ou tel utilisateur

En haut à droite de l'app, le sélecteur te permet de basculer entre **Victor** (admin), **Raph** (éditeur), **Pierre Alexis** (éditeur) et **Lou** (lectrice).

Pour l'instant c'est un switch libre (pas de mot de passe) — tant qu'on n'a pas de backend, c'est une convention entre vous.

### Créer une carte

1. Dans la **sidebar** → **Cartes** → **"Nouvelle carte"**
2. Ou depuis l'accueil, clique directement sur une card d'établissement sans carte
3. Dans l'éditeur, **"Ajouter un vin"** → pioche dans le catalogue → saisie du PVP TTC → le coef s'affiche automatiquement

### Sortir le PDF pour Lou

1. Ouvre la carte → bouton **"Aperçu PDF"** en haut à droite
2. Deux options :
   - **"Copier pour graphiste"** : met le texte formaté dans le presse-papier, Lou n'a qu'à coller
   - **"Imprimer / PDF"** : ouvre le dialogue d'impression → destination "Enregistrer au format PDF"

### Ajouter un nouveau produit

- **Victor ou Raph** : ça passe direct dans le catalogue
- **Pierre Alexis** : une alerte est envoyée à Victor pour validation (visible dans l'onglet Alertes)

---

## Mettre à jour le catalogue produits

### Workflow actuel (v1)

Pour l'instant, le fichier `produits.json` contient une **cinquantaine de produits de démarrage** — c'est suffisant pour prendre en main l'app. Les ajouts/modifications que tu fais dans l'interface sont **sauvegardés dans le navigateur** (localStorage), ce qui veut dire :

- Si tu changes de navigateur ou d'appareil, tu repars avec les données de `produits.json`
- Si tu vides le cache de ton navigateur, tu perds les modifs
- Les modifs de Victor ne sont **pas visibles** par Raph, Pierre Alexis ou Lou — c'est local à chaque appareil

C'est une limitation volontaire de la v1 pour démarrer sans backend. Pour de la collaboration réelle, il faudra passer v2 avec Cloudflare D1 ou un Google Sheets comme source partagée.

### Pour enrichir `produits.json` avec plus de produits

Si tu veux que les nouveaux produits soient partagés par défaut pour tout le monde :

1. Éditer `produits.json` localement (avec VS Code ou un éditeur de texte)
2. Ajouter tes lignes dans le tableau `produits` en respectant le format existant
3. Upload le nouveau fichier sur GitHub (même méthode que pour les mises à jour d'ITM : ouvre le fichier → crayon → remplace le contenu → Commit)

Un **convertisseur** (similaire à celui d'ITM pour Yokitup) viendra dans une prochaine version pour éviter l'édition manuelle du JSON.

---

## Mettre à jour l'app elle-même

Si on modifie `index.html` (nouvelle fonctionnalité, correction de bug) :

1. Upload le nouveau `index.html` sur GitHub (écrase l'ancien)
2. Les utilisateurs récupèrent la mise à jour **au prochain rafraîchissement** de la page (pas de service worker pour l'instant, pas besoin d'incrémenter un numéro de version comme pour ITM)

---

## Structure d'une fiche produit

Chaque produit dans `produits.json` a la structure suivante :

```json
{
  "id": "p42",
  "service": "Bouteille",           // "Au Verre" ou "Bouteille"
  "couleur": "Rouge",                // Champagne | Blanc | Rosé | Rouge | Vin Doux | Saké
  "region": "Bourgogne",
  "sousRegion": "Côte de Beaune",   // optionnel
  "appellation": "Pommard 1er Cru",
  "domaine": "Domaine Georges Joillot",
  "parcelle": "Les Charmots",        // optionnel
  "cuvee": null,                     // optionnel
  "format": "BTL",                   // 6 cl | 12 cl | 1/2 BTL | BTL | MAG | JERO | MATHU | NABU
  "millesime": "2020",
  "paHt": 42.77,
  "fournisseur": "Version Vin"
}
```

**Règles** :
- `parcelle` ou `cuvee` peuvent être ajoutées séparément ou ensemble selon le vin
- Si les deux sont présents, le rendu PDF affiche : *Appellation « Parcelle », Cuvée*
- Les formats sont **fermés** à la liste ci-dessus (pas de saisie libre)

---

## Troubleshooting

**L'app affiche "Chargement..." indéfiniment**
→ Vérifie que `produits.json` est bien présent à la racine du repo GitHub et que le JSON est valide (teste-le sur <https://jsonlint.com>).

**"Cette page est introuvable" après avoir activé GitHub Pages**
→ C'est normal dans les 2-3 premières minutes après l'activation. Rafraîchis plusieurs fois.

**Les modifs disparaissent en changeant de navigateur**
→ Comportement attendu en v1. Les données sont en localStorage, locales à chaque navigateur/appareil.

**Je veux réinitialiser mes données de test**
→ Dans ton navigateur, F12 (ouvre les outils dev) → onglet Application → Local Storage → sélectionne le domaine de l'app → clic droit → Clear. Puis recharge la page.

**Besoin d'aide ?**
→ Dis-moi précisément à quelle étape ça coince.

---

## Prochaines étapes (roadmap)

- [ ] Convertisseur CSV Yokitup → `produits.json` (comme pour ITM)
- [ ] Stockage partagé (Cloudflare D1 ou Google Sheets) pour que tous les utilisateurs voient les mêmes données en temps réel
- [ ] Envoi réel des alertes par email
- [ ] Import complet des 3215 articles du catalogue Yokitup
- [ ] Spiritueux et softs
- [ ] Gestion TVA spécifique par produit (pas seulement par établissement)
- [ ] Ajout du module sommellerie avec fiches techniques détaillées
