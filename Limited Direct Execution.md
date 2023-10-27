Basically just a fancy way of saying processes are limited by the OS in how they interact with the CPU. The OS is always the middleman communicating between the CPU and the process. 

For a program to run as fast as possible, they would directly use the CPU. Thus, the OS will create an entry for the [[process list]], allocate memory for the program, load program into memory, setup stack with argc/argv, clear registers, and execute call to main. Once the program finishes, the OS will free the memory of the process and remove the process from the [[process list]]. This concept is known as direct execution and is not good for two reasons:
- restrict permissions for processes: I/O and other operations. 
- Switching between processes

# Restrict permissions for processes through traps

Thus, we have two processor modes: [[user mode]] and [[kenel mode]]; where user mode is restricted while kernel mode can do what it likes. In order for a process to execute restricted instructions (I/O), the process must perform a [[system call]] (e.g. fork(), kill(), exec(), etc). Executing a system call: OS has to execute a special instruction called a [[trap]] instruction (**traps are usually generated automatically by the CPU when a program encounters an error or attempts to execute a privileged instruction**), goes into kernel mode, perform special instruction, OS calls special [[return-from-trap]] instruction to go back into user mode, continues executing program (this process repeats).  The kernel must control what code executes upon a trap, thus it sets up a [[trap table]] at boot time (The trap table usually has the addresses of the syscall handler and the timer handler)

##### LDE (Limited Direct Execution)
Quoted from book: 
```
There are two phases in the limited direct execution (LDE) protocol. In the first (at boot time), the kernel initializes the trap table, and the CPU remembers its location for subsequent use. The kernel does so via a privileged instruction.

In the second (when running a process), the kernel sets up a few things (e.g., allocating a node on the process list, allocating memory) before using a return-from-trap instruction to start the execution of the process; this switches the CPU to user mode and begins running the process. When the process wishes to issue a system call, it traps back into the OS, which handles it and once again returns control via a return-from-trap to the process. The process then completes its work, and returns from main(); this usually will return into some stub code which will properly exit the program (say, by calling the exit() system call, which traps into the OS). At this point, the OS cleans up and we are done.
```

# Switching between processes

If a process is running on the CPU, by definition, the OS is not running on the CPU. If the OS is not running, how can we control anything?

Thus there are two approaches for a Process to give control back to the OS:
- Waiting for system calls and traps (cooperative approach): It is assumed that [[processes]] will be friendly and transfer over control back to the OS when executing system calls (I/O, creating new process, sending network request, etc.). They can also transfer control to the OS by doing something bad like diving by 0, which in turn generates a [[trap]] and transfers control to the OS. However, if we run an infinite loop that never gives control back to the OS, our computer would freeze and we'd have no choice but to reboot. 
- [[timer interrupt]]: a timer is programmed to trigger a timer interrupt which in turn will call the [[interrupt handler]] in the OS. The interrupt handler is essentially code that allows the OS to do as it pleases with its spotlight in the CPU (stop current process, start another process, etc).  

# Saving and restoring processes
The [[scheduler]] decides whether to continue running the current process or switch to a different process. If it switches, this is known as [[context switching]]. How does context switching work? When a process is done executing, the OS saves the process register state into the PCB (process control block) which is within the [[kernel stack]]. It then loads the register state of another process into the CPU. Thus, when the [[return-from-trap]] instruction is executed, the OS continues the execution of another process within the CPU. There are more things loaded into the CPU but the registers are a large majority, so if you want to know more, look into it. 

# sneak peak into concurrency
What happens when during a system call, a timer interrupt occurs? Some OS's handle this by [[disabling interrupts]] while an interrupt handler is executing. OS's have many locking schemes to prevent concurrency issues. 