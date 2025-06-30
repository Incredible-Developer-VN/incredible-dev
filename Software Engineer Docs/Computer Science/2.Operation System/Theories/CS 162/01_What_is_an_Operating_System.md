# CS162 Lecture 1: What is an Operating System?

## Introduction

CS162, "Operating Systems and Systems Programming," is a cornerstone course at the University of California, Berkeley, consistently offered across numerous semesters, including Spring 2024, Fall 2020, and Fall 2015, underscoring its enduring relevance in computer science education. This inaugural lecture, "What is an Operating System?", is fundamental, establishing the intricate relationship between software and hardware.

The primary objectives of this introductory session are to define the essence of an Operating System, differentiate its core functions from other software layers, illustrate key aspects of OS design, articulate the compelling challenges and opportunities within the field, and provide an overview of the CS162 course structure. Although the user's inquiry specifically references John Kubiatowicz, the lecture materials for this particular instance of "Lecture 1: What is an Operating System?" indicate it was delivered by Prof. Anthony D. Joseph on August 24th, 2016. However, the slides acknowledge contributions from a collective of instructors, including John Kubiatowicz and David Culler, suggesting a collaborative and evolving curriculum. This collaborative development approach within a university department ensures that foundational course material, especially for subjects as critical as Operating Systems, is refined and shared among faculty over various semesters, fostering consistency and leveraging collective expertise.

The consistent offering of CS162 over many years  and the foundational nature of its initial lecture  underscore that the core principles of operating systems remain remarkably stable and indispensable, even amidst rapid advancements in hardware and applications. The lecture's emphasis on defining an OS and its fundamental roles highlights that these concepts serve as timeless building blocks for any computing professional. This foundational understanding provides a durable framework for adapting to future technological shifts and innovations.

## I. Defining an Operating System: Roles and Abstractions

At its core, an Operating System (OS) functions as a specialized layer of software that facilitates application software's access to the underlying hardware resources. It acts as a crucial intermediary, presenting a convenient abstraction of complex hardware devices, thereby simplifying their utilization for application developers. A vital aspect of its role is to provide protected access to shared resources and to ensure the overall security and authentication within the system. This relationship can be conceptualized as a hierarchical flow: Application interacts with the OS, which then interacts with the Hardware.

The multifaceted nature of an OS can be understood through three primary conceptual roles:

- **The OS as an "Illusionist":** In this capacity, the OS crafts clean, user-friendly abstractions of physical resources, creating the perception of limitless memory or a dedicated machine for each running program. It adeptly manages and conceals the inherent limitations of hardware, presenting higher-level constructs such as files, user accounts, and inter-process messages. This virtualization significantly streamlines programming efforts and enhances the overall user experience by simplifying interactions with complex underlying systems.
- **The OS as a "Referee":** This role involves the OS meticulously managing the sharing of resources among numerous competing applications. It rigorously enforces protection and isolation mechanisms, preventing any single program from disrupting or corrupting other applications or the OS itself. The OS ensures fair and secure allocation of resources, safeguarding direct access to storage through mechanisms such as segmentation faults.
- **The OS as "Glue":** Functioning as "glue," the OS provides essential common services that seamlessly connect various components of a computer system. These services encompass storage management, interfaces for windowing systems, user interface (UI) functionalities, and networking capabilities. This connective role facilitates sharing, authorization, and maintains a consistent "look and feel" across disparate applications.

The descriptions of the OS as an "Illusionist," "Referee," and "Glue" collectively portray the OS not merely as a software component, but as the central control plane that orchestrates and manages inherently distributed and shared hardware resources. The "Illusionist" role effectively hides physical distribution and limitations, the "Referee" role meticulously manages contention and enforces boundaries, and the "Glue" role provides unified access across the system. This extends beyond a simple enumeration of functions; it highlights the architectural philosophy that underpins the feasibility of complex multi-user, multi-tasking computing environments.

The roles of "Illusionist," which prioritizes convenience and abstraction, and "Referee," which emphasizes protection and security, inherently involve design trade-offs. Providing a highly convenient abstraction might, at times, introduce conflicts with stringent protection requirements, or vice-versa. For instance, robust isolation mechanisms enforced by the "Referee" often incur some performance overhead, potentially impacting the "Illusionist's" aim for a seamless user experience. This inherent tension suggests that OS design is a continuous balancing act, navigating competing objectives such as performance, security, usability, and resource utilization.

To further clarify these roles, the following table summarizes the key functions and responsibilities of an Operating System:

| Role | Primary Function/Goal | Key Responsibilities/Examples |
| --- | --- | --- |
| Illusionist | Abstraction | Virtual memory, virtualized devices, simplified programming interfaces |
| Referee | Resource Management | Process isolation, CPU scheduling, memory protection, security |
| Glue | Interoperability/Services | File systems, networking, windowing systems, shared libraries |

Export to Sheets

## II. Core Functions and Components of an Operating System

An operating system is responsible for critical management functions that enable the efficient and secure operation of a computer system. These essential functions include:

- **Memory Management:** This involves the allocation and deallocation of memory to various processes, managing virtual memory, and ensuring robust memory protection to prevent unauthorized access or corruption.
- **I/O Management:** The OS handles input and output operations for a wide array of devices, providing a consistent and abstracted interface for applications to interact with hardware, regardless of the specific device type.
- **CPU Scheduling:** This crucial function determines which process or thread gains access to the Central Processing Unit (CPU) at any given moment. The goal is to optimize for various metrics, including overall system performance, fairness among competing processes, and responsiveness to user interactions.

Central to the OS's operational framework is the **Process Abstraction**. A process is defined as the fundamental unit of execution within an operating system, characterized as an execution environment endowed with restricted rights. This means a process is inherently protected from interference by other processes and is prevented from directly harming the OS itself. Typically, each running program operates within its own distinct process, which provides a more manageable and user-friendly interface for execution management.

A process encapsulates all the necessary resources required for a program to execute. Its key components include:

- **Address Space:** This refers to a distinct memory region allocated exclusively to the process, which it perceives as its own private memory. This provides a crucial layer of memory isolation, preventing one process from inadvertently or maliciously accessing the memory of another.
- **One or more Threads:** A thread represents a single, unique context or unit of concurrency for execution within a process. Importantly, multiple threads can coexist within a single process, sharing its address space, which enables parallel execution within the same program context.
- **Open Files:** A process maintains a list of files that it currently has open for reading, writing, or other operations, facilitating data persistence and access.
- **Open Sockets:** These represent network connections that the process has established for communication with other processes, either on the same machine or across a network.

The definition of a process as an "execution environment with restricted rights" , along with its constituent components like address space, threads, open files, and sockets, reveals its role as the OS's primary abstraction for achieving both isolation and resource allocation. The OS manages these processes as distinct, protected entities, which is fundamental for ensuring system stability and enabling multi-tasking capabilities.

The concept of a process containing "one or more threads"  introduces the notion of concurrency within a protected environment. While threads primarily encapsulate concurrency, address spaces are designed to encapsulate protection. This highlights a sophisticated design principle: the OS provides a protected container (the process) within which concurrent execution (via threads) can occur safely. This design allows for efficient utilization of CPU resources by enabling multiple tasks to progress seemingly simultaneously, while rigorously maintaining the integrity and isolation of individual programs. The "OS as Referee" role  is directly empowered by this process-based protection mechanism, ensuring that concurrent operations do not compromise system security or stability.

## III. Foundational OS Tools and Concepts

### Dual-Mode Operation: The Basis for Protection

To enforce privilege levels and safeguard the operating system from user programs, hardware provides at least two distinct modes of operation. This fundamental mechanism is known as dual-mode operation:

- **Kernel Mode ("Supervisor Mode"):** This is a highly privileged mode in which the operating system kernel executes. In kernel mode, the OS possesses unrestricted access to all hardware resources and is authorized to execute any instruction. This level of privilege is essential for managing critical system functions and maintaining overall system integrity.
- **User Mode:** This is a less privileged mode where user applications and most system utilities run. In user mode, certain sensitive operations, such as direct hardware manipulation or modification of system registers, are explicitly prohibited. This restriction is crucial for maintaining system integrity and security, preventing rogue applications from compromising the entire system.

Transitions between user mode and kernel mode are meticulously controlled and can only occur through specific, predefined mechanisms. These typically include:

- **System Calls (Syscalls):** A process explicitly requests a service from the operating system, such as performing file I/O, allocating memory, or creating/terminating other processes. These requests act as controlled entry points into the kernel.
- **Interrupts:** These are hardware-generated events, such as the completion of an I/O operation or a timer expiration, which cause an immediate transfer of control to the OS.
- **Exceptions/Traps:** These are software-generated events, such as a division by zero error or an invalid memory access (e.g., a segmentation fault), which also transfer control to the OS for handling.

This dual-mode operation is fundamental to the OS's role as a "Referee" , providing the essential isolation and protection necessary for stable multi-user and multi-tasking environments.

The following table provides a clear comparison of Kernel Mode and User Mode, highlighting their differences and respective roles in maintaining system integrity:

| Feature | Kernel Mode ("Supervisor Mode") | User Mode |
| --- | --- | --- |
| Privilege Level | Highly Privileged | Less Privileged |
| Access to Hardware | Full access to all hardware | Restricted access to hardware |
| Permitted Operations | All instructions, including privileged ones | Limited set of instructions, no direct hardware access |
| Typical Use | Operating System kernel | User applications, most system utilities |

Export to Sheets

### Virtual Machine Abstraction

The operating system employs virtual machine abstraction as a sophisticated software engineering solution to effectively manage and "tame complexity" within a computing system. This abstraction transforms the idiosyncratic and often complex behaviors of hardware and low-level software into the convenient and standardized interfaces that programmers require. This design strategy is optimized for convenience, efficient resource utilization, robust security, and overall system reliability. Across various OS domains, such as file systems, virtual memory, networking, and scheduling, there exists a fundamental distinction between the raw hardware interface (representing physical reality) and the polished application interface (presenting a more abstract and user-friendly view).

A virtual machine (VM) is fundamentally a software emulation of an abstract machine, designed to give programs the illusion that they possess exclusive ownership of the entire underlying hardware. This mechanism makes the hardware appear to possess desired features, even if they are not directly present in the physical components.

There are two primary types of virtual machines:

- **Process Virtual Machines:**
    - This type of VM supports the execution of a single program.
    - Its functionality is typically provided by the operating system itself, creating an isolated environment for each running application.
    - **Benefits:**
        - **Programming Simplicity:** Each process operates under the illusion that it has exclusive access to all memory and CPU time, and that it controls all devices. Furthermore, disparate devices appear to present a consistent, high-level interface to the application.
        - **Fault Isolation:** Processes are isolated from one another, meaning that a bug or crash in one program cannot directly impact or crash other processes or the entire machine.
        - **Protection and Portability:** Examples like the Java Virtual Machine (JVM) exemplify how process VMs provide a secure and stable execution environment that is portable across various underlying hardware platforms.
- **System Virtual Machines:**
    - This more comprehensive type of VM supports the execution of an entire operating system along with its applications.
    - Prominent examples include VMWare Fusion, VirtualBox, Parallels Desktop, and Xen.
    - **Use Cases:**
        - System VMs are particularly useful for operating system development, as any crash within the developing OS is confined to its virtual machine, preventing system-wide instability.
        - They greatly facilitate the testing of programs across different operating systems without requiring multiple physical machines.
        - A hypervisor, which is the software layer that manages system VMs, effectively exports a virtual machine, creating an environment where other virtual machines can be hosted and run.

The explicit statement that the OS's "Virtual Machine Abstraction" is a tool to "tame complexity"  highlights a fundamental design strategy. This goes beyond merely creating virtual environments; it represents a core philosophical approach where the OS intentionally obscures the intricate, diverse, and often idiosyncratic details of hardware, presenting a simplified, consistent, and idealized interface to applications. This strategic use of abstraction is the cornerstone that enables scalability and usability in the face of ever-increasing underlying hardware complexity.

The distinction between "Process VMs" and "System VMs"  reveals that virtualization is not a singular concept but rather a spectrum of application, implemented at different layers of the computing stack. Process VMs, such as the environment provided by a typical OS for applications, have been foundational for decades. In contrast, System VMs, managed by hypervisors, have gained immense prominence with the advent of cloud computing and server consolidation. This evolution demonstrates how core OS principles are adapted and extended to address new computing paradigms and challenges, making virtualization a continuously relevant and evolving area of study.

The following table differentiates between the two main types of virtual machines, clarifying their scope, typical implementers, and primary benefits:

| Type | Scope | Typical Implementer | Primary Benefits | Examples |
| --- | --- | --- | --- | --- |
| Process Virtual Machine | Single program | OS kernel | Programming simplicity, fault isolation, protection, portability | OS process environment, Java Virtual Machine (JVM) |
| System Virtual Machine | Entire OS | Hypervisor | OS development, testing programs on other OSs, server consolidation | VMWare, VirtualBox, Parallels Desktop, Xen |

Export to Sheets

## IV. Challenges and Evolutionary Trends in Operating Systems

### Managing System Complexity: The Primary Challenge

The foremost challenge confronting operating systems is the inherent and escalating complexity of modern computing environments. Contemporary applications are composed of a diverse array of software modules, are designed to run on a multitude of devices with varying hardware architectures, and must coexist and compete with numerous other applications. Furthermore, these intricate systems must also be resilient to unexpected failures and capable of withstanding a growing variety of cyberattacks. Given this complexity, it is simply not feasible to comprehensively test software for all conceivable environments and combinations, leading to a focus on the severity of bugs rather than their mere presence. The OS actively works to mitigate software and hardware quirks by managing and contending with this pervasive complexity.

### Hardware EvolItsution and its Impact

The landscape of hardware development profoundly influences OS design, presenting both opportunities and significant challenges.

- **Moore's Law and Its Shifting Trajectory:** For decades, Moore's Law accurately predicted that the transistor density on semiconductor chips would approximately double every 18 months, leading to a consistent increase in the power and miniaturization of microprocessors. However, the lecture notes indicate a significant turning point, stating that Moore's Law "officially ended" in February 2016. This signifies that the historical rate of doubling transistors per chip every 18-24 months is no longer being consistently achieved. In response, vendors are now increasingly adopting strategies like 3D stacked chips, which involve layering more components within existing geometries to achieve greater density.
- **Power Density Considerations:** The relentless scaling predicted by Moore's Law also implied that power density could reach "amazing levels," posing significant thermal management challenges. Conversely, the importance of battery life, particularly for the burgeoning mobile device market, has become paramount. Despite the challenges, the principle behind Moore's Law can still yield benefits by enabling more functionality to be packed into devices with equivalent or even reduced total energy consumption.
- **The Rise of ManyCore Chips and the Imperative for Parallelism:** A profound "sea change in chip design" is the widespread adoption of multiple "cores" or processors integrated onto a single chip. Examples cited include Intel's 80-core multicore chip (February 2007) and the Single-Chip Cloud Computer (August 2010), which feature numerous processors per chip, potentially reaching 64 or 128 cores. The critical challenge arising from this architectural shift is "How to program these?" , necessitating that parallelism be effectively exploited at all levels of software, from applications down to the OS.

The lecture explicitly highlights the "slowdown in Joy's law of Performance" and the "end of Moore's Law" , directly linking these phenomena to the emergence of "ManyCore Chips". This sequence of events signals a fundamental transformation in how computing performance is achieved. Historically, performance gains were primarily driven by increasing clock speeds, a concept known as frequency scaling. Now, constrained by physical limits and power dissipation issues, the emphasis has decisively shifted towards parallelism, leveraging multiple cores to execute tasks concurrently. This shift has profound implications for OS design, demanding innovative approaches to scheduling, concurrency management, and resource allocation to effectively harness these multi-core architectures. The operating system must now actively enable and manage parallelism, rather than merely benefiting from implicit hardware improvements.

### Internet Scale and Societal Information Systems

The internet has experienced explosive growth, evolving from the foundational ARPANet in 1969 to connecting billions of users globally today. This expansion is further amplified by the fact that smartphone shipments now surpass PC shipments, indicating a significant shift in the dominant computing device. The world is increasingly transforming into a "massive cluster" or an "Internet of Things," characterized by microprocessors embedded in virtually every object and supported by vast, interconnected infrastructure.

This pervasive computing environment necessitates the development of scalable, reliable, and secure services. Such systems involve complex interactions between clusters of servers, extensive internet connectivity, large databases, sophisticated information collection mechanisms, remote storage solutions, and various networked applications like online games and e-commerce platforms. A simple search query, for instance, exemplifies the intricate interplay of multiple components spanning various administrative domains, including DNS servers, data centers, search indexes, page stores, load balancers, and ad servers, all working in concert to create a result page.

The discussion of "Internet Scale"  and the concept of a "Massive Cluster"  illustrates that computing is no longer confined to isolated personal computers but is increasingly distributed and embedded throughout our environment. The operating system, by providing essential abstractions such as files, sockets, and processes, and by efficiently managing resources, becomes the critical enabler for this ubiquitous computing paradigm. Without robust operating systems capable of orchestrating complex interactions across vast networks and diverse devices, the vision of a connected world with billions of hosts and smart devices would remain unrealized. This highlights the OS's indispensable role in facilitating the emergence of societal-scale information systems.

## V. Why Study Operating Systems? (Motivation for CS162)

The study of operating systems, particularly through a course like CS162, offers profound relevance for students pursuing careers in computer science and engineering. The motivations for delving into this field are multifaceted, addressing various career paths and skill development needs:

- **Relevance for Designing and Building OS Components:** A segment of students enrolled in CS162 will directly contribute to the design and construction of operating systems or their constituent components. This area remains highly dynamic, with ongoing developments in specialized, embedded, and cloud-native operating systems. The course provides the foundational knowledge and practical skills necessary for such specialized roles.
- **Importance for Creating Systems Utilizing Core OS Concepts:** A larger proportion of students will develop systems that extensively leverage the core concepts inherent in operating systems, irrespective of whether they are building software or hardware. The principles and design patterns taught in CS162, such as concurrency, memory management, and resource allocation, are pervasive and manifest across numerous levels of system design, from application frameworks to distributed computing architectures.
- **Benefits for Building Applications that Effectively Leverage OS Design and Implementation:** All students will, at some point, build applications that run on and interact with operating systems. A thorough understanding of OS design and implementation empowers developers to create more efficient, robust, and performant applications by enabling them to make optimal use of the underlying operating system's capabilities. This includes optimizing for performance, effectively managing concurrency, and ensuring application stability and security.

The course structure of CS162 is designed to foster a deep, practical understanding through a "Learning by Doing" approach. This involves individual homework assignments focused on systems programming, such as developing a simple shell, a web server, or a memory allocator. Additionally, students engage in three significant group projects using Pintos in C, covering critical OS areas like threads and scheduling, user-programs, and file systems. These group projects are structured to simulate an "Industrial Environment," emphasizing crucial professional skills such as effective communication within teams, meticulous project planning, and the creation of detailed design documentation. The course infrastructure includes a dedicated website, Piazza for collaborative discussions, and Webcast for lecture access. The primary textbook for the course is "Operating Systems: Principles and Practice" by Anderson and Dahlin.

The "Why take CS162?" section  clearly articulates the practical utility of the course, indicating that students will either actively build OS components or proficiently utilize OS concepts in other system designs. This directly aligns with the "Learning by Doing" methodology , which incorporates hands-on homeworks and the Pintos group projects. This pedagogical approach reflects a strong belief in CS162: it is not merely about theoretical comprehension but about equipping students with the practical skills and mindset essential for real-world systems engineering. The course design underscores the conviction that a profound understanding emerges from active construction and problem-solving.

The emphasis on "systems programming" , the structuring of "group projects simulating industrial environment" , and the imperative to "exploit parallelism at all levels"  in the context of "ManyCore Chips" collectively suggest that the course aims to cultivate a specific, highly relevant skillset. This skillset extends beyond conventional programming to encompass a deep understanding of low-level system interactions, concurrent programming paradigms, and collaborative software development practices. This reflects the evolving demands of the technology industry, where system-level thinking and the ability to manage complexity in distributed and parallel computing environments are increasingly paramount.

## Conclusion

Operating systems stand as indispensable layers of software, fundamentally responsible for abstracting, managing, and protecting hardware resources. They enable the execution of complex applications and facilitate multi-user environments by acting as the "Illusionist," the "Referee," and the "Glue," thereby effectively taming the inherent complexity of computing systems. Core OS concepts such as processes, threads, dual-mode operation, and virtual machines are foundational to modern system design, collectively ensuring efficiency, security, and reliability across all levels of computing.

The field of operating systems continues to evolve at a rapid pace, driven by significant shifts in hardware architecture, including the observed plateauing of traditional Moore's Law and the widespread adoption of ManyCore architectures. Concurrently, the increasing scale and distribution of computing, exemplified by the growth of cloud computing and the Internet of Things, present new challenges. Future directions for OS design will heavily involve optimizing for pervasive parallelism, meticulously managing power consumption across diverse devices, ensuring robust security in highly interconnected environments, and providing sophisticated, adaptable abstractions for an ever-expanding array of hardware. The principles and practical skills acquired in CS162 provide the essential framework for understanding and addressing these ongoing and emerging challenges in the dynamic landscape of computing.

---