## What is a Process?
When running a program, that program is going to be processed by the CPU. But, if a program had to wait for the previous program to finish what its doing, then that would make the computer very slow. A way around this is by **virtualization**, where various programs are sharing the same resources, and by resource I mean the CPU. With this, a bunch of *processes* can be running at the "same" time. 
In fact it is not running at the same time, but in different chunks of time. This method is called **time sharing**.

> **time sharing**:
> is a basic technique used by an OS to share a resource. By allowing the resource to be used for a little while by one entity, and then a little while by another, and so forth, the resource in question (e.g., the CPU, or a network link) can be shared by many. The counterpart of time sharing is space sharing, where a resource is divided (in space) among those who wish to use it. For example, disk space is naturally a space-shared resource; once a block is assigned to a file, it is normally not assigned to another file until the user deletes the original file.

## How to controll it
As you can imagine, all operating Systems nowadays offer a Process API, where we can take advantage of this technics used by the CPU. This are some basic API's:
+ **Create**: An operating system must have this option available. Whenever you start a new program, or open any app on your computer, the OS is invoked to create a new process.
+ **Destroy**: If we have an option to create a process, we should also have an option to kill it. Most programs exit themselves when finished, however, for some reason whe should be able to kill a process.
+ **Wait**: Sometimes we need to wait for a process to finish what its doing. Imagine we want to wait for its result in order to proceede to somehting else, we would need to wait. 

There are many more API's available to controll and manage processes, these are some of the most basic ones. 

## Creating processes in a little more detail
The first thing the OS needs to do in order to start a process it to load a program that resides in the disk, into memory.
Once loaded into memory, the OS needs to alocate space for the program stack and the heap.
The OS will also do some other initialization taks for the I/O. For example, in UNIX systems, each process has three *file descriptors*, for _standard input_, _output_, and _error_.

## Process states
When running processes can have many states. Those being:
- **Running**: The process is running on the processor. Which means it is executing the instructions. 
- **Ready**: When in this state, the process is ready to run, but it's not running yet. This may happen when a process is waiting for another one to finish in order to start. 
- **Blocked**: Sometimes we need to block processes to allow other processes to run in order to keep going. Imagine we have a process running and initiates and I/O operation, since that will take some time, it becomes blocked, allowing other processes to go. 

## OS data structures
As any other program, the OS needs some data structures for it to work, since the OS is also a program. So the OS stores all the process and states happening, in some file in the kernell. It will also store information such as the CPU registers state, so for when a process is blocked, for example, when it returns to what it was doing, it knows how to restore the data in the physical registers. This is known as **context switching**. 

> **Process List**:
> Operating systems are replete with various important data structures that we will discuss in these notes. The process list (also called the task list) is the first such structure. It is one of the simpler ones, but certainly any OS that has the ability to run multiple programs at once will have something akin to this structure in order to keep track of all the running programs in the system. Sometimes people refer to the individual structure that stores information about a process as a Process Control Block (PCB), a fancy way of talking about a C structure that contains information about each process (also sometimes called a process descriptor).
