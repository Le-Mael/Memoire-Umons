# graph_OSMnx_top.ipynb — README

## Description

Ce notebook télécharge des graphes routiers réels depuis OpenStreetMap via la bibliothèque **OSMnx**, puis les soumet à une suite d'algorithmes de **flot maximum** (statique et dynamique) : Edmonds-Karp, Dinic et Push-Relabel.

Le pipeline complet enchaîne :

1. Téléchargement et visualisation des graphes routiers
2. Nettoyage (suppression itérative des nœuds de degré 1)
3. Enrichissement optionnel par arêtes aléatoires
4. Identification de la plus grande composante connexe
5. Sélection automatique de la source et du puits
6. Calcul des capacités par arête (vitesse × nombre de voies)
7. Benchmark des algorithmes statiques + histogramme comparatif
8. Identification d'une arête du min-cut
9. Benchmark des algorithmes dynamiques après modification de capacité + histogramme comparatif

---

## ⚠️ Réglages à effectuer avant l'exécution

Tous les paramètres configurables se trouvent dans la cellule **`CONFIGURATION DU PIPELINE`**, juste après les imports.

### 1. `USE_RANDOM_EDGES` — Ajout d'arêtes aléatoires

```python
USE_RANDOM_EDGES = True   # True  → ajoute des arêtes aléatoires (densifie le graphe)
                          # False → conserve uniquement le graphe nettoyé
```

### 2. `SOURCE_SINK_METHOD` — Méthode de sélection de la source et du puits

```python
SOURCE_SINK_METHOD = "betweenness"  # Choisir parmi les trois options ci-dessous :
```

| Valeur | Comportement |
|---|---|
| `"north_south"` | Nœuds les plus au nord et au sud ; ajoute des arêtes de connexion |
| `"degree"` | Paire de nœuds à degré maximal + distance topologique maximale |
| `"betweenness"` | Paire de nœuds à betweenness centrality élevée + distance maximale |

> Une fois ces deux paramètres définis, **vous pouvez exécuter tout le notebook de haut en bas** sans autre intervention.

---

## Changer les villes

Les villes sont définies dans la section **`Build and plot the graph`**, dans la variable `places`.

Par défaut, **trois villes sont intentionnellement laissées décommentées** pour permettre une exécution rapide sans modification du code :

```python
places = [
    ...
    "Louviere, Belgique",   # ← actif  (exemple)
    ...
    "Oostende, Belgique",   # ← actif  (exemple)
    "seraing, Belgium"      # ← actif  (exemple)
]
```

Ces trois villes — **La Louvière, Ostende et Seraing** — servent d'exemples de référence. Elles couvrent des tailles de graphe variées (≈ 1 800 à 2 200 nœuds) et permettent de valider l'ensemble du pipeline sans délai excessif.

Pour ajouter ou remplacer des villes, il suffit de décommenter (ou commenter) les lignes souhaitées dans cette liste. D'autres exemples sont déjà présents en commentaire (Bruges, Tokyo, Barcelone, Oxford, etc.).

---

## Dépendances

Installées automatiquement par la première cellule :

```bash
pip install osmnx
```

Les autres bibliothèques requises (`networkx`, `matplotlib`, `numpy`, `pandas`) font partie de l'environnement standard de Jupyter/Colab.

