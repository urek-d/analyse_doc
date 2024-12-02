# Optimisation de la Gestion de Portefeuille

## Introduction

L'optimisation de portefeuille consiste à trouver l'allocation optimale des actifs pour maximiser le rendement attendu tout en minimisant le risque.

## Code Explication

### Importation des Bibliothèques

Nous utilisons `pandas` pour la manipulation des données, `numpy` pour les opérations mathématiques, et `cvxopt` pour l'optimisation quadratique.

```python
import pandas as pd
import numpy as np
from cvxopt import matrix, solvers
```
### Données de Rendements Historiques
Nous générons des rendements fictifs pour les actifs.
```python
data = {
    'AAPL': np.random.normal(0.001, 0.02, 100),
    'GOOG': np.random.normal(0.0012, 0.022, 100),
}

df = pd.DataFrame(data)
```


### Calcul des Rendements Moyens et de la Matrice de Covariance
Nous calculons les rendements moyens et la matrice de covariance des rendements.
```python
returns = df.mean().values
covariances = df.cov().values
```
### Matrices pour l'Optimisation Quadratique
Nous construisons les matrices nécessaires pour le solveur quadratique.
```python
Q = matrix(covariances)
p = matrix(-returns)
G = matrix(-np.eye(len(returns)))
h = matrix(0.0, (len(returns), 1))
A = matrix(1.0, (1, len(returns)))
b = matrix(1.0)
```
### Résolution
Nous utilisons solvers.qp pour résoudre le problème d'optimisation quadratique et obtenir l'allocation optimale du portefeuille.
```python
sol = solvers.qp(Q, p, G, h, A, b)
optimal_portfolio = sol['x']
print(f"Optimal portfolio allocation: {optimal_portfolio}")
```
### Conclusion
Ce modèle montre comment optimiser un portefeuille en utilisant des techniques d'optimisation quadratique.
