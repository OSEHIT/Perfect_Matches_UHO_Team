## Perfect_Matches_UHO_Team
# 📚 Scientific Literature Search Engine

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?style=for-the-badge&logo=python)
![Scikit-Learn](https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![HuggingFace](https://img.shields.io/badge/Hugging%20Face-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black)
![NetworkX](https://img.shields.io/badge/NetworkX-Graph-blueviolet?style=for-the-badge)

Ce projet implémente un **moteur de recherche sémantique** avancé destiné à la recommandation d'articles scientifiques. Il résout un problème de *Citation Matching* : étant donné un article (requête), retrouver les articles qu'il cite parmi une liste de candidats, en distinguant les liens pertinents du bruit.

Le projet compare des approches classiques (TF-IDF), neuronales (Transformers), Topologiques (PageRank) et hybrides (Graph Enhanced Embeddings).

---

## 🎯 Objectifs

* **Recherche d'Information (IR) :** Construire un système capable de classer des documents scientifiques par pertinence sémantique.
* **Comparaison d'approches :** Évaluer le gap de performance entre les méthodes fréquentielles (Sparse), les représentations denses (Embeddings) et structurelles.
* **Hybridation :** Exploiter la structure du graphe de citations pour enrichir les vecteurs sémantiques.

---

## 🗂️ Dataset

Le corpus est composé de données bibliographiques réelles :
* **Corpus :** +25 000 articles scientifiques (Titres, Résumés, Métadonnées).
* **Requêtes :** 1 000 articles sources.
* **Vérité Terrain :** Paires (Requête, Document) annotées pour l'évaluation.

---

## ⚙️ Méthodologie

Le pipeline du projet suit une complexité croissante :

### 1. Approche "Sparse" (Baseline)
Utilisation de méthodes statistiques classiques pour établir une ligne de base.
* **Techniques :** Bag-of-Words, TF-IDF.
* **Preprocessing :** Tokenisation, suppression des stopwords, Stemming (Snowball).
* **Résultat :** Capture efficace des mots-clés exacts mais échec sur la synonymie et le contexte.

### 2. Approche "Dense" (SBERT & SPECTER)
Utilisation de Transformers pour projeter les textes dans un espace vectoriel sémantique.
* **Modèles testés :**
    * `all-MiniLM-L6-v2` : Modèle généraliste rapide.
    * `allenai-specter` : Modèle SOTA pré-entraîné spécifiquement sur des citations scientifiques.
* **Stratégie :** Concaténation `[Title] + [SEP] + [Abstract]` pour maximiser l'information contextuelle.

### 3. Approche Structurelle (PageRank)
Analyse purement structurelle basée sur le graphe de citations.
* **Hypothèse :** La pertinence est corrélée à l'influence globale (centralité) de l'article.
* **Algorithme :** PageRank (`alpha=0.85`).

### 4. Approche Hybride : Graph Enhanced Embeddings
L'hypothèse est que des papiers cités ensemble ou connectés partagent une sémantique forte.
* **Graphe de citations :** Construction via NetworkX (25k nœuds, 54k arcs).
* **Lissage vectoriel :** Mise à jour de l'embedding d'un document par une combinaison linéaire de son vecteur propre et de la moyenne des vecteurs de ses voisins (cités/citants).
    * $$V_{new} = \alpha \cdot V_{original} + (1 - \alpha) \cdot \text{Mean}(V_{neighbors})$$

---

## 📈 Résultats et Performances

Les modèles sont évalués sur le set de validation via la **Précision**, le **Rappel**, le **F1-Score** et l'**AUC**.

| Modèle | Précision (@5) | Rappel (@5) | F1-Score | AUC |
| :--- | :---: | :---: | :---: | :---: |
| **TF-IDF (Baseline)** | 0.4980  | 0.5052 | 0.5016 | 0.7214 |
| **PageRank** | 0.6162 | 0.6252 | 0.6207 | 0.8696 |
| **SBERT (MiniLM)** | 0.9877 | 0.3026 | 0.4633 | 0.9550 |
| **SPECTER (AllenAI)** | 0.8017 | 0.8139 | 0.8067 | 0.9632 |
| **SPECTER + Graph Smoothing** 🏆 | **0.8314** | **0.8439** | **0.8366** | **0.9712** |

> **Conclusion :** L'ajout de l'information topologique (Graphe) au modèle spécialisé (SPECTER) permet d'atteindre les meilleures performances, illustrant la complémentarité entre le contenu textuel et la structure du réseau scientifique.

---

## 🛠️ Stack Technique

* **Langage :** Python 3.8+
* **NLP & Embeddings :** `sentence-transformers`, `scikit-learn`, `nltk`
* **Graphes :** `networkx`
* **Manipulation de données :** `pandas`, `numpy`
* **Visualisation :** `matplotlib`

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
    Ouvrez `BE_CS2_Perfect_Matches.ipynb` dans Jupyter ou Google Colab.

    *Note : L'étape d'encodage avec SPECTER peut prendre quelques minutes sans GPU. Le code inclut un mécanisme de cache (`.pkl`) pour ne pas recalculer les embeddings à chaque exécution.*

---

## 👤 Auteurs et Contexte

Projet réalisé dans le cadre du module **Introduction à la Science des Données (MOD 7.2)**.
