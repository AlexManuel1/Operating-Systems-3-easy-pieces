The scheduler must enforce [[scheduling policies]] such that it decides which process to run at a given [[context switch]]. 

Some of these policies include:
- FIFO
- [[SJF]] (Shortest job first)
- [[STCF]] (Shortest time to completion first)
- [[Round Robin]] (run job for a certain amount of time). For this, we must have an efficient time slice as we have to take context switching into consideration

For I/O here is the lifecycle:
- process requests I/O, a ready process replaces the the current process in the CPU, After I/O completes, the hardware sends an interrupt, OS processes the interrupt and moves that process back into the ready state for scheduling. 

# [[Multi-Level Feedback Queue]]
The most well known approaches to scheduling. It tries to optimize turn around time as well as make a system more responsive (give other processes a chance to execute, minimizing response time).  

The MLFQ has different queues based on priority. A job that is ready to run is on a single queue and multiple jobs could also be on that queue. Jobs will run based on priority and if two or more jobs have the same priority, they will be executed Round Robin. MLFQ will use process history to estimate it's priority. 
- Processes with more [[system calls]] will maintain a high priority as they share the CPU more. 
- Processes that hog the CPU will be given a lower priority.
Now we must create rules to prevent starvation and a single process from hogging the CPU by taking advantage of the algorithm: 
- Rule 1: If Priority(A) > Priority(B), A runs (B doesn’t). 
- Rule 2: If Priority(A) = Priority(B), A & B run in round-robin fashion using the time slice (quantum length) of the given queue. 
- Rule 3: When a job enters the system, it is placed at the highest priority (the topmost queue). 
- Rule 4: Once a job uses up its time allotment at a given level (regardless of how many times it has given up the CPU), its priority is reduced (i.e., it moves down one queue). 
- Rule 5: After some time period S, move all the jobs in the system to the topmost queue.

# Proportional share scheduling

- lottery scheduling
- stride scheduling
- Completely Fair Scheduler of Linux

