# atelier_prepa_donnees_images

Atelier de préparation d'un jeu de données images (tri de déchets : `cardboard`, `glass`, `metal`, `paper`, `plastic`, `trash`) en vue de l'entraînement d'un modèle de Machine Learning / Deep Learning.

## Structure du projet

```
atelier_prepa_donnees_images/
├── notebooks/
│   └── atelier_prepa_donnees_images.ipynb   # notebook complet (Parties 1 à 13)
├── reports/
│   └── audit_images.csv                     # résultats de l'audit du dataset brut
└── data/
    ├── raw/          # images fournies, en lecture seule (6 classes)
    ├── cleaned/       # images nettoyées : corrompues/vides/doublons exclus,
    │                  # classes corrigées, redimensionnées 224x224, RGB
    └── splits/        # data/cleaned decoupe en train/val/test (stratifie),
                        # avec data augmentation sur train/trash (classe minoritaire)
```

## Déroulé de l'atelier

1. **Exploration** (Partie 1) : extraction des métadonnées de chaque image (nom, classe, format, mode, dimensions, écart-type des pixels, canaux, taille), avec prise en charge des fichiers corrompus.
2. **Audit qualité** (Parties 2 à 8) : détection des images corrompues, des images vides, des différences de résolution et de canaux, des doublons (y compris inter-classes) et des images mal classées ; analyse du déséquilibre des classes. Toutes les statistiques sont consolidées dans `reports/audit_images.csv`.
3. **Nettoyage** : construction de `data/cleaned/` à partir de `data/raw/` en excluant les images corrompues/vides/doublons et en corrigeant les classes erronées, sans jamais modifier `data/raw/`.
4. **Uniformisation** (Parties 9 à 11) : redimensionnement 224×224 avec conservation des proportions (padding), conversion en RGB, normalisation des pixels dans [0, 1].
5. **Découpage et augmentation** (Parties 12 et 13) : split stratifié train/validation/test (70/15/15), puis data augmentation Keras (rotation, flip, zoom, translation, luminosité, contraste, teinte) appliquée uniquement au sous-ensemble d'entraînement de la classe minoritaire `trash`, avec justification du choix d'augmenter après le split plutôt qu'avant.

## Environnement

Environnement Python 3.13 géré avec [uv](https://github.com/astral-sh/uv) (dossier `.venv/`, non versionné) :

```bash
uv venv --python 3.13 .venv
uv pip install --python .venv/Scripts/python.exe numpy pandas matplotlib seaborn pillow scikit-learn tensorflow keras jupyter nbconvert ipykernel nbformat
```

Exécution du notebook de bout en bout :

```bash
.venv/Scripts/python.exe -m jupyter nbconvert --to notebook --execute --inplace notebooks/atelier_prepa_donnees_images.ipynb
```
