## Exokernel: An Operating System Architecture for Application-Level Resource Management

The original paper can be found [here](https://github.com/parthskansara/Operating-Systems/blob/main/1.%20Exokernel/Exokernels.pdf).

### Table of Contents:
* [Problem](#problem)
* [Previous Work](#previous-work)
* [Contributions](#contributions)
* [Conclusion](#conclusion)
* [Reference](#reference)

### Problem:
* **Limited flexibility**: Fixed interface and implementation of OS abstractions restricts the performance, flexibility and functionality of applications
* **No domain-specific implementation**: Applications pay overhead costs rooting from the trade-offs that are decided by the OS without considering the requirements of the
application
* **Limited view of system information**: Applications cannot apply any customized resource management abstractions as they have a limited view of the system information

### Previous Work:
* **[HYDRA](https://research.cs.wisc.edu/areas/os/Qual/papers/hydra.pdf)**: Separation of kernel policy and mechanism > Exokernel eliminates *mechanism* altogether 
* **[VM/370](https://pages.cs.wisc.edu/~stjones/proj/vm_reading/ibmrd2505M.pdf)**: Virtualization of base-machine > Expensive and complicated, while still not addressing the lack of control for applications
* **Microkernels**: Distribute kernel abstractions over sub-processes called servers, running in user space > Restricts applications from modifying the high-level abstractions
* **[Cache Kernel](https://people.eecs.berkeley.edu/~prabal/resources/osprelim/CD94.pdf)**: Low-level kernel supporting concurrent application-level operating systems > Focuses on reliability and cannot support high concurrency

### Contributions:
The exokernel architecture aims to define a low-level interface, by separating protection from management of resources. For this, it securely exports the resources to the library operating systems. This is done using the following techniques:

#### 1. Secure Bindings:
* Protects individual resources by monitoring ownership and permissions for the resources
* Enforces authorization at *bind time* and implements access checks at *access time*
* Implements this mechanism using:
	* Hardware mechanisms
	* Software caching
	* Downloading application code


#### 2. Visible Resource Revocation:
* Permits library OS to guide deallocation of resources
* Enables library OS to have knowledge about scarce resources
* Allows library OS to save state information of resources or relocate its physical names before they are revoked

#### 3. Abort Protocol:
* Security mechanism to forcefully breaking the secure bindings and freeing up resources
* Revoked resources are stored in a *repossession vector*, which the library OS can read to update its mappings
* State information of the revoked resource can be written to a memory resource
* This memory resource can be specified by the library OS in advance
* To avoid arbitrary revocation of resources, the library OS is guaranteed a set of resources that will not be revoked

The paper offers two software implementations:
* **Aegis**, an exokernel
* **ExOS**, a library operating system

#### 1. Aegis
* Enhanced performance through:
	* Monitoring ownership
	* Maintaining a small, lean kernel
	* Caching secure bindings
	* Downloading packet filters and Dynamic code generation for securely binding the network
* Maintains simplicity of basic primitives, which beats the general primitives of Ultrix in performance
* Superior implementation of exceptions dispatch and control transfer primitives
* Efficient multiplexing of the processor, memory and the network

#### 2. ExOS
* Efficient implementation of IPC, virtual memory and remote communication at the application level
* Creates special purpose implementations of these abstractions, which offers improvements in functionality and performance

### Conclusion
This paper exhibits that by avoiding high-level abstractions, the exokernel is able to provide abundant freedom to the library operating systems, which in turn improves performance. 

Furthermore, the two software implementations - Aegis and ExOS demonstrate the following hypotheses:
* Exokernels are very efficient
* Secure low-level multiplexing of hardware resources can be implemented efficiently
* Traditional OS abstracted can be implemented efficiently at the application-level
* Applications can create optimized special-purpose implementations of these abstractions

### Reference
You can find the summary written by me [here](). This was written as a part of the course [CSE 506 Operating Systems](https://www3.cs.stonybrook.edu/~dongyoon/cse506-s23/) by [Prof. Dongyoon Lee](https://www3.cs.stonybrook.edu/~dongyoon/) at Stony Brook University.
