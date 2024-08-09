Chaque programme a un nombre de processus et chaque processus a un nombre de threads. Un thread est une unité basique de charge CPU

```
Thread
__________________
| Thread ID      |
|________________|
|Program counter |
|________________|
|Register set    |
|________________|
|Stack           |
|________________|
|________________|
```
Un thread partage avec les autres threads du même processus, son code, ses données et les autres ressources systèmes telles que les fichiers ouverts et les signaux.

Par exemple dans un navigateur, on peut voir la page, télécharger un fichier et streamer un flux vidéo. Seul le multi-threading permet cela, sinon avec un programme avec un seul thread, on ne pourrait plus voir la page en téléchargeant un fichier par exemple, on ne pourrait que faire soit l'un, soit l'autre.
# Avantages du multi-threading
- Reactivité : Le multi-threading permet à un programme de continuer à s'exécuter même si une partie du programme est "bloqué" par l'exécution d'une tache chronophage, ce qui permet de gagner en reactivité.
- Partage de ressource : Les threads partagent la mémoire et les ressources du processus auxquels ils appartiennent. Le bénéfice est donc de permettre à l'application d'avoir plusieurs threads dans la même zone d'adresses.
- Économie : Allouer de la mémoire pour les process est coûteux. Parce que les threads partagent les ressources des processus auxquels ils appartiennent, il est plus économique de créer et de "context-switch" des threads.
- Utilisation de l'architecture multi-processeur : Un processus avec un seul thread n'utilisera qu'un seul CPU, le multi-threading tirera parti du fait que l'on ait plusieurs processeurs pour être encore plus efficace.

- [ ] 