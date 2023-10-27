We must abstract physical memory such that each program has its own [[address space]] which only the program can view. We do this because we don't want programs snooping around in other programs memory. 

The address space contains all the running state of the program such as the code (instructions), the stack to keep track of function call stack, local variables, parameters, return values, etc., and the heap to allocate dynamically created and user managed memory (calls to malloc() and free()). The code is placed at the top of the [[address space]], the stack is placed under the code (grows downward), and the heap is placed at the bottom of the address space (grows upward). This address space layout is just convention and you could arrange it differently if you wanted to (we could see this with threads).

When we talk about address space, we are talking about the virtualization of memory the OS is providing. The running program thinks it's loaded into memory at a particular space in physical address when in reality it is mapped by the OS at a different address. Thus, if process A tries to load data at address 0, the OS will have to make sure the process doesn't actually load address 0, rather another address such as 320kb. 

Virtualization must include 3 properties:
- transparency: the OS should virtualize memory in a way such that the virtualization is invisible to the program. Thus, the program works as if it has access to the actual physical memory.
- efficiency: virtualization should be cost efficient
- protection: other programs should not be able to view each other's memory.