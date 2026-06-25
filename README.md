# 🛒 Walmart Weekly Sales — Prédiction des ventes par indicateurs macroéconomiques

![Python](https://img.shields.io/badge/Python-3.13-blue) ![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-ML-orange) ![Pandas](https://img.shields.io/badge/Pandas-Data-green) ![Plotly](https://img.shields.io/badge/Plotly-Dataviz-purple) ![Jedha](https://img.shields.io/badge/Bootcamp-Jedha-red)

> Projet réalisé dans le cadre du Bootcamp Data Analyst Fullstack — Jedha (RNCP Niveau 6)

📎 [Voir la page projet sur mon portfolio](https://isaacprdnt.com)

---

## 🎯 Contexte business

L'équipe marketing de Walmart doit planifier ses campagnes promotionnelles sans visibilité fiable sur les ventes à venir. Les ventes hebdomadaires varient fortement selon les magasins, les saisons et des facteurs macroéconomiques difficiles à anticiper (chômage, inflation, prix du carburant).

Un modèle prédictif pourrait transformer cette incertitude en avantage opérationnel — en permettant d'anticiper les creux et les pics, d'allouer les budgets là où ils ont le plus d'impact, et de lancer les campagnes au bon moment.

---

## ❓ Question centrale

**Dans quelle mesure les indicateurs macroéconomiques permettent-ils de prédire les ventes hebdomadaires d'un magasin Walmart — et lesquels comptent vraiment ?**

---

## 📊 Dataset

| Variable | Description |
|----------|-------------|
| Store | Identifiant du magasin |
| Weekly_Sales | Ventes hebdomadaires (cible) |
| Holiday_Flag | Semaine de jour férié (0/1) |
| Temperature | Température moyenne |
| Fuel_Price | Prix du carburant |
| CPI | Indice des prix à la consommation |
| Unemployment | Taux de chômage |

- **150 entrées** | **8 variables** | Source : Kaggle (version modifiée Jedha)

---

## 🔧 Méthodologie

### Étape 1 — EDA & Preprocessing

- Exploration et identification des valeurs manquantes sur 150 entrées, 8 variables
- Suppression des lignes où `Weekly_Sales` est manquante (imputation sur la cible = biais)
- Élimination des outliers via la règle ±3σ sur Temperature, Fuel_Price, CPI et Unemployment
- Feature Engineering : décomposition de `Date` en année, mois, jour, jour de semaine
- Encodage One-Hot de `Store` et `Holiday_Flag` via pipeline Scikit-Learn

### Étape 2 — Modèle Baseline (Régression Linéaire)

- Entraînement d'un modèle de régression linéaire et évaluation sur train/test
- Analyse des coefficients pour identifier les features les plus déterminantes
- **Résultats :** Train R² 0.982 | Test R² 0.965 | RMSE 126 960$
- **Conclusion :** Un R² de 0.965 dès le baseline — les relations entre indicateurs économiques et ventes sont suffisamment linéaires pour être capturées sans complexité supplémentaire

### Étape 3 — Régularisation & Optimisation (Ridge, Lasso & GridSearch)

- Entraînement Ridge et Lasso avec plusieurs alphas testés manuellement (0.01, 0.1, 1)
- GridSearchCV : automatisation de la sélection du meilleur alpha via cross-validation (5 folds, 6 valeurs testées)
- **Résultats finaux :** Train R² 0.982 | Test R² 0.966 | RMSE 126 965$
- **Conclusion :** Lasso (alpha = 0.01) retenu — performances quasi identiques au baseline (5$ de différence sur le RMSE), meilleure robustesse et sélection automatique des features inutiles

---

## 📈 Résultats comparatifs

| Modèle | Train R² | Test R² | Test RMSE |
|--------|----------|---------|-----------|
| Baseline (Linéaire) | 0.982 | 0.965 | 126 960$ |
| Ridge | 0.982 | 0.962 | 133 699$ |
| **Lasso α=0.01 ✅** | **0.982** | **0.966** | **126 965$** |

---

## 💡 Insights clés

- **Les jours fériés n'expliquent presque rien** (corrélation 0.037) — contrairement à l'intuition commune
- **Le CPI (inflation) est le vrai levier** : corrélation négative de -0.29 avec les ventes — quand les prix augmentent, les ventes baissent
- **Le chômage a un impact quasi nul** (0.055) — l'inflation est l'indicateur macroéconomique à surveiller pour un retailer, pas le taux de chômage
- **La régularisation n'améliore pas significativement les performances** — ce qui confirme que les relations entre variables sont propres et linéaires sur ce dataset

---

## 🚀 Prochaines étapes

- Intégration de variables exogènes (Google Trends, données météo granulaires, événements locaux) pour affiner les prédictions sur les pics saisonniers
- Dashboard Streamlit interactif permettant aux managers de simuler l'impact d'un scénario économique donné sur leurs prévisions de ventes

---

## 📁 Structure du repo

```
walmart-weekly-sales/
├── 00-Walmart_salesV1.ipynb   # Notebook complet EDA + modélisation
├── Walmart_Store_sales.csv    # Dataset
└── README.md
```

---

## 👤 Auteur

**Isaac Prudent** — Business & Data Analyst  
[Portfolio](https://isaacprdnt.com) | [LinkedIn](https://linkedin.com/in/isaac-prdnt/) | [GitHub](https://github.com/isaac-prdnt)
