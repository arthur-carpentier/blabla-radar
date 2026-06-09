# BlaBla Radar

Suivi en **temps réel** des autocars **BlaBlaCar Bus** sur une carte interactive.
Application web statique, en un seul fichier, sans build ni serveur : il suffit
d'ouvrir la page dans un navigateur.

🔗 **Démo en ligne :** https://arthur-carpentier.github.io/blabla-radar/

![Statut](https://img.shields.io/badge/statut-actif-00ff88)
![Licence](https://img.shields.io/badge/licence-MIT-ff2bd6)
![Sans dépendance build](https://img.shields.io/badge/build-aucun-00f0ff)

---

## ✨ Fonctionnalités

- **Données temps réel** via l'API ouverte **SIRI-Lite** de BlaBlaCar
  (`estimated-timetable`) : horaires théoriques, horaires estimés et retards
  par arrêt.
- **Suivi multi-lignes** : ajoutez un ou plusieurs numéros de ligne, ils sont
  mémorisés localement (`localStorage`) pour les sessions suivantes.
- **Trajet aléatoire** : pas de numéro sous la main ? Un clic sur 🎲 choisit au
  hasard une ligne dont le trajet est **en cours** dans le flux temps réel
  (au moins un arrêt déjà desservi et au moins un arrêt restant).
- **Timeline des arrêts** : arrêts effectués, prochain arrêt et arrêts à venir,
  avec l'écart à l'horaire (avance / retard) affiché par arrêt.
- **Barre de progression du trajet** : pourcentage de trajet parcouru, estimé
  à partir des arrêts pointés ou de la position GPS réelle.
- **Carte Leaflet** : tracé du trajet, arrêts géolocalisés et position
  interpolée du bus le long de l'itinéraire.
- **Projection GPS** : votre position est projetée orthogonalement sur le
  segment de l'étape en cours pour estimer l'avancement réel et la distance
  restante, avec un panneau de « raisonnement » détaillant chaque étape du
  calcul.

## 🧭 Comment ça marche

1. **Récupération des données** — l'app interroge l'endpoint SIRI-Lite de
   BlaBlaCar :
   `https://open-data.bus.blablacar.com/api/siri-lite/estimated-timetable`.
   Comme l'API ne renvoie pas d'en-têtes CORS, plusieurs **proxys CORS** sont
   essayés en cascade (appel direct, puis `corsproxy.io`, `allorigins`,
   `codetabs`) jusqu'à obtenir une réponse.
2. **Géocodage des arrêts** — les noms d'arrêts sont convertis en coordonnées
   GPS via **Nominatim** (OpenStreetMap), avec mise en cache local et respect
   de la limite de débit du service.
3. **Projection sur le trajet** — la position de l'utilisateur (P) est projetée
   sur le segment [A, B] de l'étape courante à l'aide d'une projection
   équirectangulaire centrée sur le milieu du segment. On en déduit la fraction
   `t ∈ [0, 1]` parcourue sur l'étape, la distance restante et la position
   interpolée du bus affichée sur la carte.

## 🚀 Utilisation

Aucune installation : ouvrez simplement la page.

- **En ligne :** https://arthur-carpentier.github.io/blabla-radar/
- **En local :**

  ```bash
  git clone https://github.com/arthur-carpentier/blabla-radar.git
  cd blabla-radar
  # ouvrez index.html dans votre navigateur, ou servez le dossier :
  python3 -m http.server 8000
  # puis visitez http://localhost:8000
  ```

1. Saisissez un **numéro de ligne** BlaBlaCar Bus, ou cliquez sur
   **🎲 Trajet aléatoire** pour suivre une ligne en cours de trajet.
2. Les lignes actives s'affichent avec leur timeline et leur carte.
3. Activez le **GPS** pour projeter votre position réelle sur le trajet.

> ℹ️ La géolocalisation nécessite un contexte sécurisé (HTTPS), ce que fournit
> GitHub Pages.

## 🛠️ Stack technique

| Élément        | Détail                                                        |
| -------------- | ------------------------------------------------------------- |
| Front          | HTML / CSS / JavaScript natif, fichier unique `index.html`    |
| Cartographie   | [Leaflet](https://leafletjs.com/) 1.9.4 + tuiles OpenStreetMap |
| Données bus    | API SIRI-Lite BlaBlaCar (open data)                           |
| Géocodage      | [Nominatim](https://nominatim.openstreetmap.org/) (OSM)       |
| Persistance    | `localStorage`                                                |
| Hébergement    | GitHub Pages (branche `main`, racine)                         |

## 📡 Sources de données

- **BlaBlaCar Bus — Open Data SIRI-Lite :**
  https://open-data.bus.blablacar.com/api/siri-lite/estimated-timetable
- **OpenStreetMap / Nominatim** pour le géocodage et les fonds de carte.

Ce projet n'est pas affilié à BlaBlaCar. Les marques et données appartiennent à
leurs propriétaires respectifs ; consultez les conditions d'utilisation des API
concernées.

## 📄 Licence

Distribué sous licence **MIT**. Voir le fichier [LICENSE](LICENSE).
