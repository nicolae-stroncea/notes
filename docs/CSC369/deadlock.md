# Deadlock

## Resources

**Resources**: A resource is an <u>object that a process requires</u>

* hardware: printers, memory, processors, etc

* data: shared variables, files, etc.

* synchronization objects (to access critical regions): locks, semaphores, monitors

## Root causes of a resource deadlock

> Note: think of it like a traffic jam

* Resources are finite

* Processes will  wait if a resource they need is unavailable

* Resources may be held by other waiting processes

## Necessary conditions for a deadlock

1. <font color="red">Mutual exclusion</font>: only one process may use resource at a time.   
   
   > This condition is <u>satisfied in any synchronization environment</u>. It's one of the 3 necessary conditions for synchronization(see Progress section): Mutual exclusion, Progress, bounded waiting(no starvation).

2. <font color="red">Hold and wait</font>: process may hold resources while waiting for other resources to become available. Name is hold and wait because you hold your current resources and waiting for more
   
   > This condition is <u>satisfied in any synchronization environment</u>.

3. <font color="red">No preemption</font>: no resource can be **forcibly removed** from a process holding it.  
   
   > This condition is also <u>satisfied in any synchronization environment</u>. For example  we cannot forcibly take a lock from a thread give it to another.

4. <font color="red">Circular wait</font>: a <u>closed chain</u> of processes exists, such that each process holds at least one resource needed by the next process in the chain. 
   
    !!! bug
   
        This is the main reason why a deadlock occurs. e.g: A->B->C->A   

Together these conditions are necessary and sufficient for a deadlock

## Deadlock prevention

<mark>To prevent a deadlock, we need to break one of the 4 conditions.</mark>

### Break mutual exclusion

* **Method**: use architectural support(hardware) to create lock-free data structures using **non-blocking atomic operation**. 

* **Problem**: fundamental part of synchronization requirements. Very difficult to convert complex operations to a lock-free version,

### Break hold and wait

* **Problems**:
  
  * Process **may wait for a long time** for all resources to become available at the same time.
  
  * Process will try to acquire all locks at the start, instead of an on-going basis, which severly limits concurrency, since now a lot of critical sections are locked off. 
  
  * As a result of previous problem, some processes may hold for some locks for a really long time before they actually use it.
  
  * Process may not know all resources it needs in advance.

### Break no pre-emption

* **Problems**: typically it's a terrible idea to take resources away from another thread. A thread cannot know where another thread currently is, and if you take the lock away whilst it is modifying data, you can corrupt everything.

In conclusion, it's a bad idea to try and break any of the first 3 conditions of a deadlock, since they are also pillars of synchronization which make syncrhonization easier to achieve and less buggy. Therefore <mark>only way left is to prevent circular wait.</mark>

### Preventing circular wait

**Method**: <mark>assign a linear ordering to resources</mark>and require that a process holding a resource can only request resources that that follow its resource R in the ordering. 

!!! example
    process A has lock # 2, and it can only request locks with a higher number.

!!! note
    other strategies for dealing with Deadlocks are: **deadlock avoidance**(use knowledge about resources that each process might request, **deadlock detection**, and **deadlock recovery**

## Ostrich algorithm

Most OS nowadays employ "ostrich algorithm": ignore problem, hope it doesn't happen often.

## Deadlock vs Livelock

> [from wikipedia](https://en.wikibooks.org/wiki/Operating_System_Design/Concurrency/Livelock)

* a livelock is similar to a deadlock, except that the state of the processes involved in livelock constantly change with regard to one another, so no-one progresses. It's a special case of resource starvation.

* Example of livelock is 2 people meeting in a narrow corridor, where each one tries to be polite by moving aside to let the other pass, but they end up swaying from side to side without making any progress because **they always move the same way at the same time**

## Communication deadlocks

* **problem**: Deadlocks that happen when A sends request to B, and waits for reply, B waits for the message, and the *message gets lost*, then both are waiting.
* **solution**: use timeouts, use protocol to detect that message is lost and resend it.

