# 📚 Scientific Literature Search Engine

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?style=for-the-badge&logo=python)
![Scikit-Learn](https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![NetworkX](https://img.shields.io/badge/NetworkX-Graph-blueviolet?style=for-the-badge)

Ce projet implémente un **moteur de recherche sémantique** avancé pour la recommandation d'articles scientifiques. Il traite un problème de *Citation Matching* : identifier les articles cités par une publication donnée (requête) parmi un large corpus, en filtrant le bruit.

Le projet compare et combine des approches **Lexicales** (TF-IDF), **Topologiques** (PageRank), **Neuronales** (Transformers) et **Hybrides**.

---

## 🎯 Objectifs

1.  **Recherche d'Information (IR) :** Développer un système de ranking pertinent pour des documents scientifiques.
2.  **Benchmark :** Comparer l'efficacité de méthodes très différentes (Mots-clés vs Popularité vs Sens).
3.  **Hybridation Sémantique/Structurelle :** Exploiter le graphe de citations pour enrichir la compréhension textuelle.

---

## 🗂️ Dataset

Le projet s'appuie sur des données bibliographiques réelles :
* **Corpus :** +25 000 articles scientifiques (Titres, Résumés, Métadonnées).
* **Requêtes :** 1 000 articles sources.
* **Graphe :** 54 000 arcs de citations entre les papiers.
* **Vérité Terrain :** Annotations binaires (Pertinent/Non Pertinent) pour l'évaluation.

---

## ⚙️ Méthodologie

### 1. Approche Lexicale (Baseline)
Méthodes statistiques classiques pour établir une ligne de base.
* **Techniques :** Bag-of-Words, TF-IDF.
* **Limites :** Ne capture pas la synonymie ni le contexte sémantique.

### 2. Approche Topologique (PageRank)
Analyse purement structurelle basée sur le graphe de citations.
* **Hypothèse :** La pertinence est corrélée à l'influence globale (centralité) de l'article.
* **Algorithme :** PageRank (`alpha=0.85`).

### 3. Approche Dense (Deep Learning)
Projection des textes dans un espace vectoriel sémantique via des Transformers.
* **SBERT (`all-MiniLM-L6-v2`) :** Modèle générique rapide.
* **SPECTER (`allenai-specter`) :** Modèle SOTA pré-entraîné spécifiquement sur des citations scientifiques.
* **Input :** `[Title] + [SEP] + [Abstract]`.

### 4. Approche Hybride : Graph Enhanced Embeddings
Combinaison de l'information sémantique (Contenu) et sociale (Voisinage). Les vecteurs des documents sont lissés par ceux de leurs voisins dans le graphe.
* **Formule :**
    $$V_{new} = \alpha \cdot V_{original} + (1 - \alpha) \cdot \text{Mean}(V_{neighbors})$$

---

## 📈 Résultats et Performances

Les modèles sont évalués sur le set de validation (700 requêtes).

| Modèle | Type | Précision (@5) | Rappel (@5) | F1-Score | AUC |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **SBERT (MiniLM)** | *Sémantique* | 0.99 | 0.30 | 0.46 | 0.95 |
| **PageRank** | *Structurel* | 0.62 | 0.63 | 0.62 | 0.87 |
| **SPECTER** | *Sémantique (SOTA)* | 0.80 | 0.81 | 0.80 | 0.96 |
| **SPECTER + Graph** 🏆 | *Hybride* | **0.83** | **0.84** | **0.83** | **0.97** |

### Analyse
* **SBERT** est très conservateur : il a une précision excellente mais un rappel faible (il rate beaucoup de documents pertinents).
* **PageRank** offre une performance équilibrée (F1 ~0.62) sans même lire le texte, prouvant que la citation est fortement liée à la popularité.
* **SPECTER + Graph** surpasse toutes les méthodes en unifiant la finesse sémantique et la structure de citation.

---

## 🛠️ Stack Technique

* **Langage :** Python 3.8+
* **NLP :** `sentence-transformers`, `scikit-learn`, `nltk`
* **Graph Mining :** `networkx`
* **Data :** `pandas`, `numpy`

---

## 🚀 Installation et Exécution

1.  **Cloner le dépôt :**
    ```bash
    git clone [https://github.com/VOTRE_USERNAME/NOM_DU_REPO.git](https://github.com/VOTRE_USERNAME/NOM_DU_REPO.git)
    cd NOM_DU_REPO
    ```

2.  **Installer les dépendances :**
    ```bash
    pip install pandas numpy scikit-learn sentence-transformers networkx torch nltk tqdm
    ```

3.  **Lancer le notebook :**
    Exécuter le fichier Jupyter Notebook fourni.
    *Note : L'étape d'encodage SPECTER inclut un système de cache (`.pkl`) pour éviter de recalculer les embeddings à chaque lancement.*

---

## 👤 Auteurs

Projet réalisé dans le cadre du module **Introduction à la Science des Données (MOD 7.2)**.
* **Enseignants :** Julien Velcin, Erwan Versmée.
* **Étudiant :** [Votre Nom]
