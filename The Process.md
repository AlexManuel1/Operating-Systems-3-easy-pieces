A process is simply a running program (set of instructions). The OS uses CPU virtualization to run multiple processes at once. Using [[context switching]], the OS switches between these processes allowing them to execute instructions from where they left off. Also, each process has a reference to it's own block of memory.

## Process API
OS's have an API to interact with processes. Usually these APIs do the following:
- create
- destroy
- wait
- show status of process
- some miscellaneous control over a process (e.g. suspend)

# Summary of how a process begins
OS loads the program into memory (lazy loading on modern OS's; loads pieces of the code), [[memory]] is allocated for the run time [[stack]] and [[heap]]. The heap is small at first but as more memory is requested (via [[malloc]]), more memory is given to the heap and freed later (should be freed). The OS will also do other tasks such as opening I/O [[file descriptor]] ([[stdout]], [[stdin]], [[stderr]]). The OS then begins running the program at main() by transferring the process to the CPU.

# Process has three main states 
(more if we're getting specific but these are the main ones):
- running 
- ready
- blocked (usually because of I/O operation, then becomes unblocked and sent to ready state after finishing some event)

# Process Data Structure
Each process has a data structure that contains information about it. For instance, the below code is an example of what the data structure could look like in an OS:

```
// the registers xv6 will save and restore 
// to stop and subsequently restart a process struct context { 
	int eip; 
	int esp; 
	int ebx; 
	int ecx; 
	int edx; 
	int esi; 
	int edi; 
	int ebp; 
}; 

// the different states a process can be in 
enum proc_state { UNUSED, EMBRYO, SLEEPING, RUNNABLE, RUNNING, ZOMBIE }; 

// the information xv6 tracks about each process // including its register context and state 
struct proc { 
	char *mem; // Start of process memory 
	uint sz; // Size of process memory 
	char *kstack; // Bottom of kernel stack for this process 
	enum proc_state state; // Process state 
	int pid; // Process ID 
	struct proc *parent; // Parent process 
	void *chan; // If !zero, sleeping on chan 
	int killed; // If !zero, has been killed 
	struct file *ofile[NOFILE]; // Open files 
	struct inode *cwd; // Current directory 
	struct context context; // Switch here to run process 
	struct trapframe *tf; // Trap frame for the // current interrupt };
```

The register values are saved within the process data structure for the next time the process is loaded into the CPU to execute instructions. This data structure has many names:
- [[process list]]
- [[task list]]
- [[Process Control Block (PCB)]]
- [[process descriptor]]





