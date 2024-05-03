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
3. Le système d'exploitation cherche sequentiellement un executable nomme `program_name` dans chacun des dossiers.
4. Si l'executable est trouve (par exemple: `prog1` dans `/bin` ou `prog2` dans `/usr/bin`), il est execute. Sinon, la recherche continue dans le prochain dossier liste dans le PATH.
5. Si l'executable n'est dans aucun des dossiers listes dans le PATH, le terminal renvoie une erreur de type 'command not found'.

Le PATH aide les utilisateurs a execute des programmes sans avoir a specifier le chemin complet vers l'executable a chaque fois. Il permet aussi aux administrateurs systemes d'installer des logiciels dans des dossiers specifiques et de pouvoir acceder a ces programmes de n'importe ou dans le systeme.

N.B: Le PATH est habituellement configure durant l'installation du systeme d'exploitation, cependant les utilisateurs (surtout les developpeurs et les administrateurs systemes) peuvent le modifier pour ajouter ou retirer des dossiers selon les besoins.
# pipe()
La fonction `pipe()` en C est un [[appel système]] utilise pour faire de la [[communication inter-processus]] (IPC). Elle crée un pipe, c'est-a-dire un canal de données unidirectionnel qui permet un flux de données d'un processus a un autre. Le pipe agit comme un [[buffer]], permettant au processus expéditeur d’écrire des données dedans puis au processus destinataire de lire les données dudit buffer.

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
Le flux de donnes du pipe est unidirectionnel, ce qui veut dire que les donnees ne peuvent se déplacer que de la tête d'écriture vers la tête de lecture. Le pipe agit comme un [[buffer|tampon]], ce qui permet au processus expéditeur d'écrire des données dedans et au processus receveur de lire son contenu.

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
L'appel système `dup2()` en C est utilise pour dupliquer un descripteur de fichier existant vers un autre descripteur de fichier. Il est communément utilisé dans des cas ou l'on a besoin de rediriger le d'entree, de sortie (par exemple: l'entree standard, la sortie standard, l'erreur standard) vers ou depuis des fichiers, des pipes ou d'autres descripteurs de fichiers.

Voici la syntaxe de la fonction `dup2()`:
``` C
#include <unistd.h>

int dup2(int oldfd, int newfd);
```
La fonction `dup2()` prend deux arguments:
1. `oldfd`: Le descripteur de fichier qui doit être duplique
2. `newfd`: La valeur du nouveau descripteur de fichier après la duplication.

La fonction `dup2()` fonctionne comme suit:
1. Si `newfd` est déjà ouvert, il est d'abord fini.
2. Le descripteur de fichier `oldfd` est duplique et assigne a la variable `newfd`.
3. Apres l'appel a `dup2()`, le `oldfd` et le `newfd` se referent au meme descripteur de fichier et partagent la meme file offset.
La fonction retourne le nouveau descripteur de fichier (`newfd`) en cas de succès ou `-1` en cas d'erreur (auquel cas la variable globale `errno` est initialisée pour indiquer l'erreur).

Voici un schema du fonctionnement de `dup2()`:
```
Before dup2():

+---------------+             +---------------+
|   File        |             |   File        |
|   Descriptor  |             |   Descriptor  |
|   (oldfd)     |             |   (newfd)     |
+---------------+             +---------------+
        |                             |
        |                             |
        v                             v
+---------------+             +---------------+
|   Open File   |             |   Open File   |
|   Description |             |   Description |
+---------------+             +---------------+

After dup2(oldfd, newfd):

+---------------+             +---------------+
|   File        | --------->  |   File        |
|   Descriptor  |             |   Descriptor  |
|   (oldfd)     |             |   (newfd)     |
+---------------+             +---------------+
        |                             |
        |                             |
        v                             v
+---------------------------------------------+
|                  Open File                  |
|                 Description                 |
+---------------------------------------------+
```
Un usage commun de `dup2()` est de rediriger les flux de l'entree/sortie/erreur standard. Par exemple, pour rediriger la sortie standard vers un fichier, on peut utiliser `dup2()` comme suit:
``` C
#include <unistd.h>
#include <fcntl.h>

int main() {
    int fd = open("output.txt", O_WRONLY | O_CREAT | O_TRUNC, 0644);
    if (fd == -1) {
        // Handle error
    }

    if (dup2(fd, STDOUT_FILENO) == -1) {
        // Handle error
    }

    // From this point on, any output to standard output (stdout) will be written to output.txt
    printf("This will be written to output.txt\n");

    close(fd);
    return 0;
}
```
Dans cet exemple:
1. L'appel `open()` est utilise pour créer ou tronquer le fichier "output.txt" pour écrire.
2. La fonction `dup2()` est appelée pour dupliquer le descripteur de fichier `fd` (qui pointe sur "output.txt") vers la valeur descripteur de fichier de `STDOUT_FILENO` (qui représente la sortie standard).
3. Apres l'appel a `dup2()`, n'importe quelle sortie écrite vers `stdout` (par exemple: utiliser `printf()`) sera redirige vers le fichier "output.txt".
L'appel système `dup2()` est aussi utilise communément en combinaison avec l'appel système `pipe()` pour faire de la communication inter-processus (IPC) et des scenarios de redirection de processus.
# execve()
L'appel système `execve()` en C est utilise pour exécuter un programme ou une commande dans le système d'exploitation. Cette fonction fait partie de la famille des fonctions `exec`, qui remplacent l'image actuelle du processus avec une nouvelle image. Cela signifie que le processus qui fait l'appel est termine pour laisser place a un nouveau processus créé avec le programme ou la commande spécifiée.

Voici la syntaxe de la fonction `execve()`:
``` C
#include <unistd.h>

int execve(const char *path, char *const argv[], char *const envp[]);
```
La fonction `execve()` prend trois arguments:
1. `path`: Une chaîne de caractères représentant le chemin de l'executable a exécuter.
2. `argv`: Un tableau de chaînes de caractères qui contient la liste des arguments pour le nouveau programme. Le premier argument (`argv[0]`) doit être le nom du programme.
3. `envp`: Un tableau de chaînes de caractères représentant les variables d'environnement a passer au nouveau programme.

Si `execve()` s'execute sans problèmes, il ne retourne pas au processus appellant. A la place, le processus appellant est remplace par le nouveau programme et ce dernier commence a s’exécuter depuis son point d'entree (la fonction `main()` en C).
Si `execve()` échoue, il retourne `-1` et la variable globale `errno` est initialisée pour indiquer l'erreur. Dans ce cas, le processus appellant continue a s’exécuter après l'appel a `execve()`.

Voici un schema du fonctionnement d'`execve():
```
+---------------+
|   Process A   |
|               |
| execve(path,  |
|        argv,  |
|        envp)  |
|               |
+---------------+
        |
        | (replaces current process image)
        v
+---------------+
|   Process B   |
| (New Program) |
|               |
+---------------+
```
Dans ce schéma, Process A appelle `execve()` avec le chemin vers le nouveau programme, ses arguments (`argv`) et ses variables d'environnement (`envp`). Le système d'exploitation crée ensuite un nouveau processus, Process B, en chargeant le programme spécifié dans la mémoire et en remplacant son image de processus actuelle (Process A) avec le nouveau programme.

Voici un exemple de comment utiliser `execve()` en C:
``` C
#include <unistd.h>
#include <stdio.h>

int main() {
    char *args[] = {"ls", "-l", NULL};
    char *env[] = {NULL};

    if (execve("/bin/ls", args, env) == -1) {
        perror("execve");
        return 1;
    }

    // This line will never be reached if execve() is successful
    printf("This line will not be printed\n");

    return 0;
}
```
Dans cet exemple:
1. Le tableau `args` est initialisé avec les arguments à passer au nouveau programme (`ls -l`). Le dernier argument doit être `NULL` pour indiquer la fin de la liste d'arguments.
2. Le tableau `env` est initialisé avec un seul pointeur sur `NULL`, indiquant que le nouveau programme doit hériter des variables d'environnement du processus faisant l'appel.
3. La fonction `execve()` est appelée avec le chemin vers la commande `ls` (`/bin/ls`), le tableau `args` et le tableau `env`.
4. Si `execve()` se déroule sans accrocs, le processus appellant est remplacé par la commande `ls` et la sortie du programme sera affichée sur la sortie standard.
5. Si `execve()` échoue, la fonction `perror()` est appelée pour afficher le message d´erreur et le programme quitte avec un statut non-nul.

L'appel système `execve()` est communément utilisé dans divers scénarios, tels que:
- Exécuter des programmes externes ou des commandes depuis un programme en C.
- Implémenter des fonctionnalités du [[shell]] ou des interprétateurs en ligne de commandes.
- Construire des systèmes de gestion de processus plus complexes ou hiérarchiser les processus.

N.B: Après une exécution réussie d'`execve()`, le processus appellant n'existe plus et le nouveau programme s'approprie ses espaces mémoires et ses ressources. De ce fait, les tâches de cleanup ou de gestion de ressources devraient être exécutées avant d'appeler `execve()`.