# Pour bien préparer push_swap
## La notion de complexité
### Qu'est-ce que la complexité ?
### Les notations de Landau
#### Ω : Grand Omega
Cette notation est utilisée pour décrire la borne basse d'un algorithme (son meilleur cas donc).
#### O : Grand O
Cette notation est utilisée pour décrire la borne haute d'un algorithme (son pire cas donc).
#### Θ : Grand Theta
Cette notation est utilisée pour décrire la borne haute et la borne basse d'un algorithme, ce qui permet de savoir à quel moment ce dernier perd en efficacité.
### Toutes les fonctions pour évaluer la complexité d'un algorithme
| $$O$$                   | Complexité                | Problème exemple                                                |
| ----------------------- | ------------------------- | --------------------------------------------------------------- |
| $$O(1)$$                | Constante                 |                                                                 |
| $$O(log(n))$$           | Logarithmique             | Recherche dichotomique                                          |
| $$O(\sqrt{n})$$         | Racinaire                 |                                                                 |
| $$O(n)$$                | Linéaire                  | Parcours de liste                                               |
| $$O(n\ log\cdot(n))$$   | Quasi-linéaire            |                                                                 |
| $$O(n\ log(n))$$        | Linéarithmique            | Tris par comparaisons optimaux (ex: tri fusion, tri par le bas) |
| $$O(n^2)$$              | Quadratique (polynomiale) | Parcours de tableaux 2D                                         |
| $$O(n^3)$$              | Cubique (polynomiale)     |                                                                 |
| $$O(2^{poly(log(n))})$$ | Sous-exponentielle        |                                                                 |
| $$O(2^{poly(n)})$$      | Exponentielle             |                                                                 |
| $$O(n!)$$               | Factorielle               |                                                                 |
| $$O(2^{2^{poly(n)}})$$  | Doublement exponentielle  |                                                                 |
## Les algorithmes de tri
>Note:
>- n représente le nombre d'éléments dans le tableau en entrée.
>- k représente la plage de valeurs dans le tableau en entrée (pour le Radix Sort et le Counting Sort).
### Bubble sort
| Scenario   | Notations          |
| ---------- | ------------------ |
| Worst Case | O(²), Ω(n²), Θ(n²) |
| Best Case  | O(n), Ω(n), Θ(n)   |
### Insertion sort
| Scenario   | Notations          |
| ---------- | ------------------ |
| Worst Case | O(²), Ω(n²), Θ(n²) |
| Best Case  | O(n), Ω(n), Θ(n)   |

### Selection sort
| Scenario   | Notations          |
| ---------- | ------------------ |
| Worst Case | O(²), Ω(n²), Θ(n²) |
| Best Case  | O(²), Ω(n²), Θ(n²) |

### Merge sort
| Scenario   | Notations                          |
| ---------- | ---------------------------------- |
| Worst Case | O(n log n), Ω(n log n), Θ(n log n) |
| Best Case  | O(n log n), Ω(n log n), Θ(n log n) |

### Quick sort
| Scenario   | Notations                          |
| ---------- | ---------------------------------- |
| Worst Case | O(n²), Ω(n log n), Θ(n log n)      |
| Best Case  | O(n log n), Ω(n log n), Θ(n log n) |

### Heap sort
| Scenario   | Notations                          |
| ---------- | ---------------------------------- |
| Worst Case | O(n log n), Ω(n log n), Θ(n log n) |
| Best Case  | O(n log n), Ω(n log n), Θ(n log n) |

### Shell sort
| Scenario   | Notations                          |
| ---------- | ---------------------------------- |
| Worst Case | O(n²), Ω(n log² n), Θ(n log n)     |
| Best Case  | O(n log n), Ω(n log n), Θ(n log n) |

### Radix sort
| Scenario   | Notations           |
| ---------- | ------------------- |
| Worst Case | O(kn), Ω(kn), Θ(kn) |
| Best Case  | O(kn), Ω(kn), Θ(kn) |

### Counting sort
| Scenario   | Notations              |
| ---------- | ---------------------- |
| Worst Case | O(n+k), Ω(n+k), Θ(n+k) |
| Best Case  | O(n+k), Ω(n+k), Θ(n+k) |

### Bucket sort
| Scenario   | Notations              |
| ---------- | ---------------------- |
| Worst Case | O(n²), Ω(n+k), Θ(n+k)  |
| Best Case  | O(n+k), Ω(n+k), Θ(n+k) |

# Mettre en place un système de listes chaînées
### Parsing de la commande
La commande pour lancer le programme sera la suivante
`./push_swap 2 1 3 6 5 8`
Le parsing doit donc découper cet argument comme suit:

| argv\[0]      | argv\[1] | argv\[2] | argv\[3] | argv\[4] | argv\[5] | argv\[6] |
| ------------- | -------- | -------- | -------- | -------- | -------- | -------- |
| `./push_swap` | `2`      | `1`      | `3`      | `6`      | `5`      | `8`      |
### Création de la pile A
>Le premier paramètre donné en commande doit être au sommet de la pile !

Pour générer notre pile A, nous itérons à travers les arguments et à chaque fois on add back. De cette manière, notre dernier argument sera le dernier élément de la liste chaînée, c'est-à-dire qu'il sera en bas de notre pile.
### Création de la pile B
	La stack B étant vide au début de notre processus, on initialise simplement notre `t_stack *stack_b` à `NULL`.
### Mise en place des instructions
#### sa (swap a)
#### sb (swap b)
#### ss (sa et sb en même temps)
#### pa (push a)
#### pb (push b)
#### ra (rotate a)
#### rb (rotate b)
#### rr (ra et rb en même temps)
#### rra (reverse rotate a)
#### rrb (reverse rotate b)
#### rrr (rra et rrb en même temps)
# Affichage sur la sortie standard des commandes exécutées
# L'algorithmique
## Choix des algorithmes à utiliser
## Implémentation de l'algorithme