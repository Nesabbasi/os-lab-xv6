# OS-LAB-xv6
A modified [xv6](https://github.com/mit-pdos/xv6-public) operating system with some extra features.


- [Phase1: Introduction to xv6](#phase1-introduction-to-xv6) 
    - [1. Boot Message](#1-boot-message) 
    - [2. Shell Features](#2-shell-features) 
    - [3. User Program](#3-user-program)
- [Phase2: System Calls](#phase2-system-calls)
- [Phase3: CPU Scheduling](#phase3-cpu-scheduling)
    - [1. Multi-Level Feedback Queue](#1-multi-level-feedback-queue)
    - [2. Aging](#2-aging)
    - [3. System Calls](#3-system-calls)
- [Phase4: Synchronization](#phase4-synchronization)
    - [1. Semaphore Implementation](#1-semaphore-implementation)
    - [2. Dining Philosophers Problem](#2-dining-philosophers)
- [How to use?](#how-to-use)

## Phase1: Introduction to xv6
### 1. Boot Message
Added contributors' names to the system boot message
```text
1. Ava Mirmohammadmahdi
2. Nesa Abbasi
3. Sepehr Azardar
```
### 2. Shell Features
Added following shortcuts to the console: 

- `Ctrl + N` : Removes all the digits from current line

- `Ctrl + R` : Reverses the current line

- `Tab` : Filling out current line with the best option from last 15 used commands

### 3. User Program
`Prime Numbers` user program added to system which finds prime numbers in the given range and puts them in `prime_numbers.txt` file.

```text
prime_numbers 20 50
```

## Phase2: System Calls
* `find_largest_prime_factor` : This system call finds the largest prime factor of a given number. The parameter of this system call is passed in the ebx register.

* `change_file_size` : This system call changes the size of the file by adding zeros to the end of the file or removing the last bytes of the file

* `get_callers` : This system call returns the history of the callers of every system call.

* `get_parent_pid` : This system call returns the parent process id of the current process.

## Phase3: CPU Scheduling
### 1. Multi-Level Feedback Queue

1. Round Robin(RR): Has the highest priority

2. Lottery queue: The chance of each process for being executed is showed with a number.

3. Best Job First(BJF): The process with the lowest rank is executed first.

### 2. Aging
To prevent process starvation, we use the aging algorithm. In this implementation, if after 10000 cycles the processor was not assigned to the process, that process will be transferred to a queue with higher priority, and monitoring the number of unexecuted cycles of that process will start from the beginning.

### 3. System Calls

- `change_scheduling_queue`: Changes the scheduling queue of a process.
```c++
int change_scheduling_queue(int pid, int queue);
```

- `set_bjf_params_process`: Sets the Best Job First parameters for a process.
```c++
int set_bjf_params_process(int pid, float priority_ratio, float arrival_time_ratio, float executed_cycles_ratio);
```

- `set_bjf_params_system`: Sets the Best Job First parameters for a system.
```c++
int set_bjf_params_system(float priority_ratio, float arrival_time_ratio, float executed_cycles_ratio);
```

- `set_lottery_tickets`: Sets the lottery tickets of a process.
```c++
int set_lottery_tickets(int pid, int tickets);
```

- `print_process_info`: Shows the processes information.
```c++
void print_process_info(void);
```

## Phase4: Synchronization
### 1. Semaphore Implementation
Semaphores are designed to facilitate synchronization exclusively among processes, as the operating system does not support threads. The system incorporates an array of 5 semaphores, focusing on the utilization of counting semaphores. This type of semaphore allows a specific number of processes to enter a critical section simultaneously. When the maximum allowable processes in the critical section is reached, the excess processes unable to enter go into sleep mode and are queued in a First-In-First-Out (FIFO) fashion. Upon the departure of a process from the critical section, the highest-priority process in the queue gains access, ensuring efficient resource utilization.
User-level programs can access to semaphore using folowing system calls:
- `sem_init` : Initializes a semaphore.

```c++
int sem_init(int index, int value);
```

- `sem_acquire` : Acquires a semaphore.

```c++
int sem_acquire(int index);
```

- `sem_release` : Releases a semaphore.

```c++
int sem_release(int index);
```

* The simulation of the Dining-Philosophers is implemented. 


### 2. Dining Philosophers

The `dining_philosophers` simulation employs an array of semaphores to address the classic synchronization challenge in computer science. To prevent deadlock, a strategy is implemented where philosophers with even IDs pick up the left fork first, and those with odd IDs pick up the right fork first. The solution utilizes semaphores and functions discussed in part 4.1 to ensure avoidance of deadlocks and resource starvation.


## How to Use?

To set up xv6 using QEMU within a virtual environment on Linux, follow these steps:
1. Navigate to the desired phase of the project.
2. Open the terminal.
3. Execute the following command:

   ```bash
   make qemu-nox
   ```




