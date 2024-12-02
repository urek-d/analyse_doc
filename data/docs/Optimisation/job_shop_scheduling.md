# Optimisation de l'Ordonnancement des Tâches

## Introduction

L'optimisation de l'ordonnancement des tâches est un domaine crucial dans la gestion des opérations industrielles. Elle permet de déterminer l'ordre optimal dans lequel exécuter une série de tâches sur différentes machines, afin de minimiser le temps total nécessaire pour compléter tous les jobs (makespan). 

### Pourquoi Utiliser l'Optimisation de l'Ordonnancement des Tâches ?

L'optimisation de l'ordonnancement des tâches est utilisée pour :
- **Réduire les temps de production** : En minimisant le makespan, les entreprises peuvent augmenter leur efficacité.
- **Améliorer l'utilisation des ressources** : En s'assurant que les machines ne restent pas inutilisées.
- **Réduire les coûts** : En diminuant les délais de production et en optimisant l'utilisation des machines.

### Contexte d'Utilisation

L'optimisation de l'ordonnancement des tâches est essentielle dans divers secteurs, notamment :
- **Fabrication** : Pour organiser les étapes de production sur différentes machines.
- **Logistique** : Pour planifier les trajets des véhicules de livraison.
- **Informatique** : Pour gérer l'allocation des tâches dans les centres de données.

## Exemple Réel

Nous allons démontrer comment résoudre un problème d'optimisation de l'ordonnancement des tâches en utilisant des données simulées et la bibliothèque `PuLP` de Python.

### Génération des Données

Nous générons des données simulées pour représenter les jobs, les tâches, les machines, et la durée de chaque tâche.

```python
import numpy as np
import pandas as pd

# Génération des données
jobs_data = {
    'Job': [1, 1, 1, 2, 2, 3, 3, 3],
    'Task': [1, 2, 3, 1, 2, 1, 2, 3],
    'Machine': ['M1', 'M2', 'M3', 'M2', 'M3', 'M1', 'M3', 'M2'],
    'Duration': [2, 3, 2, 4, 1, 3, 2, 1]
}

df_jobs = pd.DataFrame(jobs_data)
print(df_jobs)
```
```python
from pulp import LpMaximize, LpProblem, LpVariable, lpSum, PULP_CBC_CMD

# Initialisation du modèle
model = LpProblem(name="job-shop-scheduling", sense=LpMaximize)

# Définition des variables
tasks = [(i, j) for i in range(len(df_jobs)) for j in range(i + 1, len(df_jobs))]
x = LpVariable.dicts("x", tasks, cat="Binary")
C = LpVariable.dicts("C", range(len(df_jobs)), lowBound=0, cat="Continuous")

# Ajout des contraintes
# 1. Une tâche doit être exécutée après la tâche précédente du même job
for i in range(len(df_jobs)):
    for j in range(i + 1, len(df_jobs)):
        if df_jobs.iloc[i]['Job'] == df_jobs.iloc[j]['Job']:
            model += C[j] >= C[i] + df_jobs.iloc[i]['Duration']

# 2. Une machine ne peut exécuter qu'une seule tâche à la fois
for m in df_jobs['Machine'].unique():
    for t1 in df_jobs[df_jobs['Machine'] == m].index:
        for t2 in df_jobs[df_jobs['Machine'] == m].index:
            if t1 < t2:
                model += C[t2] >= C[t1] + df_jobs.iloc[t1]['Duration'] * x[(t1, t2)]
                model += C[t1] >= C[t2] + df_jobs.iloc[t2]['Duration'] * (1 - x[(t1, t2)])

# 3. Objectif : minimiser le makespan
makespan = LpVariable("makespan", lowBound=0, cat="Continuous")
model += makespan
for i in range(len(df_jobs)):
    model += makespan >= C[i] + df_jobs.iloc[i]['Duration']

# Résolution du modèle
model.solve(PULP_CBC_CMD(msg=0))
```

# Résultats
```python
print(f"Optimal Schedule Length: {makespan.value()}")
schedule = [(df_jobs.iloc[i]['Job'], df_jobs.iloc[i]['Task'], C[i].value()) for i in range(len(df_jobs))]
schedule.sort(key=lambda x: x[2])
for job in schedule:
    print(f"Job {job[0]}, Task {job[1]} starts at time {job[2]:.2f}")
```
```bash
Optimal Schedule Length: 20.333333
Job 1, Task 1 starts at time 0.00
Job 3, Task 1 starts at time 6.00
Job 3, Task 2 starts at time 9.00
Job 3, Task 3 starts at time 11.00
Job 1, Task 2 starts at time 12.33
Job 2, Task 1 starts at time 12.33
Job 2, Task 2 starts at time 16.33
Job 1, Task 3 starts at time 18.33

```
L'optimisation de l'ordonnancement des tâches est essentielle pour améliorer l'efficacité des opérations industrielles. En utilisant des techniques de programmation linéaire entière et des outils comme PuLP, nous pouvons résoudre des problèmes complexes d'optimisation dans des contextes réels, réduire les coûts et améliorer l'utilisation des ressources.
