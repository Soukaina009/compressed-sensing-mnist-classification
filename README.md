# 🔬 Compressed Sensing vs Baseline LDA for Image Classification (MNIST)

![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)
![Topic](https://img.shields.io/badge/Focus-Compressed%20Sensing%20%7C%20Machine%20Learning-green)
![Dataset](https://img.shields.io/badge/Dataset-MNIST-orange)

## 📌 Présentation du Projet
Ce projet présente une étude comparative entre une approche classique de classification sur données brutes (**Baseline LDA**) et un framework basé sur le **Compressed Sensing (CS)** combiné à l'Analyse Discriminante Linéaire.

L'objectif principal est d'évaluer la capacité du Compressed Sensing à **réduire drastiquement la dimension des données d'entrée (de 784 à 250 variables)** lors de la phase de mesure/acquisition, tout en préservant des performances de classification compétitives.

---

## 🎯 Enjeux & Contexte
Dans les applications réelles (capteurs optiques embarqués, IoT, systèmes à ressources limitées), l'acquisition de signaux haute dimension est coûteuse en énergie et en stockage. 

En exploitant la **parcimonie (sparsity)** des images dans le domaine des ondelettes (Haar / Daubechies), la théorie du *Compressed Sensing* permet de capturer directement l'information utile via un nombre réduit de projections aléatoires $p \ll N$.

---

## 🔬 Méthodologie & Architecture

Le workflow compare trois stratégies principales :

1. **Baseline LDA :** Entraînement directement sur les $N = 784$ pixels bruts de l'image.
2. **Compressed Sensing — Approche A (Flash / Classification Directe) :**
   * Projection aléatoire Gaussienne $\Phi$ : $y = \Phi x$ (passage de 784 à 250 dimensions).
   * Classification LDA directement appliquée sur les mesures compressées $y$ sans étape de reconstruction.
3. **Compressed Sensing — Approche B (Reconstruction OMP) :**
   * Mesure compressée via $\Phi$.
   * Reconstruction du support parcimonieux $K$ via l'algorithme *Orthogonal Matching Pursuit* (OMP).
   * Classification LDA sur les coefficients reconstruits.

---

## 📊 Synthèse des Résultats

| Stratégie / Métrique | Baseline (Pixels Bruts) | CS Approche A (Directe) | CS Approche B (OMP + LDA) |
| :--- | :---: | :---: | :---: |
| **Dimension d'entrée ($p$)** | 784 | **250** (-68% de volume) | 250 → $K=80$ Coeffs |
| **Taux de Compression** | 0 % | **68.2 %** | 68.2 % |
| **Accuracy Test** | **83.80 %** | **81.50 % - 82.50 %** | ~80.00 % |
| **Temps de calcul** | Standard | **Ultra-rapide (Inférence directe)** | Élevé (Reconstruction OMP) |

### 💡 Conclusions Clés :
* **Efficacité de l'Approche Directe :** L'Approche A permet d'économiser près de **70% du volume de données** tout en conservant une précision quasi équivalente ($\approx -1.5\%$) par rapport à la baseline.
* **Gain Computationnel :** L'inférence sur le vecteur réduit $y$ évite le coût algorithmique élevé de la reconstruction matricielle OMP.

---

## 📄 Papier Scientifique & Rapport
Une feuille scientifique détaillée retraçant le cadre théorique, les équations de RIP (Restricted Isometry Property), les analyses de parcimonie des ondelettes mères ainsi que les protocoles expérimentaux est disponible dans ce dépôt :
👉 

---

## 🛠️ Installation & Exécution

### Cloner le projet & Installer les dépendances
```bash
git clone [https://github.com/Soukaina009/compressed-sensing-mnist-classification.git](https://github.com/Soukaina009/compressed-sensing-mnist-classification.git)
cd compressed-sensing-mnist-classification
pip install -r requirements.txt
