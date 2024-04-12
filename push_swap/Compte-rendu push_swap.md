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
# Mettre en place un système de listes chaînées
### Parsing de la commande
### Création de la pile A
>Le premier paramètre donné en commande doit être au sommet de la pile !

### Création de la pile B
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