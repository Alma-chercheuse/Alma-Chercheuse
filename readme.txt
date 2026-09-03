ALMA — SQUELETTE PWA (V3)
==========================

Contenu de ce zip
------------------
- index.html   : page unique de l'app (Accueil, Calendrier, Explorer,
                  Concepts, Journal, Parent). Données de démonstration
                  intégrées directement dans le fichier (objet ALMA_DATA).
- manifest.json: manifeste PWA (nom, couleurs, icônes déclarées).
- sw.js         : service worker minimal, met en cache l'app shell pour un
                  fonctionnement hors-ligne de base.
- readme.txt    : ce fichier.

Ce que fait ce squelette
-------------------------
- Navigation entre les 6 sections définies dans la spec V3.
- Affiche les 4 concepts de la semaine (données de démo, à remplacer par un
  vrai back-end / CMS).
- Permet de choisir un concept, de voir ses 4 niveaux d'accompagnement
  (Question / Indice / Expérience / Transfert).
- Fonctionnalité "Alma l'explique" : statut + phrase libre, enregistrés en
  local (localStorage du navigateur) et visibles dans le Journal.
- Conseil du jour avec l'avertissement pédagogique obligatoire déjà en place.
- Installable comme app (PWA) une fois hébergée en HTTPS.

Optimisations ajoutées (version "ludique et simple")
-------------------------------------------------------
Confort d'utilisation :
- Anneau de progression sur l'Accueil (X/4 concepts explorés cette semaine).
- Pastilles de couleur sur chaque concept indiquant son dernier statut.
- Bouton "Enregistrer" désactivé tant qu'aucun statut n'est choisi, avec
  libellé qui l'explique (évite l'alerte bloquante précédente).
- Message de confirmation discret (toast) au lieu d'une popup.
- Transitions douces entre les onglets, retour haptique léger (vibration
  courte) sur mobile lors des interactions clés.
- Petite animation de confettis (Canvas, sans dépendance externe) quand
  l'enfant explique avec ses propres mots ou avec de l'aide — renforcement
  positif ponctuel, pas systématique.
- Bouton "Installer" qui apparaît automatiquement quand le navigateur le
  permet (Android/desktop ; non disponible sur iOS, limitation du système).
- Respecte prefers-reduced-motion : les animations sont désactivées si
  l'utilisateur a demandé de réduire les animations dans son système.

Qualité de code (côté développeur) :
- Références DOM mises en cache une seule fois (objet "refs") au lieu de
  ré-interroger le DOM à chaque rendu.
- Rendu via DocumentFragment pour limiter les reflows.
- localStorage encapsulé avec gestion d'erreur (navigation privée, quota
  dépassé...) sans faire planter l'app.
- Attributs ARIA (role="tablist"/"tab", role="radiogroup"/"radio",
  aria-live sur le toast) pour l'accessibilité lecteur d'écran.
- Feuille de style organisée par zone fonctionnelle, variables CSS
  centralisées (couleurs de statut incluses) pour faciliter un futur thème.

Ce que ce squelette NE fait PAS (à construire ensuite)
--------------------------------------------------------
- Pas de back-end : les concepts, activités et conseils sont codés en dur
  dans index.html (objet ALMA_DATA), à connecter à une vraie source de
  données (API, base locale, CMS...) — voir le document
  "02-MODELE-DE-DONNEES-V3.md" fourni précédemment pour le schéma proposé.
- Pas de comptes / synchronisation entre appareils : les observations du
  Journal restent uniquement dans le navigateur de l'appareil utilisé
  (localStorage). Pas de sauvegarde cloud.
- Pas d'icônes réelles : le manifest.json référence icons/icon-192.png et
  icons/icon-512.png qui n'existent pas encore. Sans elles, l'app reste
  utilisable mais l'icône d'installation sera absente/générique.

Pour tester en local
----------------------
Un navigateur bloque les service workers sur les fichiers ouverts en
file:// direct. Il faut donc servir les fichiers via un petit serveur local.

Avec Python (déjà installé sur macOS/Linux) :
  cd alma-pwa
  python3 -m http.server 8000
Puis ouvrir : http://localhost:8000

Avec Node (si vous l'avez) :
  npx serve .

Pour déployer
--------------
1. Créer le dossier icons/ à la racine et y ajouter :
   - icon-192.png (192x192)
   - icon-512.png (512x512)
2. Héberger les 3 fichiers (+ dossier icons) sur un serveur HTTPS
   (GitHub Pages, Netlify, Vercel, etc. — HTTPS est obligatoire pour que le
   service worker et l'installation PWA fonctionnent, sauf en localhost).
3. Vérifier dans les outils développeur du navigateur (onglet
   Application/Manifest) que le manifeste et le service worker sont bien
   détectés.

Intégration GitHub (exemple)
------------------------------
  mkdir -p app/public
  # copier index.html, manifest.json, sw.js (et un futur dossier icons/)
  # dans app/public
  git checkout -b feat/pwa-skeleton-v3
  git add app/public
  git commit -m "feat: squelette PWA V3 (accueil, calendrier, concepts, journal)"
  git push -u origin feat/pwa-skeleton-v3
  # puis ouvrir la pull request sur GitHub

Prochaines étapes suggérées
------------------------------
1. Remplacer ALMA_DATA par un vrai appel à une source de données.
2. Ajouter les icônes réelles.
3. Remplacer le stockage localStorage du Journal par une synchronisation
   persistante si plusieurs appareils/parents doivent y accéder.
4. Écrire le contenu réel des concepts, activités et conseils en veillant à
   respecter la règle de rédaction du document
   "01-SPEC-FONCTIONNELLE-V3.md" (§2) sur les affirmations neuroscientifiques.
