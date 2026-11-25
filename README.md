# kanacodix.ttfl-dashboard.io
Un tableau de bord qui aide à la prise de décision en Trashtalk Fantasy League

# 🏀 Dashboard TTFL Interactif

Bienvenue sur le Dashboard TTFL ! Cet outil web permet de visualiser, d'analyser et de gérer vos performances sur votre ligue de Fantasy Basketball (TTFL). 

🌍 **Site en ligne :** [https://kanacodix.github.io/ttfl-dashboard.io/](https://kanacodix.github.io/ttfl-dashboard.io/)

---

## 📖 Guide pour les utilisateurs (Joueurs)

Le dashboard est divisé en plusieurs sections pour vous aider à faire les meilleurs choix.

### 1. Navigation et Profils
En haut de la page, une barre de navigation vous permet de sélectionner un profil spécifique (ex: *Arajiel*, *Kanatrophies*, etc.) ou de voir les statistiques de **Tous les profils**.
👉 **Astuce :** Cliquez à nouveau sur un profil déjà sélectionné pour revenir à la vue globale.

### 2. Règle des 30 jours (Disponibilité)
Dans le tableau des joueurs, chaque joueur possède un badge de statut calculé en temps réel :
*   🟢 **Dispo** : Le joueur n'a jamais été pris ou n'a pas été pris depuis plus de 30 jours.
*   🔴 **Indispo (Xj)** : Le joueur a été pris récemment. Le chiffre indique le nombre de jours restants avant de pouvoir le reprendre.

### 3. Catégories de joueurs
Le dashboard classe automatiquement les meilleurs joueurs disponibles selon 6 styles :
*   **Réguliers** : Les meilleures moyennes de score TTFL sur la saison.
*   **Explosifs** : Les plus gros plafonds (score atteint ou dépassé dans 20% des matchs).
*   **Valeurs sûres** : Le meilleur ratio moyenne / écart-type (peu de variations, très constant).
*   **Bon marché (min)** : Le meilleur rendement par minute jouée (idéal pour les joueurs à faible temps de jeu).
*   **Forme récente** : La meilleure moyenne sur les 5 derniers matchs.
*   **Volumes rebonds** : Les joueurs qui captent le plus de rebonds.

### 4. Graphiques interactifs (Nuages de points)
Les graphiques sont entièrement interactifs :
*   **Rechercher** : Tapez le nom d'un joueur ou d'une équipe dans la barre de recherche. Ses points apparaîtront en gros sur tous les nuages de points avec son nom affiché.
*   **Zoomer / Se déplacer** : Utilisez la molette de votre souris pour zoomer et cliquez-glisser pour vous déplacer.
*   **Filtrer** : Cliquez sur une ligne dans les graphiques d'évolution pour isoler un profil. Cliquez sur une barre ou un point pour rechercher automatiquement le joueur.

---

## 🛠️ Guide pour l'administrateur (Mise à jour du site)

Pour mettre à jour les données du site public, vous devez effectuer ces étapes dans l'ordre :

### Étape 1 : Importer les Boxscores
Allez dans la section **"Importer des boxscores"**.
*   **Fichier Excel (.xlsx)** : Vous pouvez déposer un fichier au format Basketball-Reference (un onglet par date au format `AAAA.MM.JJ`). Le script extrait automatiquement les statistiques.
*   **Fichier CSV** : Vous pouvez coller ou déposer un CSV plat contenant les colonnes : `Date, Joueur, Equipe, Adversaire, Domicile, Minutes, FGM, FGA, 3PM, 3PA, FTM, FTA, REB, AST, STL, BLK, TOV, PTS, Statut`.
*Les minutes au format `35:12` sont automatiquement converties.*

### Étape 2 : Ajouter les Picks
*   **Manuellement** : Utilisez le formulaire "Ajouter un pick à la main".
*   **En masse** : Importez un fichier CSV/TSV avec les colonnes `Date, Joueur, Pts, Reb, Ast, Stl, Blk, Ftm, Fgm, Fg3m, Malus, Score`.

### Étape 3 : Exporter les données
Une fois vos modifications terminées, allez tout en bas de la section d'importation et cliquez sur **"Exporter data.json"**.
Cela télécharge un fichier `data.json` contenant l'intégralité de vos profils et boxscores.

### Étape 4 : Mettre à jour GitHub
1. Remplacez l'ancien fichier `data.json` de votre dépôt GitHub par le nouveau.
2. Faites un `commit` et un `push`.
3. Attendez environ 30 secondes que GitHub Pages mette à jour le site.

---

## 📐 Règles de calcul

### Formule de score TTFL
Le score est calculé selon la formule officielle :
> **Points** + **Rebonds** + **Passes** + **Interceptions** + **Contres** + **Tirs réussis** + **3pts réussis** + **LF réussis**
> **- (Moins)** **Balles perdues** + **Tirs ratés** + **3pts ratés** + **LF ratés**

### Gestion des noms de joueurs
Le script est extrêmement tolérant sur les noms. Il ignore les accents, les majuscules et la ponctuation. 
*Exemple : "Luka Doncic", "Luka Dončić" et "luka doncic" seront fusionnés dans les mêmes statistiques.*

---

## 💻 Pour les développeurs

Le projet est constitué de deux fichiers principaux à la racine :
1.  **`index.html`** : Contient toute la logique (HTML, CSS, JS). Les librairies utilisées sont Chart.js (graphiques), Chart.js Zoom (interactivité) et SheetJS (lecture Excel).
2.  **`data.json`** : La base de données. Ce fichier est chargé via un `fetch('./data.json')` asynchrone au démarrage de la page.

### Structure du `data.json`
```json
{
  "version": 1,
  "profiles": {
    "NomDuProfil": [
      { "iso": "2026-10-20", "j": "Joueur", "s": 25, "m": 14, "l": "15/5/3" }
    ]
  },
  "box": {
    "2026-10-20": [
      { "j": "Joueur", "t": "LAL", "mn": 32.5, "pts": 20, "reb": 5, "ast": 3, "fgm": 8, "fga": 15, "tpm": 2, "tpa": 5, "ftm": 2, "fta": 2, "stl": 1, "blk": 0, "tov": 2 }
    ]
  }
}

*Note : Le tableau `box` accepte également l'ancien format sous forme de tableaux (`["Joueur", "LAL", ...]`) pour des raisons de compatibilité.*
