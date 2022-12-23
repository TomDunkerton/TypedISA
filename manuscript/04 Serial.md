# High Performance Serial Processing

One of the ways to speed up scientific computation is by using many processors, working in parallel, each working on a part of the problem. The kinds of problems that are best served by a  *huge* degree of [Data parallelism](https://en.wikipedia.org/wiki/Data_parallelism) are called *“embarrassingly parallel”* problems. Usually, this involves very large data arrays which are divided up among many processors all running the same program. However, most real-world problems have sections of code that **must** be computed in serial.  

According to Amdahl's law, **“the speedup of a program using multiple processors in parallel computing is limited by the time needed for the *sequential* fraction of the program.”**

A somewhat humorous illustration of this principle asks the question "if it takes nine months for one woman to have a baby, can nine women have a baby in one month?" The answer of course is no! Some tasks simply can't be shared no matter how many willing volunteers you may have!

The same is true with real life problems we try to solve on a computer. There are some things that *must* be done in sequence because the results of the next operation depend on what has been done before. We call this "Serial" or "Sequential" programming. Each task must be completed before the next one can begin. But some very smart hardware designers have figured out ways to speed up sequential programs also, as we shall soon see.

While optimizing the parallel portion of a program through parallel processors is profitable, any acceleration of the sequential portions of code does even more to the speed up the overall program. In real life scientific applications, sequential code is needed to calculate data elements which will later be calculated in parallel using Vector operations. Any acceleration of the sequential part of the program yields significant acceleration of the overall program.

The following illustration and explanation is taken from the Wikipedia article on [Amdahl's Law](https://en.wikipedia.org/wiki/Amdahl%27s_law):

![Optimizing Different Parts](Resources/Speeding up Serial.jpg)

A>   Assume that a task has two independent parts, A and B. 
A>   Part B takes roughly 25% of the time of the whole computation. 
A>   By working very hard, one may be able to make this part 5 times faster, 
A>   but this reduces the time of the whole computation only slightly. 
A>   In contrast, one may need to perform less work to make part A perform twice as fast. 
A>   This will make the computation much faster than by optimizing part B, 
A>   even though part B's speedup is greater in terms of the ratio, (5 times versus 2 times)

## Instruction Level Parallelism (ILP)

To provide the best computation speeds possible for the sections of code that must be run in serial, we must find individual instructions that can be run in parallel. This is called [Instruction Level Parallelism (ILP)](https://en.wikipedia.org/wiki/Instruction-level_parallelism).

ILP facilitates the acceleration of a *single thread* of computation and takes place on a *single processor*. This is not the same as Data Level Parallelism (DLP) in which a large data set is divided up and the computation is spread over *many* processors.  

The amount of ILP in any given application is determined by the algorithm. The key concept needed to understand ILP is Data Dependency. In short, if a calculation needs the result of a previous calculation, the instruction will not be dispatched until all its data items are available. In the meantime, other instructions which have received all the necessary data items will continue to be dispatched. For example:

Consider the following program:

~~~~~~~~~~~~~~~~
    1   a = x * y
    2   b = x / y
    3   c = x + y
    4   d = x - y
    5   e = a * b 
    6   f = c * d    
~~~~~~~~~~~~~~~~

In this example, operations 1, 2, 3 and 4 can all be performed in parallel. This is because the input operands x and y do not depend on other operations. Then operations 5 and 6 will be performed in parallel. While the instructions may be dispatched at the same time, they do not take the same amount of time to complete. 

Addition and subtraction take fewer cycles to complete than multiplication and division. Because variable b is the result of the division (x / y), operation 5 will have to wait until "b" is available. In the meantime operation 6 can proceed before operation 5 because variables c and d are the result of addition and subtraction, which are much faster.

There are several ways to exploit ILP and speed up serial code which we will examine below. RISC++ is a hybrid system that uses a combination of techniques used by other theoretical and actual processors. RISC++ is designed to produce fast serial code with a simple instruction set that occupies a small foot print in memory. 

The architecture defines two processor types; one for scalar instructions and data and the other for processing variable length vectors. The scalar part is optimized for ILP and the vector part is optimized for DLP.

## Dataflow Architecture

There has been much research for increasing ILP using [Dataflow Architecture](https://en.wikipedia.org/wiki/Dataflow_architecture). This family of designs has had a variety of proposed implementations, implemented in both hardware and software. The basic concept is to execute instructions in the *order that data arrives at the input* to the instruction rather than in the order specified by an Instruction Pointer. While I am not aware of any general-purpose computers using a strict data flow design at the ISA level, the concepts have been used in a variety of applications. 

At the software level much research has been done in [Dataflow Programming](https://en.wikipedia.org/wiki/Dataflow_programming). Some of this has involved new programming languages and new programming paradigms. But the marketplace continues to demand solutions that allow existing software to run faster on new hardware without being completely re-written. Current microprocessors use a data flow architecture under the covers as we see below.

## Very Long Instruction Word (VLIW)

The complexities and disadvantages of OoOE led Intel to invest a considerable amount of time and money in an ISA called [IA-64](https://en.wikipedia.org/wiki/IA-64) also known as Itanium. This processor family was designed to solve the problems created by the complex hardware needed to support X86 and OoOE. The job of reordering instructions to support ILP was moved from the hardware to the Compiler. Highly sophisticated compilers would examine the entire program’s source code and determine which instructions could be executed in parallel. Then the parallel instructions would be grouped together in a [Very Long Instruction Word (VLIW)](https://en.wikipedia.org/wiki/Very_long_instruction_word). 

The IA-64 defined a 128-bit bundle of 3 instructions which were 41 bits each. The remaining 5 bits were used for routing instructions to various execution units. The 128-bit bundles could be linked together to create as many parallel instructions as the hardware could support but the maximum number of instructions that were dispatched per cycle was typically six.

Itanium had two major disadvantages that made it less of a marketing success than Intel had hoped. The first was that customers were reluctant to move away from the X86 with its huge inventory of compatible software. The second was that developing complex new compilers was very expensive. Still, while the Itanium never replaced X86 in popularity, it filled a niche market where it performed quite well.

## MAJC by Sun Microsystems

Another less well-known VLIW based Architecture was called [MAJC (Microprocessor Architecture for Java Computing)](https://en.wikipedia.org/wiki/MAJC). This processor designed by Sun Microsystems was an attempt to leverage the fact that the JAVA Programming Language used a technique called [Just in Time Compilation (JIT)](https://en.wikipedia.org/wiki/Just-in-time_compilation) to dynamically convert Java Bytecode into machine language. JIT compilation is a powerful concept that continues to be a major part of [Microsoft .NET Framework](https://en.wikipedia.org/wiki/.NET_Framework) and the programming languages that use it.

MAJC utilized a variable length VLIW instruction which were 1 to 4 instructions long. Each instruction was a fixed length of 32 bits. MAJC also used a single set of registers for integer and floating-point operations. This simplified the dispatching hardware and allowed for up to four floating-point or four integer instructions per cycle or any combination of the two. MAJC was only used internally at Sun, but the idea of using JIT to compile directly to a VLIW machine code was later used to emulate X86 as described below.

## Transmeta and Code Morphing

Just as Sun and the MAJC used JIT compilation to convert Java Byte Code into a VLIW based machine language, a company called Transmeta used [Code Morphing Software](https://en.wikipedia.org/wiki/Transmeta#Code_Morphing_Software) to translate X86 byte code into a proprietary VLIW.  One of the advantages of Code Morphing was that new features of X86 could be added to the supported architecture features without changing the hardware. The simpler hardware also cost less, consumed less power and produced less heat. Larger blocks of binary code could be analyzed for translation than when decoding was done by hardware. But doing translation in software rather than hardware did affect the performance of Transmeta chips. As time went on, the advantages of lower cost, speed, power, and heat were overcome by advances in newer X86 chips designed by Intel and AMD. Transmeta eventually went out of business. 

## Queued Operands

Another less known methodology for supporting ILP and speeding up serial code is using queued operands. In this methodology, register dependencies are eliminated by having instructions read input operands from the head of a Queue and write the results to the tail. This eliminates certain Data Hazards which prevent instructions to be executed in parallel. The use of queued operands greatly simplifies hardware design because the complexities of OoOE are eliminated. The instruction set is also very compact because the instructions can be one byte long with no data operands. For more information see the following paper produced in the Journal of Supercomputing:

A>    Sowa, Masahiro & Ben Abdallah, Abderazek & Yoshinaga, Tsutomu. (2005). 
A>    Parallel Queue Processor Architecture Based on Produced Order Computation Model. 
A>    The Journal of Supercomputing. 32. 217-229. 10.1007/s11227-005-0160-z. 

While RISC++ is not a Queue Processor in the strictest sense, it does use queued operands to facilitate dynamic allocation of registers for special queued instructions.

## Out of Order Execution (OoOE)

The most common way to speed up a serial instruction stream is called [Out of Order Execution (OoOE)](https://en.wikipedia.org/wiki/Out-of-order_execution). This method starts with a traditional [von Neumann architecture](https://en.wikipedia.org/wiki/Von_Neumann_architecture) which uses an instruction pointer and branch instructions to define the sequence of instructions. Then as serial instructions are decoded they are placed in [Reservation Stations](https://en.wikipedia.org/wiki/Reservation_station) where they wait until all their input data are ready. 

As the input data (source registers) become available, the instructions are sent to the appropriate parallel execution units (adders, multipliers, logical units, etc.) for execution. The instructions are often executed in a different order than the program sequence in memory. However, the results are written back to the registers in the original program order. 

OoOE is used by most modern implementations of X86. It is also used in the IBM PowerPC and various other processors. The main advantage of OoOE is that it *preserves legacy binary code* while still improving single thread performance. The main disadvantages are that it *requires highly complex hardware* and that only a small window of binary code is optimized at a time.

### The Parable of the Bank

To illustrate this let us take the example of a line of people waiting patiently in queue at a bank. The queue splits into several smaller queues, one for each teller. As each person is serviced by a teller, the customers move on in a different order than when they came in. Some people come in with their checks signed and their deposit slips filled out. Others need to get out of line to sign the check and fill out the deposit slip.

Meanwhile other customers pass by the unprepared customers, get their work done and leave. Once the unprepared customer gets his check signed and deposit slip filled out he can rejoin the line. At the same time the various tellers take a different amount of time depending on the transaction. Some simply deposit a check with all the information ready. Some count a wad of cash. Others have to count pennies. Others sign people up for credit cards they really shouldn't have, but that's another story. 

Some customers proceed quickly to the ATM and get their money right away. Still others use the drive through window and wait in line in their cars for a longer time. What does this have to do with computers? Let me explain.

### The Parable Explained.

RISC++ uses Out of Order Execution of instructions in the Typed Scalar Unit (TSU). However, the Fast Integer Unit and the Typed Vector Unit execute instructions in the order provided by the Instruction Pointer (IP). 

Some of the Condition Bits, which are set as a result of comparing two Typed Scalar registers, are set out of order. If these bits are tested while they are still in the process of being set, the processor will stall. If the COMPARE instruction uses a register that is being set as the result of a DIVIDE instruction, it may wait a long time. 

For this reason TSU conditional instructions, which are executed Out of Order, will wait in the Reservation Station for three data items; the condition bit, and the two input operands. If the condition bit is false, the instruction will be skipped and removed from the Reservation Station. If the CB is true, the instruction will be dispatched to the final stage of execution in the TSU.

The following instruction types are listed in the order of speed from slowest to fastest:

1. Slowest: Load from Memory when there is a cache miss.
2. Faster: Floating point Divide, Square Root, Sine, Cosine, etc.
3. Faster: Floating Point Multiply.
4. Faster: Floating Point Add, Subtract, Compare etc.
5. Fastest: Integer Add, Subtract, And, Or, Compare, etc.

Because instructions must wait in the reservation station until all the input operands are available, some may wait longer than others. The instructions are executed in a different order than that which was defined by the Instruction Pointer and Branch instructions. OoOE facilitates [Instruction Level Parallelism (ILP)](https://en.wikipedia.org/wiki/Instruction-level_parallelism). 

If the instruction is dispatched to the Fast Integer Unit (FIU) it executes very quickly, usually a single cycle. The FIU uses *"in order execution"*, each instruction must complete its operation before the other proceeds. This is like the customer who uses the ATM. He might need to stand in line to use it, but when he does he can finish his transaction quickly

If the instruction is dispatched to the Typed Vector Unit, it must wait in a queue until the two input operands are ready, but other instructions cannot bypass it. Since Vector instructions can take a long time to process, it could be a long wait. But like the FIU, the instructions proceed in the order in which they were dispatched. But once the Vector instruction is finally executed, it processes a large amount of data (256 bytes) in a single instruction cycle. In our bank analogy this could be the drive through window when there is only one teller.

Next I will explain further *why* RISC++ is more than just an interesting idea, but is a powerful CPU that can work cooperatively in today's computing environments.

