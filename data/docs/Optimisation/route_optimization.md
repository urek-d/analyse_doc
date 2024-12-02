# Optimisation des Routes de Livraison

## Introduction

L'optimisation des routes de livraison, également connue sous le nom de problème du voyageur de commerce (TSP), vise à trouver le chemin le plus court pour qu'un véhicule visite un ensemble de points et retourne à son point de départ.

## Code Explication

### Importation des Bibliothèques

Nous utilisons `networkx` pour créer et manipuler des graphes, et `matplotlib.pyplot` pour la visualisation.

```python
import networkx as nx
import matplotlib.pyplot as 

```
### Données de Distances
Voici un exemple de données de distances entre différentes villes.
```python
distances = {
    'A': {'B': 10, 'C': 15, 'D': 20},
    'B': {'A': 10, 'C': 35, 'D': 25},
    'C': {'A': 15, 'B': 35, 'D': 30},
    'D': {'A': 20, 'B': 25, 'C': 30},
}
```
### Création du Graphe
Nous créons un graphe en ajoutant des arêtes entre les villes avec des poids représentant les distances
```python

G = nx.Graph()
for city, neighbors in distances.items():
    for neighbor, distance in neighbors.items():
        G.add_edge(city, neighbor, weight=distance)
```
### Résolution du TSP
Nous utilisons l'algorithme d'approximation de networkx pour trouver le chemin optimal.
```python
tsp_path = nx.approximation.traveling_salesman_problem(G, cycle=True)
print(f"Optimal path: {tsp_path}")
```
### Visualisation
Nous affichons le graphe avec les distances étiquetées.
```python
pos = nx.spring_layout(G)
nx.draw(G, pos, with_labels=True, node_size=700, node_color='lightblue')
labels = nx.get_edge_attributes(G, 'weight')
nx.draw_networkx_edge_labels(G, pos, edge_labels=labels)
plt.show()
```
### Conclusion
Ce modèle montre comment résoudre un problème de route de livraison en utilisant des graphes