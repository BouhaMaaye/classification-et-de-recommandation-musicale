# 🎧 Projet de classification et de recommandation musicale avec le dataset GTZAN

## 🎯 Objectif
Ce projet vise à :
- Classifier automatiquement des morceaux de musique par genre
- Créer un système de recommandation basé sur les caractéristiques audio
- Explorer les similarités musicales à l’aide de techniques de Machine Learning

## 🧰 Technologies utilisées
- Python 3
- Librairies principales : `librosa`, `pandas`, `numpy`, `scikit-learn`, `matplotlib`, `seaborn`

---

## 📁 Contenu du projet

### 1. 📥 Chargement et Préparation des Données
- Utilisation du dataset **GTZAN** (10 genres, 1000 extraits de 30 secondes)
- Création d’un DataFrame avec les colonnes `filename`, `label`, et les features audio

### 2. 🎵 Extraction de caractéristiques audio
Pour chaque fichier `.wav`, nous avons extrait :
- **MFCCs (13 coefficients)** : représentation de l'enveloppe spectrale
- **Chroma STFT** : présence des 12 demi-tons
- **Spectral centroid, rolloff, bandwidth**
- **Zero Crossing Rate**
- **Tempo (BPM)**
- **Fréquence fondamentale**
- **Contraste spectral**

> 🧪 Résultat : un DataFrame de taille `(1000, 20+)` avec toutes les features pour chaque piste.

---

### 3. 📊 Analyse des Similarités
- **Similarité cosinus** et **distance euclidienne** calculées entre les pistes
- Construction de la **matrice des similarités** (1000 x 1000)

### 4. 🤖 Importance des features
- Importance des features évaluée à l’aide d’un RandomForestClassifier
- Top 5 des features les plus importantes :
  1. `mfcc_1`
  2. `spectral_centroid`
  3. `rolloff`
  4. `contrast`
  5. `tempo`

> Ces features ont été **pondérées** pour améliorer le système de recommandation.

---

### 5. 🎯 Clustering (KMeans + PCA)
- Application du **KMeans** avec `k=3` clusters (à partir du score de silhouette)
- Réduction de dimension avec **PCA**
- Visualisation en 2D des clusters

> 📈 Score de silhouette obtenu : **~0.36**  
> 🔍 Les clusters regroupent partiellement les genres similaires (e.g., jazz et blues proches)

---

### 6. 💡 Recommandation musicale
Recommandations basées sur la similarité cosinus des features pondérées :
- Pour chaque piste, on recommande les **5 plus proches** pistes
- Analyse de la **diversité** des recommandations avec la distance cosinus entre les morceaux recommandés

---

### 7. ✅ Évaluation du système de recommandation

| Métrique       | Score     |
|----------------|-----------|
| Précision      | **0.768** |
| Rappel         | **0.748** |
| F1-score       | **0.752** |
| NDCG@5         | **0.85**  |

> Les scores montrent une **bonne cohérence de recommandation** : les morceaux recommandés sont souvent du même genre, ou d’un genre musicalement proche.

> La diversité moyenne mesurée (distance cosinus moyenne entre les morceaux recommandés) est **0.53**, montrant un bon compromis entre **pertinence** et **diversité**.

---

## 📌 Justification des choix

- **Librosa** : bibliothèque spécialisée pour l'audio
- **KMeans** : simple et efficace pour explorer des regroupements naturels
- **PCA** : permet une visualisation claire de la structure des données
- **Random Forest** : fiable pour estimer l’importance des features
- **Cosine Similarity** : bien adaptée à des vecteurs normalisés



## 📌 Conclusion
Le projet montre qu’il est possible de **comprendre, grouper et recommander** automatiquement des musiques uniquement via leurs caractéristiques audio. 
