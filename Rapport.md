# Task 1

to print value of env variable we use ***printenv*** 
i add env variable called **var_name** and it's value is  **haithem** 


```bash
[06/04/23]seed@VM:~$ printenv PWD
/home/seed
[06/04/23]seed@VM:~$ env |grep PWD
PWD=/home/seed
[06/04/23]seed@VM:~$ export var_name=haithem
[06/04/23]seed@VM:~$ printenv var_name
haithem
[06/04/23]seed@VM:~$ env | grep var_name
var_name=haithem
[06/04/23]seed@VM:~$ unset var_name
[06/04/23]seed@VM:~$ printenv var_name
[06/04/23]seed@VM:~$ cd Desktop/
```
To remove the variable we use with command ***unset***

# Task 2

To compile the the **myprintenv.c** i use ***gcc*** and it will generate **a.out** and i will put his output to file1

````bash
[06/04/23]seed@VM:~/.../Labsetup$ gcc myprintenv.c 
[06/04/23]seed@VM:~/.../Labsetup$ ./a.out > file1
[06/04/23]seed@VM:~/.../Labsetup$ ls
a.out  cap_leak.c  catall.c  file1  myenv.c myprintenvc
````
## Step 2.
Now comment out the printenv()statement in the child process case (Line1), and uncomment the printenv()statement in the parent process case (Line2). Compile and run the code again,  anddescribe your observation. Save the output in another file.
````c
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>

extern char **environ;

void printenv()
{
  int i = 0;
  while (environ[i] != NULL) {
     printf("%s\n", environ[i]);
     i++;
  }
}

void main()
{
  pid_t childPid;
  switch(childPid = fork()) {
    case 0:  /* child process */
      // printenv();          
      exit(0);
    default:  /* parent process */
       printenv();       
      exit(0);
  }
}

````

after i will compile it and save the output the **file2**

````bash
[06/04/23]seed@VM:~/.../Labsetup$ gcc myprintenv.c 
[06/04/23]seed@VM:~/.../Labsetup$ ./a.out > file2
[06/04/23]seed@VM:~/.../Labsetup$ ls
a.out  cap_leak.c  catall.c  file1  file2  myenv.c  myprintenv.c
````
when i use ***diff*** command it doesn't show anything 
it mean that there is no different between the **file1** and **file2**
````bash
[06/04/23]seed@VM:~/.../Labsetup$ diff file1 file2
[06/04/23]seed@VM:~/.../Labsetup$ 
````

# Task 3

when i compile and run **myenv.c** it doesn't show anything 

````bash
[06/04/23]seed@VM:~/.../Labsetup$ gcc myenv.c 
[06/04/23]seed@VM:~/.../Labsetup$ ./a.out 
[06/04/23]seed@VM:~/.../Labsetup$ 
````
## Step 2.
when i change the invocation of execve on line 1

````c
#include <unistd.h>

extern char **environ;

int main()
{
  char *argv[2];

  argv[0] = "/usr/bin/env";
  argv[1] = NULL;

  execve("/usr/bin/env", argv, environ);  

  return 0 ;
}
````

and i compile and run the program it will show this (it is long output i don't provide all of it ) 

````bash
[06/04/23]seed@VM:~/.../Labsetup$ gcc myenv.c 
[06/04/23]seed@VM:~/.../Labsetup$ ./a.out 
SHELL=/bin/bash
SESSION_MANAGER=local/VM:@/tmp/.ICE-unix/2326,unix/VM:/tmp/.ICE-unix/2326
QT_ACCESSIBILITY=1
COLORTERM=truecolor
XDG_CONFIG_DIRS=/etc/xdg/xdg-ubuntu:/etc/xdg
XDG_MENU_PREFIX=gnome-
GNOME_DESKTOP_SESSION_ID=this-is-deprecated
GNOME_SHELL_SESSION_MODE=ubuntu
SSH_AUTH_SOCK=/run/user/1000/keyring/ssh
XMODIFIERS=@im=ibus
DESKTOP_SESSION=ubuntu
SSH_AGENT_PID=2275
GTK_MODULES=gail:atk-bridge
PWD=/home/seed/Desktop/Env_var/Labsetup
LOGNAME=seed
XDG_SESS......
````






