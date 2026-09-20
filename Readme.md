# Atelier Préparation de Données Images

## Description

Ce projet a pour objectif de préparer un jeu de données d'images de déchets afin de le rendre propre et homogène, prêt à être utilisé pour entraîner un modèle de Machine Learning ou de Deep Learning.

Le modèle final devra classer chaque image dans l'une des six catégories suivantes :

- cardboard : cartons ondulés, cartons plats
- glass : bouteilles et objets en verre
- metal : canettes, boîtes métalliques
- paper : feuilles, journaux
- plastic : bouteilles, emballages plastiques
- trash : emballages bonbons, tasses jetables

Les images proviennent de sources multiples et présentent donc de nombreux problèmes : dimensions différentes, formats variés, images RGB et grayscale, images trop petites, images corrompues, images vides, doublons, images mal classées et classes déséquilibrées.

L'atelier consiste à construire un pipeline complet de nettoyage et de préparation de ces images.

## Structure du projet

```
atelier_prepa_donnees_images/
│
├── README.md
│
├── notebooks/
│ └── atelier_prepa_donnees_images.ipynb
│
├── reports/
│ ├── audit_images.csv
│ ├── audit_images_final.csv
│ ├── audit_images_sans_corrompues.csv
│ ├── audit_images_sans_doublons.csv
│ ├── audit_images_sans_petites.csv
│ ├── audit_images_sans_vides.csv
│ ├── parametres_normalisation.json
│ ├── rapport_final.pdf
│ ├── data_augmentation_exemple.png
│ ├── deséquilibre_classes.png
│ ├── distribution_canaux.png
│ ├── distribution_resolutions.png
│ ├── images_corrompues.png
│ ├── images_vides.png
│ ├── normalisation_pixels.png
│ ├── suppression_doublons.png
│ ├── test_conversion_rgb.png
│ ├── test_redimensionnement.png
│ └── uniformisation_rgb.png
│
└── data/
├── raw/
│ ├── cardboard/
│ ├── glass/
│ ├── metal/
│ ├── paper/
│ ├── plastic/
│ └── trash/
│
├── cleaned/
│ ├── cardboard/
│ ├── glass/
│ ├── metal/
│ ├── paper/
│ ├── plastic/
│ └── trash/
│
└── splits/
├── train/
│ ├── cardboard/
│ ├── glass/
│ ├── metal/
│ ├── paper/
│ ├── plastic/
│ └── trash/
├── val/
│ └── (même structure)
└── test/
└── (même structure)
```

## Étapes réalisées

### Partie 1 – Exploration du dataset

- Récupération des informations pour chaque image : nom, classe, format, mode, largeur, hauteur, écart-type des pixels, nombre de canaux et taille.
- Prise en charge des fichiers corrompus.
- Sauvegarde des résultats dans `reports/audit_images.csv`.

### Partie 2 – Détection des images corrompues

- Fonction de détection des images illisibles ou endommagées.
- Suppression des images corrompues du dataset.
- Sauvegarde dans `reports/audit_images_sans_corrompues.csv`.

### Partie 3 – Détection des images vides

- Détection des images entièrement noires, entièrement blanches ou avec très peu de variation de pixels.
- Utilisation de l'écart-type et de la proportion de pixels extrêmes.
- Suppression des images vides.
- Sauvegarde dans `reports/audit_images_sans_vides.csv`.

### Partie 4 – Détection des différences de résolution

- Analyse des résolutions minimale, maximale et les plus fréquentes.
- Détection des images ne respectant pas la contrainte minimale de 64×64 pixels.
- Suppression des images trop petites.
- Sauvegarde dans `reports/audit_images_sans_petites.csv`.

### Partie 5 – Détection des différents canaux

- Analyse des modes d'images (RGB, RGBA, L, P, etc.).
- Identification des images non-RGB qui devront être converties.

### Partie 6 – Détection des doublons

- Calcul d'une empreinte MD5 pour chaque image.
- Détection des images identiques même avec des noms de fichiers différents.
- Détection des doublons entre classes.
- Suppression des doublons.
- Sauvegarde dans `reports/audit_images_sans_doublons.csv`.

### Partie 7 – Détection des images mal classées

- Contrôle visuel par classe à l'aide d'échantillons.
- Identification manuelle des images suspectes.
- Marquage dans une colonne dédiée.

### Partie 8 – Analyse du déséquilibre des classes

- Comptage des images par classe.
- Calcul du ratio entre classe majoritaire et classe minoritaire.
- Identification des classes à augmenter.

### Partie 9 – Redimensionnement

- Redimensionnement de toutes les images à 224×224 pixels.
- Conservation des proportions avec ajout de padding.
- Sauvegarde dans `data/cleaned/`.

### Partie 10 – Uniformisation des canaux

- Conversion de toutes les images en RGB (3 canaux).
- Gestion des cas particuliers (RGBA, L, P).
- Sauvegarde dans `data/cleaned/`.

### Partie 11 – Mise à l'échelle des pixels

- Normalisation des valeurs des pixels entre 0 et 1.
- Formule : pixel / 255.0
- Sauvegarde des paramètres dans `reports/parametres_normalisation.json`.

### Partie 12 – Découpage Train/Validation/Test

- Découpage du dataset nettoyé selon les ratios 70% / 15% / 15%.
- Reproductibilité garantie avec une graine aléatoire (seed=42).
- Sauvegarde dans `data/splits/`.

### Partie 13 – Data Augmentation

- Application de transformations (rotation, retournement, zoom, luminosité, etc.) sur la classe minoritaire.
- Utilisation de `ImageDataGenerator` de Keras.
- Objectif : équilibrer les classes.
- Note : la data augmentation est appliquée après le découpage pour éviter les fuites de données.

### Partie 14 – Bonus

- Génération d'un rapport PDF automatique avec `reportlab`.
- Mise en place d'un split stratifié pour garantir la représentativité des classes dans chaque sous-ensemble.
- Sauvegarde du rapport dans `reports/rapport_final.pdf`.

## Technologies utilisées

- Python 3.8 ou supérieur
- Bibliothèques principales :
  - numpy
  - pandas
  - Pillow (PIL)
  - matplotlib
  - scikit-learn
  - tensorflow / keras
  - tqdm
  - reportlab

## Installation

1. Cloner le dépôt ou extraire le dossier du projet.

2. Créer un environnement virtuel :

```bash
python -m venv venv
```
