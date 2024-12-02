# Optimisation de la Planification de la Production

## Introduction

L'objectif est de trouver les niveaux de production optimaux pour minimiser les coûts tout en satisfaisant la demande. Cela est essentiel pour la gestion de la production industrielle.

## Code Explication

### Importation des Bibliothèques

Nous utilisons `pandas` pour la manipulation des données, `numpy` pour les opérations mathématiques, et `scipy.optimize` pour l'optimisation linéaire.

```python
import pandas as pd
import numpy as np
from scipy.optimize import linprog
```
### Données de Coûts de Production
Nous générons des données fictives de coûts de production par machine
```python
production_costs = np.array([10, 20, 30])
capacity_constraints = np.array([100, 100, 100])
total_demand = 250
```
### Modèle d'Optimisation Linéaire
Nous utilisons linprog pour résoudre le problème d'optimisation linéaire et obtenir les niveaux de production optimaux.
```python
res = linprog(production_costs, A_ub=[-capacity_constraints], b_ub=[-total_demand], bounds=[(0, None)] * len(production_costs))
print(f"Optimal production levels: {res.x}")
```
### Conclusion
Ce modèle montre comment optimiser les niveaux de production pour minimiser les coûts de production tout en satisfaisant la demande. Vous pouvez adapter ce modèle à vos propres données de production pour améliorer l'efficacité de votre production
