# Le PATH
Au sein d'un ordinateur, le PATH est une [[variable d'environnement]] qui liste les dossiers du systeme d'exploitation qui devraient etre passes en revue pour trouver des executables ou des commandes quand on les invoque dans le shell. C'est un composant crucial de l'Interface en Ligne de Commandes (CLI) et de la maniere dont le systeme d'exploitation localise et execute les programmes.

Lorsaue l'on tape une commande dans le terminal, le systeme d'exploitation cherche dans les dossiers mentionnes dans le PATH jusqu'a ce qu'il trouve l'executable avec le nom que l'on avait specifie. Si l'executable est trouve dans l'un des dossiers du PATH, il est execute; sinon le terminal renverra une erreur de type "command not found".

Le PATH est une liste de dossiers separes par un caractere particulier (`:` sur les systemes UNIX tels que Linux ou MacOS, `;` sur Windows).

```
+-------------------+
|   Terminal/Shell  |
+-------------------+
        |
        | program_name
        v
+-------------------+
| PATH Environment  |
| Variable          |
|                   |
| /bin:/usr/bin     |
| /usr/local/bin    |
+-------------------+
        |
        |
+-------v------+--------------------------+
|      /bin    | /usr/bin | /usr/local/bin|
|  +-------+   |  +-------+-------+       |
|  | prog1 |   |  | prog2 | prog3 |       |
|  +-------+   |  +-------+-------+       |
+--------------+--------------------------+
```
Dans ce schema:
1. Lorsque l'on invoque une commande `program_name` dans le terminal, le systeme d'exploitation cherche une correspondance dans les dossiers listes dans le PATH.
2. Le PATH contient une liste de dossiers tels que `/bin`, `/usr/bin` et `usr/local/bin` (dans un systeme UNIX).
3. Le systeme d'exploitation cherche sequentiellement un executable nomme `program_name` dans chacun des dossiers.
4. Si l'executable est trouve (par exemple: `prog1` dans `/bin` ou `prog2` dans `/usr/bin`), il est execute. Sinon, la recherche continue dans le prochain dossier liste dans le PATH.
5. Si l'executable n'est dans aucun des dossiers listes dans le PATH, le terminal renvoie une erreur de type 'command not found'.

Le PATH aide les utilisateurs a execute des programmes sans avoir a specifier le chemin complet vers l'executable a chaque fois. Il permet aussi aux administrateurs systemes d'installer des logiciels dans des dossiers specifiques et de pouvoir acceder a ces programmes de n'importe ou dans le systeme.

N.B: Le PATH est habituellement configure durant l'installation du systeme d'exploitation, cependant les utilisateurs (surtout les developpeurs et les administrateurs systemes) peuvent le modifier pour ajouter ou retirer des dossiers selon les besoins.
# pipe()
La fonction `pipe()` en C est un [[appel systeme]] utilise pour faire de la [[communication inter-processus]] (IPC). Elle cree un pipe, c'est-a-dire un canal de donnees unidirectionnel qui permet un flux de donnees d'un processus a un autre. Le pipe agit comme un [[buffer]], permettant au processus expediteur d'ecrire des donnees dedans puis au processus destinataire de lire les donnees dudit buffer.

Voici la syntaxe de la fonction `pipe()`:
``` C
#include <unistd.h>

int pipe(int pipefd[2]);
```
La fonction `pipe()` prend un tableau de deux entiers (`pipefd`) en tant qu'argument. Apres une execution reussie, il cree un pipe and stocke deux descripteurs de fichiers dans le tableau `pipefd`:
- `pipefd[0]`: La tete de lecture du pipe.
- `pipefd[1]`: La tete d'ecriture du pipe.
La valeur de retour de `pipe()` est `0` en cas de succes et `-1` en cas d'erreur (auquel cas, la [[variable globale]] `errno` est initialisée pour indiquer l'erreur).

Voici un schema:
```
+---------------+       +---------------+
|   Process A   |       |   Process B   |
|               |       |               |
|   pipefd[1]   |       |   pipefd[0]   |
|      Write    |<----->|      Read     |
|               |       |               |
+---------------+       +---------------+
```
Le flux de donnes du pipe est unidirectionnel, ce qui veut dire que les donnees ne peuvent se deplacere que de la tete d'ecriture vers la tete de lecture. Le pipe agit comme un [[buffer|tampon]], ce qui permet au processus expediteur d'ecrire des donnees dedans et au processus receveur de lire son contenu.

Voici un exemple d'utilisation d'un pipe pour communiquer entre deux processus en C:
``` C
#include <unistd.h>
#include <stdio.h>
#include <string.h>

int main() {
    int pipefd[2];
    char buffer[256];
    
    // Create the pipe
    if (pipe(pipefd) == -1) {
        perror("pipe");
        return 1;
    }
    
    // Fork a new process
    pid_t pid = fork();
    
    if (pid == 0) { // Child process
        // Close the write end of the pipe
        close(pipefd[1]);
        
        // Read from the pipe
        ssize_t bytes_read = read(pipefd[0], buffer, sizeof(buffer));
        if (bytes_read > 0) {
            buffer[bytes_read] = '\0';
            printf("Child process received: %s\n", buffer);
        }
        
        // Close the read end of the pipe
        close(pipefd[0]);
    } else if (pid > 0) { // Parent process
        // Close the read end of the pipe
        close(pipefd[0]);
        
        // Write to the pipe
        const char *message = "Hello from parent!";
        write(pipefd[1], message, strlen(message) + 1);
        
        // Close the write end of the pipe
        close(pipefd[1]);
    } else {
        perror("fork");
        return 1;
    }
    
    return 0;
}
```
Dans cet exemple :
1. La fonction `pipe()` est appelée pour créer un nouveau pipe et les [[descripteurs de fichiers]] pour lire et écrire sont stockes dans le tableau `pipefd`.
2. La fonction `fork()` est appelée pour créer un nouveau processus enfant.
3. Dans le processus enfant, la tête d’écriture du pipe (`pipefd[1]`) est fermée puisque l'enfant a seulement besoin de lire depuis le pipe. L'enfant lit ensuite les données depuis la tête de lecture du pipe (`pipefd[0]`) et écrit le message reçu.
4. Dans le processus parent, la tête de lecture du pipe (`pipefd[0]`) est fermée étant donne que le parent a seulement besoin d’écrire dans le pipe. Le parent écrit ensuite un message dans la tête d’écriture du pipe (`pipefd[1]`).
Le pipe permet au processus parent d'envoyer des données au processus enfant au travers d'un canal de données unidirectionnel. Ceci est un exemple simple de communication inter-processus via un pipe en C.

Les pipes sont couramment utilises dans les systèmes d'exploitations bases sur [[UNIX]] pour diverses raisons, tels que rediriger le flux d'entree et de sortie, implementer des pipelines de commandes (par exemple: `grep pattern file | wc -l`) et construire des mécanismes d'IPC plus complexes comme les names pipes (FIFOs) et les pseudoterminaux.
# fork()
L'appel système `fork()` en C est utilise pour créer un nouveau processus, appelé le processus enfant. Le processus originel qui appelle `fork()` est lui appelé le processus parent. Apres l'appel a `fork()`, le processus parent et enfant continuent a exécuter le meme programme, mais ils ont des [[identifiants de processus]] (PID) différents et des espaces mémoire différents.

Voici la syntaxe de la fonction `fork()`:
``` C
#include <unistd.h>

pid_t fork(void);
```
La fonction `fork()` retourne deux fois:
1. Dans le processus parent, elle retourne le PID du processus enfant nouvellement créé. Si la valeur de retour est plus grande que 0, elle représente le PID du processus enfant.
2. Dans le processus enfant, elle retourne 0.
Si `fork()` échoue, elle retourne `-1`, et la variable globale `errno` est initialisée pour indiquer l'erreur.

Voici un schema du fonctionnement de `fork()`:
```
					+---------------+
					|   Parent      |
					|   Process     |
					+---------------+
							|
		                    | fork()
		                    |
	                +-------v---------+
		            |                 |
	+---------------+   +---------------+
	|   Parent      |   |   Child       |
	|   Process     |   |   Process     |
	+---------------+   +---------------+
```
Apres l'appel a `fork()`, le parent et l'enfant continuent a exécuter le meme programme en exécutant l'instruction juste après l'appel a `fork()`. Cependant, ils ont des espaces mémoire différents, ainsi n'importe quel changement fait aux variables ou a la mémoire d'un processus n'affectera pas l'autre processus.

Voici un exemple de comment utiliser `fork()` en C:
``` C
#include <stdio.h>
#include <unistd.h>

int main() {
    pid_t pid;
    pid = fork();

    if (pid < 0) {
        // Fork failed
        fprintf(stderr, "Fork failed\n");
        return 1;
    } else if (pid == 0) {
        // Child process
        printf("Hello from child process (PID: %d)\n", getpid());
    } else {
        // Parent process
        printf("Hello from parent process (PID: %d, Child PID: %d)\n", getpid(), pid);
    }

    return 0;
}
```
Dans cet exemple:
1. L'appel système `fork()` est execute, créant un nouveau processus enfant.
2. Si `fork()` échoue (retourne -1), un message d'erreur est affiche.
3. Si la valeur de retour est 0, cela signifie que le processus actuel est le processus enfant. Le processus enfant imprime un message avec son PID.
4. Si la valeur de retour est supérieur a 0, cela veut dire que le processus actuel est le processus parent. Le processus parent affiche un message avec son PID et le PID de l'enfant.
La sortie de ce programme devrait ressembler a cela:
```
Hello from parent process (PID: 12345, Child PID: 12346)
Hello from child process (PID: 12346)
```
En general l'appel système `fork()` est utilise dans divers scenarios, tels que:
- Créer de nouveaux processus pour exécuter différentes taches en parallèle.
- Implementer des processus serveur qui peuvent gérer plusieurs requêtes client simultanément.
- Construire des mécanismes de communication inter-processus plus complexes, tels que des pipes, message queues ou de la shared memory.
- Implementer des processus de fond ou des [[daemons]].

N.B: Apres l'appel `fork()`, le parent et l'enfant partagent certaines ressources telles que les descripteurs de fichiers ouverts, les gestionnaires de signaux et les autres attributs de processus. Une gestion méticuleuse des ressources et du cleanup est vitale pour éviter des problèmes comme la corruption de données ou les fuites de ressources.
# dup2()
