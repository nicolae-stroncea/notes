# Processes

**Process**: running program.

* A typical system might be running tens or hundreds of processes at the same time. Doing so makes the
system easy to use, as you never need to be concerned whether the CPU is avilable, you simply run the
program. 

#### How do you provide the illusion of many CPUs?

* There are typically only a few Physical CPUs available, how can the OS provide illusion of nearly-endless
supply of said CPUs?

* **It runs one process, stops it, runs another, and so forth.** OS creates this illusion by
**virtualizing** the CPU. It looks like there are many CPUs when in fact its only one, and were
switching between them at very high speeds.

## Virtualization

* **mechanisms**: low-level protocols and methods that implement some functionality. e.g:context switch(a time sharing mechanism).
* **policies**: more high-level, reside on top of mechanisms. They're algorithms that are responsible for some decisions in the CPU(which program should next run the OS?). A **scheduling policy** in OS will make this decision using:
	* *historical information* (which program has run more over last minute)
	* *workload knowledge* (what types of programs are run)
	* *performance metrics* (is system optimizing for interactive performance or thoroughput).

### Time sharing

* The main purpose of time sharing is to make it look like you have infinite concurrent processes you can use, and to let you create and run as many programs as you want at the same time. The disadvantage is the performance. Each will run more slowly if the CPU must be shared(this is because OS will have to
switch more often). 
* The resource(CPU, network link, etc **(time sharing occurs for many different resources)** ) can be used
for some time by process A, then process B, and so on. 
* Employed by all modern OS. 
* **Context sharing**: gives the OS the ability to stop running one program and start running another on a given CPU.

### Space sharing

* A resource is divided in space among those who want to use it.
* Disk sharing is naturally a shared resource(once a block is assigned to a file, it's normally not assigned
to another file until the user deletes the original file.

## Abstraction: The process

> A process is simply a running program. To understand what a process is, ask ourselves: at any given time, what parts of a machine are important to the execution of a program. 

* Memory: Memory that the process can address(**address space**) is part of the process.
	* Instructions lie in memory.
	* Data the program reads and writes sits in memory.
	* Registers: instructions explicitly read or update registers, so they're important to execution.
		* **Program Counter**: which instruction of program will execute next
		* **Stack Pointer**: manages the stack for function parameters
		* **Frame Pointer**: manages the stack for function parameters, local variables and return addresses.

### Process Creation

* Before running program, OS must load code and static data(initialized variables) into **address space** of the process. 
* Programs reside on disk in some **executable format**. So OS  reads the bytes from disk and places the into its address space. Modern OS's do it lazily(they load pieces of code or data only as they are requested/needed in program execution**(.


