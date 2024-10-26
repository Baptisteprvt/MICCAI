# 🧠 Segmentation d'Images Médicales avec un Réseau de Neurones U-Net

Ce projet a pour but de segmenter des images médicales de patients, en utilisant des modèles de deep learning, en particulier un réseau de neurones U-Net.

## 📝 Description du projet

Le projet se concentre sur la segmentation d'images T1 et T2, en extrayant des informations pertinentes pour chaque patient et en générant des masques de segmentation. Cette tâche est réalisée en plusieurs étapes :
1. Chargement des images de chaque patient à partir de données au format NIfTI.
2. Prétraitement des images, y compris la normalisation des pixels pour améliorer les performances du modèle.
3. Entraînement d'un réseau U-Net personnalisé pour segmenter les images et extraire des coupes transversales.

## 🚀 Démarrage rapide

1. **Prérequis** : Installez les dépendances en utilisant pip :
    ```bash
    pip install nibabel numpy matplotlib keras
    ```
2. **Données** : Les données d'images médicales sont dans le dossier `images/`. Chaque patient a un dossier avec des images T1, T2 et des masques de segmentation.

3. **Exécution** : Lancez le script pour charger et afficher les images ou entraîner le modèle de segmentation.

## 📂 Structure du code

### 1. Chargement et Affichage des Images

- `load_img_subject(subject_id)`: Fonction qui charge les images T1, T2 et les labels d'un sujet spécifique depuis le répertoire `images/`.
- `display_images(images, dimension)`: Fonction qui affiche les coupes médianes de chaque dimension (1, 2 ou 3) pour visualiser les données des patients.

### 2. Prétraitement et Normalisation des Images

- `normalize_image(img)`: Cette fonction normalise les images en supprimant les valeurs négatives et en recentrant les pixels autour de zéro.
- `load_norm_img_subject(subject_id)`: Charge et normalise les images d'un patient donné, en séparant les types T1 et T2 et les labels.

### 3. Extraction et Préparation des Données

- `extract_z(pt_train)`: Extrait les coupes pertinentes des images pour un ensemble de patients d'entraînement, en comptant et en filtrant les coupes avec un nombre suffisant de pixels non nuls. La fonction retourne les tableaux `X_train` et `Y_train`.

### 4. Réseau U-Net pour la Segmentation

Le réseau U-Net est défini avec une architecture de convolution en cascade permettant la segmentation de précision :

- `unet(input_size)`: Modèle U-Net en Keras. Il utilise des couches de convolutions 2D et de MaxPooling pour réduire la taille de l'image, et des couches de convolutions transposées pour restaurer la résolution initiale de l'image.

### 5. Entraînement du Modèle

Le modèle est entraîné sur des images de taille `(144, 192, 2)` :
```python
model.fit(X_Train, Y_Train, batch_size=10, epochs=100, validation_data=(X_val, Y_val))
```

## ⚙️ Fonctionnalités Clés

- **Prétraitement des données** : Les images sont normalisées pour supprimer les valeurs aberrantes et améliorer la convergence du modèle, tandis que des coupes pertinentes sont extraites pour réduire le volume de données inutiles.
- **Architecture U-Net** : Le modèle de segmentation U-Net permet une segmentation précise des images médicales, en utilisant une architecture de convolution en cascade.
- **Affichage des images** : La visualisation des coupes T1, T2 et des labels est facilitée par une fonction dédiée, qui permet de visualiser les données de chaque patient.

## 📈 Résultats et Évaluation

Le modèle est évalué en suivant des métriques telles que la précision et la perte. Ces indicateurs sont suivis sur les ensembles de validation et de test pour mesurer la capacité de généralisation du modèle et optimiser les performances.

## 📊 Structure des Données

Le jeu de données est organisé en trois ensembles :
- **patients_train** : Ensemble d'entraînement comprenant 6 patients.
- **patients_val** : Ensemble de validation comprenant 2 patients.
- **patients_tests** : Ensemble de test comprenant 2 patients.

Cette répartition suit un ratio 60/20/20, permettant d'entraîner efficacement le modèle tout en réservant suffisamment de données pour la validation et le test.

## 🛠️ Outils et Technologies Utilisés

- **Nibabel** : Bibliothèque Python utilisée pour manipuler les données d'imagerie médicale au format NIfTI.
- **NumPy** : Utilisée pour les calculs matriciels et la manipulation de tableaux multidimensionnels.
- **Matplotlib** : Utilisé pour la visualisation des coupes d'images.
- **Keras** : Framework de deep learning permettant de définir et d'entraîner le modèle U-Net de segmentation.
