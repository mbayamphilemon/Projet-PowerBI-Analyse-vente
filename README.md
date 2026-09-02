 # 📊 Analyse des ventes avec Microsoft Power BI

## 📌 Présentation du projet

Dans le cadre de ce projet, une entreprise de distribution souhaite mettre en place un **tableau de bord décisionnel** permettant de suivre ses ventes, son chiffre d'affaires et sa rentabilité.

L'objectif est de construire un rapport Power BI **professionnel, interactif et orienté décision**, permettant à la direction d'identifier les tendances commerciales, les produits performants, les régions les plus rentables, les performances des commerciaux ainsi que le comportement des différents segments clients.

---

# 🎯 Objectifs

Ce projet vise à :

* Suivre le **chiffre d'affaires** ;
* Analyser le **bénéfice et la rentabilité** ;
* Suivre les **quantités vendues** ;
* Analyser l'évolution des performances dans le temps ;
* Comparer les régions ;
* Comparer les catégories et produits ;
* Évaluer les performances des commerciaux ;
* Analyser les segments clients ;
* Étudier les modes de paiement ;
* Identifier les opportunités et les points de faiblesse ;
* Formuler des **recommandations business** à partir des données.

---

# 🗃️ Données utilisées

Le modèle repose sur une architecture composée d'une table de faits et de plusieurs tables de dimensions.

### 📦 Fact_Ventes

Contient les transactions de vente.

Principales informations :

* ID de vente
* Date
* Produit
* Client
* Commercial
* Quantité
* Prix unitaire
* Coût unitaire
* Mode de paiement

### 🏷️ Dim_Produit

Référentiel des produits et de leurs catégories.

### 👥 Dim_Client

Informations relatives aux clients et à leurs segments.

### 👨‍💼 Dim_Commercial

Informations sur les commerciaux et leurs régions.

### 📅 Dim_Date

Calendrier permettant l'analyse temporelle des ventes.

---

# 🧹 1. Nettoyage des données — Power Query

Les données fournies contiennent volontairement quelques anomalies afin de mettre en pratique les opérations de préparation et de nettoyage des données.

Les principales étapes réalisées sont :

* Vérification des types de données ;
* Traitement des valeurs nulles ;
* Suppression des doublons ;
* Harmonisation des libellés ;
* Suppression des colonnes inutiles ;
* Contrôle des valeurs aberrantes.

Cette étape permet de garantir la **qualité et la fiabilité des données** avant leur exploitation dans Power BI.

---

# 🏗️ 2. Modélisation des données

Le projet utilise un **modèle en étoile** avec `Fact_Ventes` au centre et les différentes dimensions autour.

```text
                  ┌───────────────┐
                  │  Dim_Produit  │
                  └───────┬───────┘
                          │
                          │
┌──────────────┐    ┌─────▼──────┐    ┌────────────────┐
│  Dim_Client  │────│ Fact_Ventes│────│ Dim_Commercial │
└──────────────┘    └─────┬──────┘    └────────────────┘
                          │
                          │
                  ┌───────▼───────┐
                  │    Dim_Date   │
                  └───────────────┘
```

Les relations sont établies à partir des identifiants appropriés, notamment entre :

`Fact_Ventes[Date]` → `Dim_Date[Date]`

Cette modélisation facilite les analyses croisées et garantit une meilleure organisation du modèle décisionnel.

---

# 🧮 3. Mesures DAX

Plusieurs mesures ont été créées afin de construire les indicateurs du dashboard.

### Chiffre d'affaires

```DAX
CA =
SUMX(
    Fact_Ventes,
    Fact_Ventes[Quantite] * Fact_Ventes[Prix_Unitaire]
)
```

### Coût total

```DAX
Cout Total =
SUMX(
    Fact_Ventes,
    Fact_Ventes[Quantite] * Fact_Ventes[Cout_Unitaire]
)
```

### Bénéfice

```DAX
Benefice =
[CA] - [Cout Total]
```

### Taux de marge

```DAX
Taux de marge =
DIVIDE(
    [Benefice],
    [CA]
)
```

### Quantité vendue

```DAX
Quantite vendue =
SUM(Fact_Ventes[Quantite])
```

### Nombre de ventes

```DAX
Nombre de ventes =
DISTINCTCOUNT(Fact_Ventes[ID_Vente])
```

### Panier moyen

```DAX
Panier moyen =
DIVIDE(
    [CA],
    [Nombre de ventes]
)
```

Des mesures complémentaires d'évolution temporelle ont également été utilisées pour analyser la progression du chiffre d'affaires dans le temps.

---

# 📊 4. Dashboard

Le rapport Power BI est organisé autour de **quatre pages d'analyse**.

## 🏠 Page 1 — Vue générale

Cette page fournit une vision globale de la performance commerciale.

### KPI

* Chiffre d'affaires
* Bénéfice
* Taux de marge
* Quantité vendue

### Analyses

* Évolution mensuelle du chiffre d'affaires ;
* Évolution mensuelle du bénéfice ;
* Chiffre d'affaires par région ;
* Chiffre d'affaires par catégorie.

### Filtres interactifs

* Année
* Région
* Catégorie
* Commercial
* Mode de paiement

---

# 📦 Page 2 — Analyse des produits

Cette page permet d'identifier les produits qui contribuent le plus à la performance commerciale.

### Analyses

* Top 10 produits par chiffre d'affaires ;
* Top 10 produits par bénéfice ;
* Comparaison chiffre d'affaires / bénéfice ;
* Identification des produits générant un chiffre d'affaires élevé mais une faible marge.

L'objectif est de distinguer les produits qui **vendent beaucoup** de ceux qui sont réellement **rentables**.

---

# 👨‍💼 Page 3 — Analyse commerciale

Cette page est consacrée à l'analyse des performances des commerciaux.

### Indicateurs

* Chiffre d'affaires par commercial ;
* Bénéfice par commercial ;
* Taux de marge ;
* Classement des commerciaux ;
* Comparaison des régions.

Cette analyse permet d'identifier les commerciaux présentant les **meilleures performances globales**.

---

# 👥 Page 4 — Analyse clients & paiements

Cette page analyse la contribution des différents segments clients ainsi que les comportements liés aux modes de paiement.

### Analyse clients

* Chiffre d'affaires par segment client.

### Analyse des paiements

* Nombre de ventes par mode de paiement ;
* Chiffre d'affaires par mode de paiement ;
* Part du Mobile Money ;
* Comparaison des performances selon le mode de paiement.

---

# 🔎 5. Questions business

L'analyse cherche notamment à répondre aux questions suivantes :

### 🌍 Régions

**Quelle région génère le plus de chiffre d'affaires ?**

### 🏷️ Catégories

**Quelle catégorie est la plus rentable ?**

### 📦 Produits

**Quels sont les 10 produits les plus performants ?**

**Quels produits génèrent beaucoup de chiffre d'affaires mais peu de bénéfice ?**

### 👨‍💼 Commerciaux

**Quel commercial réalise la meilleure performance globale ?**

### 📅 Temps

**Quel mois est le plus performant ?**

### 👥 Clients

**Quel segment client contribue le plus au chiffre d'affaires ?**

### 💳 Paiements

**Quel mode de paiement est le plus utilisé ?**

**Les performances commerciales diffèrent-elles selon le mode de paiement ?**

---

# 💡 6. Principaux insights

L'analyse permet d'identifier :

* Les régions contribuant le plus au chiffre d'affaires ;
* Les catégories les plus rentables ;
* Les produits les plus performants ;
* Les produits présentant un chiffre d'affaires élevé mais une rentabilité faible ;
* Les commerciaux les plus performants ;
* Les périodes présentant les meilleures performances ;
* Les segments clients contribuant le plus au chiffre d'affaires ;
* Les modes de paiement les plus utilisés ;
* Les éventuelles différences de performance selon les modes de paiement.

> Les valeurs et conclusions présentées dans cette section correspondent aux résultats obtenus dans le dashboard final.

---

# 📌 7. Méthodologie

Le projet suit une démarche complète d'analyse de données :

```text
Données brutes
      ↓
Power Query
      ↓
Nettoyage & contrôle qualité
      ↓
Modélisation en étoile
      ↓
Création des mesures DAX
      ↓
Analyse des données
      ↓
Visualisations Power BI
      ↓
Insights business
      ↓
Recommandations
```

---

# 🛠️ Technologies utilisées

| Technologie            | Utilisation                       |
| ---------------------- | --------------------------------- |
| **Microsoft Power BI** | Analyse et visualisation          |
| **Power Query**        | Nettoyage et transformation       |
| **DAX**                | Calcul des indicateurs            |
| **Excel**              | Source des données                |
| **GitHub**             | Gestion et présentation du projet |


# 🎯 Livrables

Le projet comprend :

* 📊 Le fichier Power BI `.pbix` ;
* 📈 Le dashboard final interactif ;
* 🖼️ Les captures des principales pages ;
* 📄 La documentation du projet ;
* 💡 Une synthèse des principaux insights business ;
* 📌 Des recommandations orientées décision.

---

# 🚀 Compétences démontrées

À travers ce projet, je démontre ma capacité à :

* Préparer et nettoyer des données avec Power Query ;
* Construire un modèle de données en étoile ;
* Créer des mesures DAX ;
* Concevoir des KPI ;
* Réaliser des analyses temporelles ;
* Analyser les ventes et la rentabilité ;
* Comparer les performances commerciales ;
* Construire des dashboards interactifs ;
* Identifier des insights business ;
* Transformer des données en informations utiles à la prise de décision.

---

# 👤 Auteur

## MBAYAM GUELBE PHILEMON

**Ingénieur Informaticien · Data Analyst · Développeur Web & Mobile**

### Compétences Data

`Excel` · `Power Query` · `Power BI` · `DAX` · `SQL` · `PostgreSQL`

---

> **Transformer les données en informations utiles pour faciliter la prise de décision.**
