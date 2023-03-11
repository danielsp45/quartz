---
quickshare-date: 2023-03-08 10:24:28
quickshare-url: "https://noteshare.space/note/clezj9g23194701pj6u4kg1di#JbLEHoUckIDeO+tannmknvPZCUCcGwRQZMu9UYn0G4E"
---
UNIX systems offer nit systems calls to interact with processes. Here we'll talk a bit about them.

## The `fork()` system call
The `fork()` system call is a way to create a new proceess almost identicall to the process it was called from. When you call `fork()` it's almost like a new program just started running from the point you called the systems function. That way if we have this piece of code:
```c
#include <stdio.h>  
#include <stdlib.h>  
#include <unistd.h>  
 
int main(int argc, char *argv[]) {  
	printf("hello world (pid:%d)\n", (int) getpid());  
	int rc = fork();  
	if (rc < 0) {  
		// fork failed  
		 fprintf(stderr, "fork failed\n");  
		 exit(1);  
	 } else if (rc == 0) {  
		 // child (new process)  
		 printf("hello, I am child (pid:%d)\n", (int) getpid());  
	 } else {  
		 // parent goes down this path (main)  
		 printf("hello, I am parent of %d (pid:%d)\n",  
		 rc, (int) getpid());  
	 }  
 return 0;  
 }
```
The output may be this:
```sh
prompt> ./p1  
hello world (pid:29146)  
hello, I am parent of 29147 (pid:29146)  
hello, I am child (pid:29147)  
prompt>
```

Notice how the child didn't start running from the beginning of the `main()` function. 

## The `wait()` system call

Sometimes you need to wait for a process to finish running. For this matter you can use the `wait()` system call (or it's more complete sibling `waitpid()`).
Look at this example:
```c
#include <stdio.h>  
#include <stdlib.h>  
#include <unistd.h>  
 
int main(int argc, char *argv[]) {  
	printf("hello world (pid:%d)\n", (int) getpid());  
	int rc = fork();  
	if (rc < 0) {  
		// fork failed  
		fprintf(stderr, "fork failed\n");  
		exit(1);  
	 } else if (rc == 0) {  
		// child (new process)  
		printf("hello, I am child (pid:%d)\n", (int) getpid());  
	 } else {  
		// parent goes down this path (main)  
		int rc_wait = wait(NULL);
		printf("hello, I am parent of %d (pid:%d)\n",  
		rc, (int) getpid());  
	 }  
 return 0;  
 }
```
The output is:
``` sh
prompt> ./p1  
hello world (pid:29146)  
hello, I am child (pid:29147)  
hello, I am parent of 29147 (pid:29146)  
prompt>
```
This example is quite similar to the one above, but it isn't. This example is *deterministic* which means that the output order in this one is always going to be the same because of the `wait()` system call which tells the parent process to wait for the child process and only after it can proceede. 


## The `exec()` system call

The `exec()` system call is a bit more difficult to understand than the other ones. Whenever you call this system call, you're switching the program running in the current process for another one. Here is an example:
```c
#include <stdio.h>  
#include <stdlib.h>  
#include <unistd.h>  
 
int main(int argc, char *argv[]) {  
	printf("hello world (pid:%d)\n", (int) getpid());  
	int rc = fork();  
	if (rc < 0) {  
		// fork failed  
		fprintf(stderr, "fork failed\n");  
		exit(1);  
	 } else if (rc == 0) {  
		// child (new process)  
		printf("hello, I am child (pid:%d)\n", (int) getpid());  
		char *myargs[3];
		myargs[0] = strdup("wc");
		myargs[1] = strdup("p3.c");
		myargs[2] = NULL;
		execvp(myargs[0], myargs); //this is the exec() system call
		printf("this shouldn't print out");
	 } else {  
		// parent goes down this path (main)  
		int rc_wait = wait(NULL);
		printf("hello, I am parent of %d (rc_wait:%d) (pid:%d)\n",
			rc, rc_wait, (int) getpid()); 
	 }  
 return 0;  
 }
```
And this should return:
```sh
prompt> ./p1  
hello world (pid:29383) 
hello, I am child (pid:29384) 
	29  107  1030 p3.c
hello, I am parent of 29384 (rc_wait:29384) (pid:29383) 
prompt>
```
As you can see, the "this shouldn't print out" didn't in fact print out. That's because the `exec()` system call made the current process run another program and leave this one. 

The separation of the `exec()` and `fork()` system calls are essential to achieve a variety of things because it lets the shell run code _after_ a fork() but _before_ the call to exec().

> **Note:
> There are many different `exec()` system calls. Read the man pages to learn more.


## The `kill()` system call

```c
int kill(pid_t** _pid_**, int** _sig_**);**
```
Beyond `fork()`, `exec()` and `wait()`, there are a lot of other interfaces for interacting with processes in UNIX systems. For example the `kill()` system call, which can be used to send a signal to any process or process group. This signals are used everywhere, and usually a certain keyboard stroke is a key to send a signal. For example in your terminal, you can send a signal to stop the program running with ctrl+c. That's because your shell terminal uses processes in order to work.
If the _pid_ is positive, then the signal _sig_ is sent to the process with the ID specified by _pid_. 
If the _pid_ equals 0, then _sig_ is sent to every process in the group of the calling process. 
If the _pid_ equals -1, then the _sig_ is sent to every process for which the process calling process has permission to send signals, except for process 1 (**init**)
If the _pid_ is less than -1, then _sig_ is sent to every process in the process group whose is **-pid**.