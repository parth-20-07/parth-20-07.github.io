---
id: embedded_fw
title: FreeRTOS Resources
created: 2025-06-27
---

# Beginners Guide

Complete Beginner Guide: [Beginner's guide to FreeRTOS](https://www.freertos.org/Documentation/01-FreeRTOS-quick-start/01-Beginners-guide/00-Overview)
## Basics Terminologies
- **Multitasking**: Running multiple processes/applications alternatively on different threads and and switching between them to perform multiple tasks in a time frame.
![[multi-tasking-diagram.png]]
- **Concurrency**: Running multiple processes/applications alternatively on different threads and switching to others really quick such that it gives an illusion that all are running simultaneously.
![[concurrency.png]]
- **Scheduling**: The **scheduler** is the part of the kernel responsible for deciding which task should be executing at any particular time. The kernel can pause and later resume a task many times during the task's lifetime. If task B replaces task A as the executing task (task A is paused and task B resumed) then task A is said to have been swapped (or switched) out, and task B swapped (or switched) in.
- **Scheduling Policy**: The **scheduling policy** is the algorithm used by the scheduler to decide which task to execute at any point in time. The policy of a (non real-time) multi-user system will most likely allow each task a "fair" proportion of processor time. Swapping can happen only under following conditions:
	- External Stimuli such timers or interrupts
	- Internal API requests such as `yielding`, `sleeping` or `blocking`
	
	![[scheduling.png]]
	Referring to the numbers in the diagram above:
	- At marker (1), task 1 is executing.
	- At marker (2), the kernel swaps task 1 out ...
	- ... and at marker (3), swaps task 2 in.
	- While it is executing, at marker (4), task 2 locks a processor peripheral for its own exclusive access (not visible in the diagram).
	- At marker (5), the kernel swaps task 2 out ...
	- ... and at marker (6), swaps task 3 in.
	- Task 3 tries to access the processor peripheral previously locked by task 2, and finding it locked, blocks at marker (7) to wait for the peripheral to be unlocked.
	- At marker (8), the kernel swaps task 1 in.
	- Etc.
	- At marker (9), task 2 is executing again, finishes with the peripheral, and unlocks it.
	- At marker (10), task 3 is executing again, finds the peripheral accessible, so continues executing until it is swapped out again.

---

# Resources
- [FreeRTOS Website](https://www.freertos.org/)
- [[Mastering-the-FreeRTOS-Real-Time-Kernel.v1.1.0.pdf]]
- [FreeRTOS/Lab-Project-FreeRTOS-Tutorials](https://github.com/FreeRTOS/Lab-Project-FreeRTOS-Tutorials/tree/main)