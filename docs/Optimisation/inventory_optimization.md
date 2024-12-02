# Optimisation des Inventaires

## Introduction

L'optimisation des inventaires vise à déterminer le niveau de stock optimal pour minimiser les coûts tout en satisfaisant la demande. Les coûts comprennent les coûts de maintien des stocks et les coûts de rupture de stock.

## Code Explication

### Importation des Bibliothèques

Nous commençons par importer les bibliothèques nécessaires.

```python
import pandas as pd
import numpy as np
from scipy.optimize import minimize
```
### Génération de Données Fictives
Nous générons des données fictives de demande quotidienne sur un mois

```python
np.random.seed(0)
demand = np.random.poisson(20, 30)
holding_cost_per_unit = 0.5
shortage_cost_per_unit = 10
```
### Fonction Objectif
Cette fonction calcule le coût total en fonction du niveau de stock moyen.

```python
def total_cost(stock_level):
    holding_cost = holding_cost_per_unit * stock_level
    shortage_days = (demand > stock_level).sum()
    shortage_cost = shortage_cost_per_unit * shortage_days
    return holding_cost + shortage_cost
```


### Optimisation
Nous utilisons minimize pour trouver le niveau de stock qui minimise le coût total.
```python

initial_guess = [1]
bounds = [(0, None)]
solution = minimize(total_cost, initial_guess, bounds=bounds)
optimal_stock_level = solution.x[0]

print(f"Optimal inventory level: {optimal_stock_level:.2f}")
```

### conclusion
Ce modèle simple montre comment déterminer le niveau de stock optimal pour minimiser les coûts de maintien et de rupture de stock

