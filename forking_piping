#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <stdlib.h>
#include <string.h>
#include <sys/wait.h>

#define MAX_CHILDREN 3

int main(void)
{
    int fd[2];
    char name[20];

    void sigHandler(int sig);
    
    if(signal(SIGINT, sigHandler) == SIG_ERR)
    {
        perror("Signal error!");
        exit(1);
    }

    for(int num_process = 0; num_process < MAX_CHILDREN; num_process++)
    {
        printf("Input name: ");
        scanf("%s", name);
        
        if(pipe(fd) == -1)
        {
            perror("piping Failed!");
            continue;
        }

        pid_t pid = fork();
        
        if(pid == 0)
        { //child code
            printf("Name is: %s\n", name);
            
            char buff[50];
            printf("Child %i (pid = %i)\n", num_process, getpid());
            close(fd[1]);

            if (read( fd[0], buff, sizeof(buff)) <= 0) //read from pipe
            {
                perror("Reading failed!");
                exit( EXIT_FAILURE );
            }

            printf("Reading child = %s\n", buff);
            exit(0);
        }
        
        if(pid < 0)
        {
            perror("Forking failed!");
            exit(1);
        }

        else
        { //parent

            printf("Parent %i\n",getpid());
            close(fd[0]);
            write(fd[1], name,strlen(name)+1);
            printf("Parent sends %s\n", name);
            wait(NULL);
        }
    }

    return 0;
}

void sigHandler(int sig)
{
    printf("Keyboard signal interrupt");
    exit(1);
}
