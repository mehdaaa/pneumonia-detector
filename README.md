# Détection de la Pneumonie avec un Réseau de Neurones Convolutifs (CNN)

## Introduction

Ce projet vise à automatiser la **détection de la pneumonie** à partir de **radiographies thoraciques** en utilisant un **modèle de Deep Learning** basé sur des **Convolutional Neural Networks (CNN)**.

L'objectif est de classifier efficacement les radiographies en deux catégories :
- **NORMAL** : Poumons sains
- **PNEUMONIA** : Présence d'infection pulmonaire

---

## 📷 Exemple de Radiographies Thoraciques

### Exemple d'une radiographie NORMALE
![Radio Normale](assets\radio_normale.jpeg)

### Exemple d'une radiographie avec PNEUMONIE
![Radio Pneumonie](assets\radio_pneumonie.jpeg)

---

## 📌 Jeu de Données et Prétraitement

### **Présentation du Jeu de Données**
Le dataset est composé d'images de radiographies thoraciques classées en deux catégories :
- **NORMAL** (poumons sains)
- **PNEUMONIA** (poumons infectés)

Le dataset est organisé en trois dossiers :
- **Train** : utilisé pour l'entraînement du modèle
- **Test** : utilisé pour l'évaluation finale
- **Validation** : disponible mais **non utilisé** ; nous avons préféré créer une validation en séparant **20%** du dossier **Train**.

### **Détail des Images**
| Dossier | NORMAL | PNEUMONIA | Total |
|:-------:|:------:|:---------:|:-----:|
| **Train** | 1 352 | 3 876 | 5 228 |
| **Validation** | 9 | 9 | 18 (non utilisé) |
| **Test** | 234 | 390 | 624 |

✅ **Remarque** :  
- **20% des images du dossier Train** sont réservées à la **validation** pendant l'entraînement du modèle.  
- **Le dossier Validation officiel** (18 images) est **ignoré** car jugé insuffisant pour une validation représentative.

### 🔧 Prétraitement et Augmentation
- **Redimensionnement** : toutes les images sont redimensionnées à une taille fixe (`256x256`).
- **Augmentation des données** : application de transformations pour enrichir l'ensemble d'entraînement :
  - Flips horizontaux
  - Rotations aléatoires
  - Zooms sur les zones d'intérêt
  - Ajustements de contraste et de luminosité
- **Normalisation** : les valeurs des pixels sont mises à l'échelle dans l'intervalle `[0, 1]`.
- **Gestion du déséquilibre des classes** : utilisation de pondérations (`class_weight`) pour équilibrer l'impact des classes majoritaires et minoritaires durant l'entraînement.

---

## 🛠️ Architecture du Modèle CNN

| Couche | Type | Paramètres principaux | Activation |
|:-------|:-----|:----------------------|:-----------|
| Entrée | Input (256x256x3) | - | - |
| Augmentation | RandomFlip, RandomRotation, RandomZoom, RandomContrast | - | - |
| Convolution 1 | 32 filtres, 3x3 | ReLU |
| MaxPooling | 2x2 | - |
| Convolution 2 | 64 filtres, 3x3 | ReLU |
| MaxPooling | 2x2 | - |
| Convolution 3 | 128 filtres, 3x3 | ReLU |
| MaxPooling | 2x2 | - |
| Convolution 4 | 256 filtres, 3x3 | ReLU |
| MaxPooling | 2x2 | - |
| Flatten | - | - |
| Dense 1 | 256 neurones | ReLU |
| Dropout | 0.5 | - |
| Sortie | 1 neurone | Sigmoïde |

### ⚙️ Paramètres d'entraînement
- **Loss** : Binary Crossentropy
- **Optimiseur** : Adam
- **Métriques** : Accuracy, AUC
- **Callbacks** : EarlyStopping, ModelCheckpoint

---

## 📈 Résultats

Après l'entraînement, le modèle a obtenu les performances suivantes :

| Ensemble | Accuracy | AUC     |
|:---------|:---------|:--------|
| Validation | ~88% | ~98.6% |
| Test | ~88% | ~95.3% |

---

## 📊 Matrices de Confusion

![Matrice de Confusion](assets\courbe_roc.png)

---

## 📉 Courbes ROC

![Courbe ROC](assets\courbe_roc.png)

---

## 📝 Conclusion

✔️ Le modèle CNN est capable de détecter efficacement la pneumonie sur des images médicales.  
✔️ Les techniques d'augmentation des données et la gestion du déséquilibre ont fortement amélioré les résultats.  
✔️ Le modèle montre une bonne généralisation et une excellente capacité de séparation des classes.

### 📈 Perspectives d'amélioration
- Tester avec **cross-validation** pour évaluer la performance du modèle de manière plus robuste.
- Séparer les images de pneumonie en sous-catégories pour identifier différents types de pneumonie (ex: **virale**, **bactérienne**).

---
