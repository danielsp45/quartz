We learned that processes run by time sharing the CPU. So we know that different processes are always running in the CPU and that raises some challenges. One of them being _performance_. Virtualization is a difficult thing to achieve but our computer handles it very well, how does it do that? The second question is about control. With so many processes running how can we controll all of them and make sure they don't break anything? Without control a process could run forever, so this topic is essential. 

## Limited Direct Execution
This first technique allows programs to run very fast, but has some limitations with it. 
As the the name "Direct Execution" says, the program run directly on the CPU. When the OS wishes to start a program running, it creates an entry for process list, allocates some memory for it, loads the program into memory, sets up the stack, clears the the registers and executes it.
This sounds very simple but offers no control from part of the OS when the program is running. This allows the program to do whatever it wants. Plus this doesn't allow for the OS to controll the process running which doesn't help in the quest for virtualization. 

## Restricted operations
The hardware assists the OS by providing different modes of execution.
In **user mode**, applications do not have full access to hardware resources. 
In **kernell mode**, the OS has access to the full resources of the machine. 

To execute a system call, a program must execute a special **trap** instruction. This instructuion quickly jumps into the kernel, and once in kernel mode you can perform the system call, if allowed. When finished, the OS calls a **return-from-trap** instruction that returns to the user calling program. 