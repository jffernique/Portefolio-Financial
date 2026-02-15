# Portfolio Manager Pro v3

> Gestionnaire de portefeuille boursier avancé avec analyse technique, calcul de renforcement PRU, simulation ROI dividendes et news RSS en temps réel.

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white)
![PyQt5](https://img.shields.io/badge/GUI-PyQt5-green?logo=qt&logoColor=white)
![Yahoo Finance](https://img.shields.io/badge/Data-Yahoo%20Finance-purple)
![License](https://img.shields.io/badge/License-MIT-yellow)

---

## Sommaire

- [Aperçu](#aperçu)
- [Fonctionnalités](#fonctionnalités)
- [Installation](#installation)
- [Lancement](#lancement)
- [Guide d'utilisation](#guide-dutilisation)
  - [Fenêtre principale](#fenêtre-principale)
  - [Gestion des positions](#gestion-des-positions)
  - [Analyse technique](#analyse-technique)
  - [Calcul de renforcement PRU](#calcul-de-renforcement-pru)
  - [ROI Dividendes](#roi-dividendes)
  - [Bandeau news RSS](#bandeau-news-rss)
  - [Export de données](#export-de-données)
  - [Sauvegarde et restauration](#sauvegarde-et-restauration)
- [Raccourcis clavier](#raccourcis-clavier)
- [Colonnes du tableau](#colonnes-du-tableau)
- [Configuration](#configuration)
- [Architecture technique](#architecture-technique)
- [Dépendances](#dépendances)
- [Contribution](#contribution)
- [Donation](#donation)
- [Licence](#licence)

---

## Aperçu

Portfolio Manager Pro est une application desktop PyQt5 conçue pour suivre un portefeuille boursier en temps réel, analyser les opportunités de renforcement et simuler l'impact des dividendes sur le retour sur investissement. L'interface adopte un thème sombre moderne inspiré des terminaux de trading.

**Caractéristiques principales :**
- Suivi multi-devises (EUR, USD, GBP) avec conversion automatique
- 25 colonnes de données par position dont 6 périodes de variation
- Analyse technique avancée avec 3 indicateurs (RSI, MACD, Bollinger)
- Calcul de renforcement PRU avec simulation multi-paliers
- Simulation ROI dividendes avant/après renforcement
- Bandeau défilant de news RSS Yahoo Finance
- Interface non-bloquante grâce au threading

---

## Fonctionnalités

### Suivi de portefeuille
- **25 colonnes** de données par position (libellé, ticker, secteur, parts, prix d'achat, cours actuel, variation, gain/perte, poids, variations sur 6 périodes, dividendes, 52 semaines, volume, sparkline, notes)
- **Pastille de signal** : indicateur visuel (🟢🟡🟠) sur chaque ligne pour repérer les opportunités de renforcement PRU
- **Sparklines** : mini-graphiques de tendance 30 jours intégrés dans le tableau
- **Multi-devises** : conversion automatique EUR/USD/GBP via l'API Frankfurter
- **Tri et recherche** : toutes les colonnes sont triables, barre de recherche temps réel

### Analyse technique
- **RSI (14)** : détection des zones survendues (<30) et surachetées (>70)
- **MACD (12, 26, 9)** : détection des retournements de momentum
- **Bandes de Bollinger (20, 2)** : identification des excès baissiers
- **Score composite 0-100** : synthèse des 3 indicateurs avec recommandation contextuelle
- **Graphiques** : visualisation QPainter des 3 indicateurs sur 120 jours

### Calcul de renforcement PRU
- **Pourcentage ajustable** : de 0.1% à 50% de baisse du PRU souhaitée
- **Calcul exact** : nombre d'actions, coût total (nb × prix actuel), nouveau PRU
- **Tableau multi-paliers** : simulation simultanée sur 7 paliers (0.5%, 1%, 2%, 3%, 5%, 7%, 10%)

### ROI Dividendes
- **Comparaison avant/après** renforcement côte à côte
- **Impact du renforcement** sur le dividende annuel (plus de parts = plus de dividendes)
- **Projection année par année** sur 12 à 25 ans avec PRU effectif et indicateur de rentabilité
- **Métriques complètes** : rendement sur PRU, dividende mensuel, temps de couverture de la perte

### News RSS
- **Bandeau défilant** en bas de la fenêtre principale
- **Flux Yahoo Finance** pour chaque ticker du portefeuille
- **Interactif** : pause au survol, clic pour ouvrir l'article, molette pour la vitesse

### Export et sauvegarde
- **CSV** : export semicolonne UTF-8-BOM (compatible Excel FR)
- **Rapport HTML** : rapport stylisé avec résumé et tableau complet
- **Sauvegarde/Restauration** : backup JSON horodaté

---

## Installation

### Prérequis

- Python 3.8 ou supérieur
- pip (gestionnaire de paquets Python)

### Installation des dépendances

```bash
pip install PyQt5 yfinance pandas numpy requests
```

### Dépendance optionnelle (QR code pour la page de donation)

```bash
pip install qrcode[pil]
```

### Cloner le projet

```bash
git clone https://github.com/VOTRE_UTILISATEUR/portfolio-manager-pro.git
cd portfolio-manager-pro
```

---

## Lancement

```bash
python main4evo3.py
```

Au premier lancement, un fichier `positions.json` sera créé automatiquement. L'application démarre avec un tableau vide — appuyez sur **Ctrl+N** pour ajouter votre première position.

---

## Guide d'utilisation

### Fenêtre principale

La fenêtre principale se compose de :

1. **Barre de menu** : Fichier, Portfolio, Données, Aide
2. **Barre d'outils** : boutons Ajouter, Modifier, Supprimer, Rafraîchir, Analyser + barre de recherche
3. **Panneau de résumé** : 6 métriques globales (investissement, valeur, gain, performance, dividendes, rendement moyen)
4. **Tableau des positions** : 25 colonnes triables avec pastille de signal et sparklines
5. **Barre de progression** : affichée pendant le chargement des données
6. **Bandeau news RSS** : défilant en bas avec les dernières actualités Yahoo Finance
7. **Barre de statut** : nombre de positions, taux EUR/USD

### Gestion des positions

**Ajouter une position (Ctrl+N)** : ouvre un dialogue avec les champs libellé, ticker Yahoo Finance (ex: `AAPL`, `AI.PA`, `BTC-USD`), secteur, nombre de parts, prix d'achat, date, notes et seuils d'alerte.

**Modifier (Ctrl+M ou double-clic)** : modifie la position sélectionnée. Les données de marché sont conservées.

**Supprimer (Delete)** : supprime après confirmation.

**Dupliquer (Ctrl+D)** : crée une copie de la position sélectionnée.

**Rafraîchir (F5)** : charge les données de marché pour toutes les positions via Yahoo Finance. Le chargement est non-bloquant (threading) avec barre de progression. Les news RSS sont chargées automatiquement après le rafraîchissement.

**Rafraîchir une position (Ctrl+R)** : rafraîchit uniquement la position sélectionnée.

### Analyse technique

Sélectionnez une position puis cliquez sur **Analyser (Ctrl+T)** ou clic droit → Analyse technique.

La fenêtre d'analyse comporte 4 onglets :

#### Onglet 1 : Signal de renforcement

Un score composite de 0 à 100 basé sur les 3 indicateurs techniques calculés sur les données daily :

| Indicateur | Poids | Signal d'achat |
|---|---|---|
| RSI (14) | 40 pts | RSI < 30 (survendu) |
| MACD (12, 26, 9) | 30 pts | Histogramme en retournement |
| Bollinger (20, 2) | 30 pts | Prix proche bande basse |

Interprétation du score :

| Score | Verdict |
|---|---|
| 75-100 | 🟢 Signal fort — Excellent moment pour renforcer |
| 55-74 | 🟢 Signal modéré — Conditions favorables |
| 35-54 | 🟡 Neutre — Attendre de meilleures conditions |
| 20-34 | 🟠 Défavorable — Pas le bon moment |
| 0-19 | 🔴 Éviter — Conditions très défavorables |

#### Onglet 2 : Calcul renforcement PRU

Ajustez le pourcentage de baisse souhaité (0.1% à 50%) et le calcul donne :

- **Nombre d'actions** à acheter au prix actuel
- **Coût total** exact (nombre × prix)
- **Nouveau PRU** réel après renforcement
- **Tableau multi-paliers** : simulation sur 7 pourcentages simultanément

**Formule utilisée :**

```
n = parts_actuelles × (PRU - PRU_cible) / (PRU_cible - prix_marché)
PRU_cible = PRU × (1 - pourcentage / 100)
```

Le nombre d'actions est arrondi au supérieur (`ceil`), puis le coût et le nouveau PRU sont recalculés avec ce nombre entier.

#### Onglet 3 : ROI Dividendes

Compare le scénario actuel vs le scénario avec renforcement :

**Panneau gauche (SANS renforcement)** : parts, PRU, investi, dividende annuel/mensuel, rendement sur PRU, perte latente, temps de couverture, ROI total.

**Panneau droit (AVEC renforcement)** : mêmes métriques recalculées avec les nouvelles parts. Plus de parts = plus de dividendes annuels = ROI accéléré.

**Tableau de projection** : année par année sur 12-25 ans avec dividendes cumulés, % investi récupéré, % perte couverte, gain net, PRU effectif (PRU − dividendes cumulés/action), et indicateur de rentabilité (✅/❌).

#### Onglet 4 : Graphiques indicateurs

3 graphiques QPainter sur les 120 derniers jours :

- **RSI** : courbe avec seuils 30/50/70
- **MACD** : ligne MACD + ligne signal + zéro
- **Bollinger** : prix + SMA20 + bandes sup/inf avec remplissage

### Bandeau news RSS

Le bandeau en bas de la fenêtre affiche les dernières news Yahoo Finance pour vos tickers.

- **Chargement** : automatique après chaque F5
- **Pause** : survolez le bandeau avec la souris
- **Ouvrir** : cliquez sur une news pour l'ouvrir dans le navigateur
- **Vitesse** : molette de la souris (1 à 10)
- Les tickers apparaissent en vert, les dates en gris

### Export de données

**Export CSV (Ctrl+E)** : exporte toutes les positions en CSV semicolonne, encodage UTF-8-BOM pour compatibilité Excel français.

**Export HTML (Ctrl+Shift+E)** : génère un rapport HTML stylisé avec les cartes de résumé (investissement, valeur, gain, performance) et le tableau complet des positions avec couleurs.

### Sauvegarde et restauration

**Fichier → Créer une sauvegarde** : copie le fichier `positions.json` avec un horodatage (ex: `positions_backup_20260215_143022.json`).

**Fichier → Restaurer une sauvegarde** : remplace les positions actuelles par celles du fichier de sauvegarde sélectionné (confirmation demandée).

---

## Raccourcis clavier

| Raccourci | Action |
|---|---|
| `F5` | Rafraîchir toutes les positions |
| `Ctrl+N` | Ajouter une position |
| `Ctrl+M` | Modifier la position sélectionnée |
| `Ctrl+D` | Dupliquer la position |
| `Ctrl+R` | Rafraîchir la sélection |
| `Ctrl+T` | Analyse technique & renforcement PRU |
| `Ctrl+E` | Exporter en CSV |
| `Ctrl+Shift+E` | Exporter rapport HTML |
| `Delete` | Supprimer la position |
| `Ctrl+H` | Afficher les raccourcis |
| `Ctrl+Q` | Quitter |
| `Double-clic` | Modifier une position |
| `Clic droit` | Menu contextuel (modifier, dupliquer, analyser, supprimer) |

---

## Colonnes du tableau

| # | Colonne | Description |
|---|---|---|
| 0 | Pastille | Signal de renforcement PRU (🟢🟡🟠 ou vide) |
| 1 | Libellé | Nom de la position |
| 2 | Ticker | Symbole Yahoo Finance |
| 3 | Secteur | Secteur d'activité (auto-détecté) |
| 4 | Parts | Nombre de parts détenues |
| 5 | Achat | Prix d'achat (PRU) |
| 6 | Courant | Cours actuel |
| 7 | Δ Value | Variation en valeur (courant − achat) |
| 8 | % Δ | Variation en pourcentage |
| 9 | Gain/Loss | Gain ou perte total (variation × parts) |
| 10 | Poids (%) | Poids de la position dans le portefeuille |
| 11-16 | 1J / 1S / 1M / 3M / 6M / 1A | Variations sur 6 périodes |
| 17 | Rdt Div. (%) | Rendement dividende annuel |
| 18 | Div. Annuel | Dividende annuel par action |
| 19 | Total Div. | Dividende annuel total (div × parts) |
| 20 | 52s Haut | Plus haut sur 52 semaines |
| 21 | 52s Bas | Plus bas sur 52 semaines |
| 22 | Vol. Moy. | Volume moyen 20 jours |
| 23 | Tendance 30j | Sparkline mini-graphique |
| 24 | Notes | Notes personnelles |

### Pastille de signal

| Pastille | Condition | Signification |
|---|---|---|
| 🟢 | Prix < PRU et tendance 1M < -3% | Signal fort : bon timing potentiel |
| 🟡 | Prix < PRU et tendance 1M entre 0% et -3% | Renforcement possible, confirmer par l'analyse |
| 🟠 | Prix < PRU et tendance 1M > 0% | Prix sous le PRU mais en remontée |
| (vide) | Prix ≥ PRU | Renforcement non pertinent |

---

## Configuration

### Fichier positions.json

Le fichier `positions.json` est créé automatiquement et sauvegardé en JSON indenté (lisible). Chaque position contient :

```json
{
    "alert_high": 0,
    "alert_low": 0,
    "label": "Apple",
    "notes": "",
    "purchase_date": "2024-01-15",
    "purchase_price": 185.50,
    "sector": "Technology",
    "shares": 10.0,
    "ticker": "AAPL"
}
```

Les données de marché (`history`) sont volatiles et ne sont pas sauvegardées — elles sont rechargées à chaque rafraîchissement.

### Fichier de log

L'application génère un fichier `portfolio.log` avec les événements (chargements, erreurs, sauvegardes).

### Géométrie de la fenêtre

La taille et la position de la fenêtre sont sauvegardées automatiquement entre les sessions via `QSettings`.

### Adresse de donation BTC

Pour configurer votre adresse de donation Bitcoin, modifiez la variable en haut de la méthode `_show_donation` dans le code :

```python
BTC_ADDRESS = "bc1qVOTREADRESSEBTCici"
```

---

## Architecture technique

```
main4evo3.py (fichier unique ~3200 lignes)
│
├── Utilitaires
│   └── clean_numeric()          # Nettoyage robuste des valeurs numériques
│
├── Workers (QThread)
│   ├── DataWorker               # Chargement Yahoo Finance (non-bloquant)
│   └── RssWorker                # Chargement flux RSS Yahoo Finance
│
├── Widgets personnalisés
│   ├── NumericTableWidgetItem    # Item avec tri numérique
│   ├── SparklineWidget           # Mini-graphique 30 jours en cellule
│   ├── ModernProgressBar         # Barre de progression stylisée
│   ├── IndicatorChartWidget      # Graphiques d'indicateurs techniques
│   └── NewsTickerWidget          # Bandeau défilant RSS
│
├── Moteur d'analyse
│   └── TechnicalAnalysis         # RSI, MACD, Bollinger, score, calcul PRU
│
├── Dialogues
│   ├── PositionDialog            # Ajout/modification de position
│   └── AnalysisDialog            # Analyse technique + PRU + ROI + graphiques
│
└── Fenêtre principale
    └── PositionTracker (QMainWindow)
        ├── Menu bar, toolbar, status bar
        ├── Résumé portefeuille (6 métriques)
        ├── Tableau 25 colonnes (triable, filtrable)
        ├── Système d'alertes prix
        ├── Export CSV / HTML
        ├── Sauvegarde / restauration
        └── Bandeau news RSS
```

### Technologies

| Composant | Technologie |
|---|---|
| Interface graphique | PyQt5 (Fusion + palette sombre) |
| Données de marché | Yahoo Finance (yfinance) |
| Taux de change | Frankfurter API |
| News | Yahoo Finance RSS (xml.etree) |
| Analyse technique | NumPy + Pandas |
| Graphiques | QPainter (rendu natif) |
| Persistence | JSON (positions) + QSettings (fenêtre) |
| Threading | QThread (DataWorker, RssWorker) |

---

## Dépendances

| Package | Version min. | Utilisation |
|---|---|---|
| `PyQt5` | 5.15+ | Interface graphique |
| `yfinance` | 0.2+ | Données de marché Yahoo Finance |
| `pandas` | 1.3+ | Manipulation de données, EMA, SMA |
| `numpy` | 1.21+ | Calculs RSI, tableaux numériques |
| `requests` | 2.25+ | API taux de change, flux RSS |
| `qrcode` | 7.0+ | *(optionnel)* QR code page donation |
| `Pillow` | 8.0+ | *(optionnel)* Rendu QR code en image |

Tous les autres modules utilisés (`json`, `csv`, `math`, `xml.etree`, `logging`, `shutil`, `copy`, `re`) font partie de la bibliothèque standard Python.

---

## Contribution

Les contributions sont les bienvenues ! Pour contribuer :

1. Forkez le projet
2. Créez une branche (`git checkout -b feature/ma-fonctionnalite`)
3. Committez vos changements (`git commit -m 'Ajout de ma fonctionnalité'`)
4. Poussez la branche (`git push origin feature/ma-fonctionnalite`)
5. Ouvrez une Pull Request

### Idées de contributions

- Support de nouveaux brokers / sources de données
- Graphiques interactifs (matplotlib / plotly intégrés)
- Alertes par notification système
- Import depuis fichiers CSV de brokers
- Mode multi-portefeuille
- Tests unitaires

---

## Donation

Si ce projet vous est utile, vous pouvez soutenir son développement en faisant un don en Bitcoin :

**Adresse BTC :** `bc1qVOTREADRESSEBTCici`

Vous pouvez aussi accéder à la page de donation directement dans l'application via **Aide → Faire un don (BTC)** avec QR code intégré.

Merci pour votre soutien !

---

## Licence

Ce projet est distribué sous licence MIT. Voir le fichier [LICENSE](LICENSE) pour plus de détails.

---

*Développé avec Python, PyQt5 et beaucoup de café.*
