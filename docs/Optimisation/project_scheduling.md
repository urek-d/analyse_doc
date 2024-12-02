# Optimisation de la Planification des Projets

## Introduction

L'optimisation de la planification des projets vise à minimiser le temps total nécessaire pour terminer un projet en planifiant les tâches de manière optimale.

## Code Explication
Precedence (précédence) dans le contexte de l'optimisation de la planification des projets fait référence à l'ordre dans lequel les tâches doivent être effectuées. Par exemple, si la tâche B ne peut commencer qu'après la fin de la tâche A, alors A est une tâche de précédence pour B. Cela signifie que B est dépendante de A et que cette dépendance doit être respectée lors de la planification.
### Importation de la Bibliothèque

Nous utilisons `cp_model` d'OR-Tools pour la modélisation de contraintes.

```python
from ortools.sat.python import cp_model
```

### Données des Tâches
Voici un exemple de données de tâches avec leurs durées et précédences.
```python
tasks = {
    'task1': {'duration': 3, 'precedence': []},
    'task2': {'duration': 4, 'precedence': ['task1']},
    'task3': {'duration': 2, 'precedence': ['task2']},
}
```

### Création du Modèle
Nous créons un modèle de satisfaction de contraintes avec OR-Tools
```python
model = cp_model.CpModel()
```
### Variables de Début de Tâche
Nous déclarons des variables représentant les moments de début des tâches.
```python
task_starts = {}
for task, data in tasks.items():
    task_starts[task] = model.NewIntVar(0, 20, task + '_start')
```
### Contraintes de Précédence
Nous ajoutons des contraintes pour s'assurer que les tâches respectent les précédences.
```python
for task, data in tasks.items():
    for prec in data['precedence']:
        model.Add(task_starts[task] >= task_starts[prec] + tasks[prec]['duration'])
```
### Fonction Objectif
Nous définissons l'objectif pour minimiser le makespan
```python
makespan = model.NewIntVar(0, 20, 'makespan')
model.AddMaxEquality(makespan, [task_starts[task] + data['duration'] for task, data in tasks.items()])
model.Minimize(makespan)
```
### Résolution
Nous utilisons cp_model.CpSolver pour résoudre le modèle et obtenir l'ordonnancement optimal.
```python
solver = cp_model.CpSolver()
status = solver.Solve(model)

if status == cp_model.OPTIMAL:
    for task in tasks:
        print(f"{task} starts at {solver.Value(task_starts[task])}")
    print(f"Optimal makespan: {solver.Value(makespan)}")
```
### Conclusion
Ce modèle montre comment optimiser la planification des tâches pour minimiser le temps total de réalisation d'un projet.