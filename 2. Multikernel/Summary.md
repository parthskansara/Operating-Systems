## The Multikernel : A New OS Architecture for Scalable Multicore Systems

The original paper can be found [here](https://github.com/parthskansara/Operating-Systems/blob/main/2.%20Multikernel/Multikernels.pdf).

### Table of Contents:
* [Problem](#problem)
* [Previous Work](#previous-work)
* [Contributions](#contributions)
* [Conclusion](#conclusion)
* [Reference](#reference)

### Problem:
* **Lack of Scalability**: The previous operating systems were unable to scale with the advancements in the hardware
* **Not Portable**: OS was not portable across diverse hardware - both across machines and within machines (in terms of diverse core architectures)
* **Overhead of using Shared Memory approach**: Cores waste 1000s of cycles in updating data structures using shared memory compared to message passing

### Previous Work:
* **[Auspex](http://www.bitsavers.org/pdf/auspex/eng-doc/848_Architecture_SE_Training.pdf) & [IBM System/360](https://www.ibm.com/ibm/history/ibm100/us/en/icons/system360/)**: Distributed system-like OS > Multikernel improves parallelism, deals with diversity of machines
* **[Tornado & K42](https://www.usenix.org/legacy/events/osdi99/full_papers/gamsa/gamsa.pdf)**: Introduced clustered objects, using partitioning and replication > Based on shared memory itself
* **Microkernel**: Employs message-passing > Uses shared memory kernel
* **[Distributed OS](https://dl.acm.org/doi/pdf/10.1145/6041.6074)**: Uniform OS over a network of independent computers > Multikernel applies similar principle to a collection of cores

### Contributions:
The multikernel architecture propose a new OS that treats the machine as a network of cores, communicating using message passing. It is based on the 3 fundamental principles:

#### 1. Explicit Communication among cores
* No shared memory except messaging channels
* Exposes information about shared state
* Allows OS to use common networking optimizations like pipelining and batching
* Enables separation of requests from responses, and permits the core to be productive while waiting for a response
* Communication through well-defined interfaces allows a scope for refining and tolerance to fault

#### 2. Hardware-independent Design
* Adapting to new hardware becomes less cumbersome
* Communication algorithms can run independently of the underlying hardware implementation

#### 3. State Replication over State Sharing
* Reduces load on system interconnect
* Reduces dispute for memory
* Reduces the hassle of synchronization
* **Suggested optimization** > Allow limited sharing between a set of closely-related cores or threads

The paper offers a multikernel OS implementation based on these design principles called **Barrelfish**.

#### Barrelfish
* Separates the CPU driver from the monitor components

  a. CPU driver
  * Runs in privileged mode
  * No state sharing policy allows it to be event-driven, single threaded and non-preemptable
  * Kernel is small and easy to develop

  b. Monitor components
  * Runs in distinguished user-mode
  * Independent of the processor
  * Schedulable

* Also uses **System Knowledge Base (SKB)** to maintain knowledge of the underlying hardware, which can be utilized for optimization

### Conclusion
This paper exhibits that by treating a multi-core system like a network of cores, and avoiding shared memory, the operating system can be made scalable across diverse hardware.

The multikernel implementation by the authors, Barrelfish, is able to meet the following goals to a certain extent:
* Has similar performance to existing standard operating systems on modern multi-core hardware
* Shows the potential to scale effectively to large numbers of cores, especially under demanding conditions that put pressure on global OS data structures
* Can be adapted to different hardware or use alternative methods for sharing without the need for significant modification
* Makes use of message passing to improve performance through pipelining and batching
* Utilizes the modular design of the OS to optimize placement of OS features based on hardware configuration and usage

### Reference
You can find the summary written by me [here](https://github.com/parthskansara/Operating-Systems/blob/main/2.%20Multikernel/Multikernels%20Summary%20Assignment.pdf). This was written as a part of the course [CSE 506 Operating Systems](https://www3.cs.stonybrook.edu/~dongyoon/cse506-s23/) by [Prof. Dongyoon Lee](https://www3.cs.stonybrook.edu/~dongyoon/) at Stony Brook University.
