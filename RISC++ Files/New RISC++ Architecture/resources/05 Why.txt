# Why Do We Need Another ISA? 

This chapter is going to give a justification for introducing a new ISA at this point in time. Since this "hobby" has had me studying computer architectures over the a long period of time, I think it is good to review the way various computer architectures have evolved. Then I can show how RISC++ fits in the current state of technology. This chapter is going to have many links. I don't expect people will want to look at them all. Some concepts that may be new to some readers will be explained better later in the document.

## RISC++ instruction set is simpler than X86

If you are a compiler writer, a developer of math libraries, someone who has written assembly level code for high performance vector processing (before GPGPUs and using SIMD), or if you are one of those "god like" people who have implemented the X86 Architecture in hardware, you will probably have no argument that X86 has become complex and cumbersome. But it also is an amazing piece of technology upon which the vast majority of computer software in the world today runs very well. 

The reason why I believe there is room for another Instruction Set Architecture is because the X86 family of micro-processors have unnecessary complexity in their hardware due to the fact that they have evolved over the course of several decades. Since one of the requirements of the X86 hardware is to remain compatible with previous versions, this means that hardware must be more complex than if it only needed to support a new, streamlined, simple instruction set. 

In the early days of programming, in both Assembler and C++, the programmer had much control over the binary code produced. But more control also meant that he or she had to spend much of time learning the intricacies of the hardware and less time in actually solving problems. Today there are all kinds of software tools and code libraries that insulate the programmer from the complexity of the underlying hardware. But the complexity is still there and the various "middle-ware" software must deal with it.

If the design of the RISC++ instruction set and hardware is sound, (and I believe it is), programmers will see very little difference in how they write code. In fact they probably won't even know if the code will be run on a RISC++ chip, since many of them will be in cloud based servers where only the bravest geeks dare go. But the compiler will generate more streamlined and efficient code. 

But wait, you might say, memory is cheap and fast today, why bother economizing? The answer is that on-chip caches are still premium hardware real estate and any economies will yield great rewards in processor performance. Not only does the generated code take up less memory. It also has much shorter path lengths so it runs faster.

I can hear the next objection. In the last chapter I argued the *CISC* not *RISC* ISAs have the smaller memory foot print. This is true to a point. The variable instruction length and memory to memory instructions do take up less space in the Instruction Cache. But I think RISC++ binary code will use less memory because a *single code image* supports many data types. 

This is especially important when [Operator Overloading](https://en.wikipedia.org/wiki/Operator_overloading) is used to define various data types or combination of data types. When HLL code with Operator Overloading is compiled to most ISAs, like the X86, there must be many *type specific* copies of the binary code. 

With RISC++, only the Load and Store instructions need to be type specific. All the Typed Scalar arithmetic instructions are generic in regards to data type, and thus only one binary code image is needed. Since typed registers can be loaded before calling a subroutine, then passed on the stack, the subroutine itself does not need any type specific arithmetic instructions.

Take time to scan the [x86 and AMD64 instruction reference](https://www.felixcloutier.com/x86/index.html). What do you notice? How many different versions of ADD, SUBTRACT, MULTIPLY, DIVIDE, COMPARE, CONVERT, instructions are there? In X86 (as with most architectures) there is a unique instruction for each data type. 

Once you start putting multiple data types inside 64 and 128 bit registers, and then allow combinations of types in a single instruction, the number of instructions increases exponentially. Imagine how complex the complier has to be and how many different code libraries are needed to generate code for various combinations of data types, especially with "operator overloading". (I assuming some knowledge of C++ programming here).

The X86-64 instruction set has a very large number of instructions. A great number of these support various data types and the combinations of types. I am referring to data types such as 8, 16, 32 and 64 byte integer (signed and unsigned) as well as 32 and 64 bit Floating Point. These types are so common that no one seems to think how many instructions it takes the manipulate them, especially when there are both scalar and vector instructions that use them. 

When high level languages allow the mixing of data types in an operation it seems so easy to the programmer. But under the covers the compiler has to determine which instructions to use out of a vast array of choices. In the past, not all chips supported the full instruction set, so that made the job of the compiler even harder as it needed to test bits to determine which features were available on the particular chip is was running on. Thank goodness the X86-64 instruction set seems to have stabilized. But it is still very complex.

##  X86 Architecture Overview

In this document, the term [X86](https://en.wikipedia.org/wiki/X86) includes the ISA that is implemented on many families of software compatible chips designed by Intel, AMD, and other companies. Looking at the linked Wikipedia article will show what a large number of versions of X86 are out there and how many companies manufacture them. X86 includes both 32 and 64-bit scalar instruction sets and many variants of vector (SIMD) instruction-sets. It also includes a complex variety of options to handle real and virtual memory addressing.

Over time, the scalar instructions of X86 have evolved from 8 to 64-bit computers with considerable duplication of function. New innovations and higher clock speeds continue to squeeze more and more performance out of the chips. Deep pipelines, Out of Order Execution, replication of resources, and other techniques all work together to produce higher and higher performance of an aging architecture based on a serial instruction stream. However, this has not come without a cost. A significant amount of resources are dedicated to supporting the complex instruction set, rather than being devoted to speed and functionality. 

But the real complexity was in the compilers that tried to keep up with an ever evolving set of instructions which were not implemented on every chip. For this reason the SSE instructions were not implemented in a transparent way to the programmer. Instead the programmer had to drop down into assembler if he or she really wanted to get the most performance possible. Some C++ compliers included intrinsic data types and instructions which were basically allowing direct access to the hardware without resorting to Assembler programming. But the code was very machine specific and could not be easily ported from one processor type to another.

Reading the following two articles will give a good overview of the X86 ISA and how it evolved over time:

1. [IA-32](https://en.wikipedia.org/wiki/IA-32): The 32 bit version of the X86 architecture which lasted many years.

2. [X86-64](https://en.wikipedia.org/wiki/X86-64): The 64 bit version which almost all new processors implement.

##  X86 Single Instruction Multiple Data [(SIMD)](https://en.wikipedia.org/wiki/SIMD) 

Long before graphics processors were around software had to process large blocks of data which represented the graphics to be displayed on the screen. The CPU had to fill the screen buffer very quickly especially as screen resolutions increased and games required animations in real time. 

The 16/32 bit processors of that time were no match for the demands of gaming computers. Over time, and as memory prices dropped, graphics coprocessors emerged and eventually evolved to the processing powerhouses they are today. Now we have high resolution live video on our phones and we seem to take it for granted. But it was not always this way. 

At first, the problem of processing large parallel data sets was done in the CPU using instructions that processed several bytes of parallel data in a single register. If you have not yet done so please read the Wikipedia article on SIMD linked above. Then, if you wish, follow the links of each of the variations on SIMD which evolved over time. Below I will very briefly summarize the main points.

### [MMX](https://en.wikipedia.org/wiki/MMX_(instruction_set)): Introduced in January 1997 by Intel.

* Used to speed up the processing of parallel *integer* data for graphics.
* Instructions used 64 bit floating point registers to hold parallel integer data.
* Internal [80 bit Floating Point](https://en.wikipedia.org/wiki/Extended_precision) registers use [NAN](https://en.wikipedia.org/wiki/NaN) encoding to flag integer data.
* Integer data types: 8 x 8-bit, 4 x 16-bit, 2 x 32-bit, 1 x 64-bit.
* Pro: Using Floating Point registers allowed Operating System to save and restore registers during context switching without modification.
* Con: Sharing Regs between Floating Point and graphics software slowed both down.

### [3DNow!](https://en.wikipedia.org/wiki/3DNow!): SIMD ISA by Advanced Micro Devices (AMD) in 1998.

* Like MMX, it used Floating Point Registers with the same pros and cons.
* Added Vector Floating Point using two side by side 32-bit floats in a 64-bit register.
* Add, Subtract, Multiply Vector float. Convert to/from integer and float various lengths.
* Reciprocal approximation followed by Multiply instead of Divide.
* Square root and Reciprocal Square root approximation.
* Average of various integer types (8, 16, 32, signed unsigned) horizontally in 8-byte register.
* Special instructions to speed up context switching and cache management.
* Not popular with software developers. Support dropped in August 2010

In 1999 Intel introduced a whole new set of instructions to handle SIMD called Streaming SIMD Extensions (SSE) as part of the [Pentium III](https://en.wikipedia.org/wiki/Pentium_III) processor. The Pentium III was a 32 bit processor and did not have many of the features now found in X86-64 processors. 

Also, in the meantime, Intel was working on the [Itanium Architecture](https://en.wikipedia.org/wiki/Itanium) (also known as IA-64) as its new entry into 64 bit computing. We will talk about the pros and cons of this architecture later. For now let it suffice to say that it was not at all compatible with the 32 bit X86. 

AMD saw this and got to work on its own version of a 64 bit Architecture which *was* backwards compatible with the 32 bit version This was rightly called AMD-64. AMD broke new ground but it wasn't long before Intel realized that the AMD-64 was the future and began making its own versions Now it is the dominant architecture world wide. 

The following is a summary of the evolution of SSE:

### [SSE](https://en.wikipedia.org/wiki/Streaming_SIMD_Extensions) Extension of X86-32 SIMD introduced in 1999.

* Continued to use MMX instructions for integer operations mapped to Floating Point registers.
* Added 8 new 128 bit registers for 4 x 32 bit Floating Point Data.
* Introduced new scalar and "packed" (vector) operations.
* Scalar/Vector: Add, Subtract, Multiply, Divide, Reciprocal, Square Root, Reciprocal Square Root, Minimum, Maximum.
* Various data shuffling, conversion, compare, bit manipulation and memory access instructions.

### [SSE2](https://en.wikipedia.org/wiki/SSE2) Extension of 32 bit Pentium 4 introduced in 2000.

* SSE2 added 144 new instructions to SSE, which has 70 instructions.
* Uses 4 x 32 and 2 x 64 bit floating point data in 128 bit registers.
* Included 16 x 8, 8 x 16, 4 x 32, 2 x 64 bit integers.
* Same basic scalar and vector instructions as SSE with new data types.
* Large number of instructions to accommodate various data types and combinations of types.
* Smaller Floating Point data causes round off errors.
* Here is the full [X86-64 Instruction Listings for SSE2](https://en.wikipedia.org/wiki/X86_instruction_listings#SSE2_instructions)

### [SSE3](https://en.wikipedia.org/wiki/SSE3) Intel introduced SSE3 in early 2004

* SSE3 contains 13 new instructions over SSE2
* Added more horizontal instructions which added and subtracted data across the 128 bit registers.
* Instructions to support array of structures.
* MONITOR, MWAIT - These optimize multithreaded applications, giving processors with [Hyper-threading](https://en.wikipedia.org/wiki/Hyper-threading) better performance.
* NOTE: Hyperthreading is not used in RISC++. Instead the architecture tries to improve performance of serial threads, as will be explained in the next chapter.

### [SSSE3](https://en.wikipedia.org/wiki/SSSE3) Supplemental Streaming SIMD Extensions: 

* Introduced with Intel processors based on the [Core micro-architecture](https://en.wikipedia.org/wiki/Intel_Core_(microarchitecture)) in June 2006
* SSSE3 contains 16 new discrete instructions. 
* Each instruction can act on 64-bit MMX or 128-bit XMM registers. 
* Twelve instructions that perform horizontal ADD or SUBTRACT. 
* Six instructions that evaluate absolute values.
* Two instructions that perform "multiply and add" operations.
* Two instructions that accelerate packed-integer multiply operations.
* Two instructions that perform a byte-wise, in-place shuffle.
* Six instructions that negate packed integers.
* Two instructions that align data from two operands.

### [SSE4](https://en.wikipedia.org/wiki/SSE4) Announced on September 27, 2006

* SSE4.1 consists of 54 instructions.
* SSE4.2, a second subset consisting of the 7 remaining instructions
* Multiplication of different size integers in vector (packed).
* Dot Product of arrays.
* Conditional copying based on mask bits.
* Packed minimum/maximum for different integer operand types 
* Round values in a floating-point register to integers, using one of four rounding modes specified by an immediate operand 

### [SSE5](https://en.wikipedia.org/wiki/SSE5) Proposed by AMD on 2007

* NOTE: the linked article is confusing about what instructions were actually implemented and which were dropped. 
* The proposal is included as refence to the kind of ideas that were being discussed at the time.
* AMD proposed 256 **bit** SIMD, RISC++ has 256 **byte** variable length vectors.

Various compilers sought to use normal C++ "for loops" to generate code directed at various SIMD instruction sets. This is called [Automatic vectorization](https://en.wikipedia.org/wiki/Automatic_vectorization). Today the [Intel C++ Compiler](https://en.wikipedia.org/wiki/Intel_C%2B%2B_Compiler) and the [GNU Compiler Collection](https://en.wikipedia.org/wiki/GNU_Compiler_Collection) seem to have conquered the very complex issues of Automatic Vectorization. 

It is my belief, however, that the way RISC++ handles scalar and vector data facilitates Automatic Vectorization seamlessly. This is because the instructions already work together to provide easy compilations of "for loops". But I will need experts in compiler design to refute or validate that presumption.

## A brief History of RISC++

When I first started designing RISC++ there were two essential ideas which have never changed. The first was dynamically typed registers. The second was conditional instructions. I could have published back then and people would have said "cool, but why?". Who cares what the hardware is doing or what the Assembler Language and  machine code looks like, as long as my High Level Language (HLL) code works. 

As time when on this "hobby" was a welcome distraction from my "real job" as a pastor. I enjoyed reading about the evolution of the X86, and reading up on other architectures such as IBM Power PC, DEC Alpha, Intel Itanium, and others. 

But I was always looking at the ISA and trying to think through how to go from C++ to the machine code architecture, and then how to implement that machine code in real hardware. As I have hopefully demonstrated in this chapter, the greatest challenge is in how to make HLL *"for loops"* translate into fast Vector instructions.

A preliminary version of RISC++ 1.0 was published on Arstechnica.com and several people made comments. At the time, I was trying to incorporate Scalar and Vector (SIMD) instructions into a single thread by using a unified set of 128-bit registers. The goal was to simplify the hardware by having a single execution unit, a single instruction set, and a single register set, for all the operations in a thread of operation. 

Many of the comments were about the rare use of 128-bit scalar data, and the wasted space in the registers when 8, 16, 32 or 64-bit scalar data was needed. 128-bit registers are fine for SIMD instructions but are inefficient for scalar. Another major complaint was the use of dynamic register saving when using 128-bit registers. I also did not explain the “why” of dynamic typing very well.

So, I archived version 1.0 and started working on version 2.0. That version had two completely different processor types, one for scalar and another for vector operations. That configuration was much like the use of a general purpose Central Processing Unit (CPU) for scalar operations and General Purpose Computing on Graphics Processing Units (GPGPU) for vector operations. These two processor types are used extensively in various CPU and GPU chips manufactured by Intel, AMD and Nvidia. 

The main difference between RISC++ 2.0 and Graphics Based hardware was that the RISC++ ISA supported more IEEE 754 Data Types and exceptions. The Vectors were massive as they are in GPGPU applications. Also the RISC++ vector hardware had no graphics specific hardware. The scalar architecture was about what it is today. (TSRs and Conditions bits have been around since the early '90s). But there were all kinds of problems inherent in having a [Heterogeneous System Architecture](https://en.wikipedia.org/wiki/Heterogeneous_System_Architecture).

See also: [Multiprocessor system architecture](https://en.wikipedia.org/wiki/Multiprocessor_system_architecture).

The main problem was that this approach was moving away from the primary goal of RISC++, to produce a simple binary code generated from a C++ like compiler. The CPU/GPU configuration requires an Application Programming Interface (API) like [CUDA](https://en.wikipedia.org/wiki/CUDA) to utilize the GPU for vector operations. CUDA helps the programmer by handling the issues involved with a specific hardware configuration, synchronization of threads, etc. 

That configuration was originally designed for Graphics Processing and later adapted for general purpose vector processing. Such an approach has proven to be very successful for many reasons, not the least of which is the ubiquity of X86 CPUs and graphics processor cards made popular by the gaming industry. But this hardware configuration requires a hardware specific programming paradigm.

RISC++ is not intended to replace existing hardware and software, but to provide a niche solution for *“scientific processing”*. Scientific processing makes heavy use of the 64-bit binary floating-point numerical format for both scalar and vector operations. Therefore, the hardware has been optimized for this format, while also supporting other formats. 

RISC++ Version 3.0 (now simply RISC++) defines a configuration with a Typed Scalar Unit (TSU), which supports Instruction-level parallelism (ILP) and a Vector Processing Unit (VPU), which implements Data Parallelism. Both units are accessed by a single thread of instructions. RISC++ also defines an Instruction Control Unit (ICU), a Load/Store Unit (LSU) and a Fast Integer Unit (FIU). We have introduced these units and will examine them in more detail later in the document.

## Intel's oneAPI and Data Parallel C++

Intel has been working hard on the problem of supporting various kinds of processors through a single Application Programming Interface (API). Their solution, which is quite recently announced, is called the [Intel® oneAPI Toolkits](https://software.intel.com/en-us/oneapi). This API supports a new Intel C++ compiler called [Intel® oneAPI Data Parallel C++](https://software.intel.com/en-us/oneapi/dpc-compiler). Together, the API and the Compiler, take "normal" C++ coding practices and produce machine code which can run on the following processor types:

1. [Central processing unit (CPU)](https://en.wikipedia.org/wiki/Central_processing_unit)
2. [Graphics processing unit (GPU)](https://en.wikipedia.org/wiki/Graphics_processing_unit)
3. [Field-programmable gate array](https://en.wikipedia.org/wiki/Field-programmable_gate_array)
4. And various other specialized accelerators.

I have not yet had time to fully study Data Parallel C++ and adapt RISC++ to fit. As I said, this whole project has had me chasing after evolving technology and never getting published. I am hoping that the way RISC++ handles vectors already supports the Intel compiler constructs. If not, it is still the goal of RISC++ to support legacy C++ programs without requiring a new programing paradigm.

Tools like Data Parallel C++ insulate the programmer from the complexity of the X86 instruction set and hardware, but that complexity is still there "under the covers". 

In RISC++, the combination of *typed registers*, *typed variable length vectors* and *conditional instructions* facilitate compact code to be fetched from the Instruction Cache and to be executed with a minimal number of branches. 

Also, while the X86 hardware uses 128 bit registers to facilitate Vector operations, each RISC++ vector instruction operates on 16 x 128 bits (256 bytes) in every cycle. A single RISC++ vector instruction like ADD VECTOR, MULTIPLY VECTOR, etc. replaces complex SIMD used in the X86 ISA. The RISC++ Assembler code also has a close relationship to the C++ source code and can be read intuitively by human beings.

The rest of the book will go into more details about the RISC++ ISA and the hardware which implements it.


 


