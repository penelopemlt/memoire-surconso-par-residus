# Détection de surconsommations résidentielles par analyse des résidus de prévision

*Code du mémoire M2 IASD — Pénélope Millet (2024–2025)*

Ce dépôt contient le notebook associé au mémoire **« Détection de surconsommations résidentielles par analyse des résidus de prévision »**. L’approche consiste à : (1) **prédire la consommation** d’un foyer avec une base simple (persistance/lags + régression linéaire + RandomForest), (2) **calculer les résidus** (vérité − prédiction) et (3) **détecter les surconsommations** via des **seuils de quantiles** calibrés globalement ou par groupes (par défaut : *house\_id × hour\_of\_week*). Les anomalies ponctuelles sont ensuite **agrégées en épisodes** avec le calcul de différentes métriques.

---

## Contenu

* `Memoire_M2_IASD_Millet_Penelope.ipynb` — notebook principal.
*  REFIT_15min.csv — version nettoyée et agrégée (15 min) du jeu de données, obtenue après préparation des données.
  
### Format des données

| colonne     | type         | description                                    |
| ----------- | ------------ | ---------------------------------------------- |
| `house_id`  | str          | identifiant du foyer                           |
| `Datetime`  | datetime ISO | horodatage (pas de 15 min)                     |
| `Aggregate` | float        | puissance/énergie agrégée (W)                  |

Par défaut, le notebook lit data/REFIT_15min.csv. Si vous déplacez/renommez le fichier, adaptez la constante DATA_PATH tout en haut du notebook.

```python
# Dans le notebook (cellule de config)
DATA_PATH = 'data/REFIT_15min.csv'
```

## Installation

Prérequis : **Python ≥ 3.10**


**`requirements.txt`**

```txt
pandas>=2.0
numpy>=1.24
scikit-learn>=1.5
matplotlib>=3.7
joblib>=1.3
```

## Utilisation 

1. Téléchargez/preparez le CSV au **format** ci‑dessus, mettez‑le à jour dans `DATA_PATH`.
2. Ouvrez le notebook `Memoire_M2_IASD_Millet_Penelope.ipynb` et exécutez les cellules **dans l’ordre**.
3. Paramètres clés dans le notebook :

   * **Features** : lags (J−7, J−1h, etc.), moyennes glissantes causales, codages cycliques.
   * **Modèles testés** : `LagBaselineRegressor` (persistance par feature de lag) + `LinearRegression` + `RandomForest`
   * **Détection** : quantiles sur résidus positifs (global vs **`GroupedPercentileDetector`** groupé par `house_id × hour_of_week`).
   * **Modes** : `detect_zero_shot` (calibré population) et `detect_progressive` (recalibrage périodique par fenêtre).
   * **Agrégation** : `build_anomaly_episodes` calcule les épisodes & l’**excess\_overuse\_kWh** ainsi que d'autres métriques.


---

