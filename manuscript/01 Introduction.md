{sample: true}
{class: part}

# Introduction

### About this Book

The intended audience for this book is people who understand both C++ and Assembler language programming. Familiarity with various CPUs and instruction sets from Intel, AMD, IBM, ARM, and others helps but is not essential. I desire to use this document as an educational tool for people studying Computer Science at a College level, and those who are self-taught. I will do my best to explain new ideas as they come up in future chapters. For now, if I use unfamiliar terms, please be patient, I'll explain them later.

This document was written using the Typora Markdown Editor. I also use the <u>Draw.io</u> desktop app for diagrams. The book will be published on [leanpub.com](https://leanpub.com/) and Amazon in PDF, Kindle, and paperback formats. Some chapters will also be available for free on GitHub. The online formats have the added benefit of being able to follow various links for further education. This includes both text and video links. The links are underlined in the electronic document text for easy navigation online. Then a benefit of using Lean Pub is that the full links are in the footnotes so that readers on the paper copy of the book can type them into their browser.

I use Wikipedia extensively, but I caution that these articles cannot necessarily be considered authoritative, but only as a starting point for further study. After reading this book and following the links, may I suggest that you donate to Wikipedia for providing such a rich set of relevant online information? Wikipedia articles tend to be comprehensive and might be overwhelming for beginners. There are many online courses that explain things more simply but some of them are copyrighted or need to be paid for. I don't want to risk copyright violation by linking to them. I will give my own brief explanations and if further study is needed, Wikipedia is a good place to start.

I would welcome comments and suggestions from experts in various fields. I would greatly appreciate input from people who design compilers for various languages such as C++. I would also welcome comments from hardware designers, operating system experts, and people who design critical performance scientific software. I am not an expert in any of those fields, but I have spent a great deal of time studying various computer architectures both at the ISA level and the implementations in hardware. I have built on top of what others have done and tried to streamline and pick the best features.

This document is my personal intellectual property. I don't plan on getting a patent but rely on my ideas' copyright protection. I desire to be given credit for the ideas in this document. I will also make every effort to credit the works of others, and I ask that I be credited with my ideas. I want this document to be used for educational purposes and I want the ideas to be as widespread as possible. In the end, it all comes down to trust.

### About the Author

I was born in Nairobi, Kenya the son of missionaries. I spent much of my childhood in Kenya and Tanzania. I graduated from Rift Valley Academy in 1974 and returned to the US for college. I graduated from Geneva College in 1978 with a double major in Physics and Computer science. I was very fortunate to get a job with IBM in Endicott N.Y. where I worked as a Computer Programmer testing the design of mid-range mainframes based on the 370 Architecture. A large part of my job involved writing software that verified that the hardware worked according to specifications.

I was involved in a project which used the RISC 801 processor developed by the research division of IBM. The system presented two architectures to the user, the RISC 801 and the IBM 370. After a long time of development, the system was finally announced and sold as the IBM 9370. While it could have run in "native mode" using a Unix-like operating system, that capability was never announced or sold. The 9370 did not do well in the marketplace, but eventually, it was used as the platform for the AS/400 system which was very successful.

To learn more, please read over the following Wikipedia Articles:

  * [IBM 801 RISC](https://en.wikipedia.org/wiki/IBM_801)
  * [IBM 9370](https://en.wikipedia.org/wiki/IBM_9370).

I was responsible to design the software that would run self-diagnosing test cases to verify that 801 ISA was properly implemented in hardware. The test cases were designed to set up initial conditions, execute a few instructions, predict the results and then compare actual results to those predicted. The software which controlled the test cases was a micro Operating System that I wrote in 801 Assembly language.

My job also involved testing various I/O devices connected to IBM 370 Mainframes. I tested various hard drives, printers, and telecommunication devices and protocols. My job required me to know the 370 CPU Architecture and I/O Architecture as well as IBM internal CPU Architectures used as I/O controllers. I also tested telecommunication architectures such as SNA/SDLC (System Network Architecture/Synchronous Data Link Control).

In 1986 I resigned from IBM and went to Seminary to become a pastor. Some saw it as a mid-life crisis, but I considered it a new calling. I will never regret that decision. It was incredibly rewarding to be able to help other people spiritually. I was a bi-vocational pastor and helped to support myself through free-lance programming, substitute teaching, and working in the mental health field.

But I never lost my passion for trying to learn about computer architecture. I can't remember when I started writing ideas in notebooks for my own RISC architecture. What started as "doodling", morphed into a hobby, then became something I wanted to publish. I was studying the C++ programming language and mentally "compiling" various operators, control structures, and data structures into my own RISC architecture which would be a close fit for the C++ language. I named it RISC++.

I want to make it clear from the outset that **any** High-Level Language can be compiled into the RISC++ machine code. C++ is merely the model used to define the instructions and data types. In my specification, I use examples of C++ code and show the resulting RISC++ Assembler language that would be created to support those structures. 

### Definition of terms

[Central Processing Unit (CPU)](https://en.wikipedia.org/wiki/Central_processing_unit) - The part of a computer system where software is run and which communicates with and controls all the other parts of the system. The CPU runs machine language instructions that perform arithmetic and logical operations. These instructions also move data around between RAM and various input and output devices such as the keyboard, mouse, display, hard drive, printer, and the internet. The first CPUs I worked on at IBM were the size of refrigerators. Now they are much more powerful and fit in my phone. But the basic functions have changed very little. They just run faster and handle larger amounts of data.

[Microarchitecture](https://en.wikipedia.org/wiki/Microarchitecture) - The internal design of various CPUs which defines how the machine code is interpreted and processed. The micro-architecture might change significantly from one CPU model to another without changing the machine code (ISA). As the number of transistors increases over time, CPU designers find ways to use those extra transistors to make programs compiled into the same ISA run faster. Intel and AMD have produced many different micro-architectures which are reflected in the names of their chip families.

The diagram below illustrates the various components of a generic CPU:



![Inside CPU](Inside CPU.drawio.png)





[Random Access Memory (RAM)](https://en.wikipedia.org/wiki/Random-access_memory) - The data that the CPU processes is moved into the CPU, processed, and then stored in very fast storage we call RAM or "memory". The data in RAM is lost when the power is turned off, therefore it is important to store the data on Hard Drives or Solid State Devices (SSD). There are many kinds of RAM that can be very fast and expensive or slower and more economical. More on that later when we discuss Registers and Caches.

Some micro-architectures are designed for very high speed. Others are designed for small size and low power consumption so they can be used in devices like phones, or even microwave ovens and watches. This leads to the discussion of CISC vs RISC.

[Instruction Set Architecture (ISA)](https://en.wikipedia.org/wiki/Instruction_set_architecture) - The interface between software and hardware. High-Level Languages (HLL) like C++, Python, and so forth, translate human-readable code into machine code that runs on the hardware. This machine code is defined in bits and is difficult for humans to program directly. Therefore the ISA is programmed in an Assembly Language. 

[Assembly Language](https://en.wikipedia.org/wiki/Assembly_language) - A human-readable programming language that has a direct correspondence with machine code instructions. Every different CPU has its own Assembly Language that only runs on that CPU. This distinguishes Assembly languages from HLLs which produce machine code for a great many CPUs each with its own ISA. The software that converts an HLL into machine code is called a [Compiler](https://en.wikipedia.org/wiki/Compiler). As new ISAs are invented, various compilers must be updated to support the specific machine code of each new CPU. Modern compilers translate into "middleware"

[GNU Compiler Collection (GCC)](https://en.wikipedia.org/wiki/GNU_Compiler_Collection) - A compiler is a program that converts a High-Level Language into machine code for a specific CPU. The GCC is a group of open-source compilers that produce machine code for a large number of ISAs (see link). GCC has compilers for C, C++, D, Objective C, Fortran, Pascal, Python, VDHL, and others. But it also targets a great many ISAs producing both Assembly and machine code. But it is important to understand that any time a new ISA is invented or new instructions are added to an existing ISA, the compilers need to be updated. This can become a major hindrance to the success of new CPUs in the marketplace, but more on that later.

[x86 ISA](https://en.wikipedia.org/wiki/X86) was first invented in 1978 by Intel and introduced on the 8086 processor. This processor was used in the IBM PC and on the many "clones" that created a new industry standard. Since Microsoft wrote the operating systems (DOS and Windows) that run on these computers, the x86 ISA became the de facto standard that continues today. x86 has been implemented on many CPUs from many manufacturers. The x86 ISA has added many new instructions over the decades (see link) but continues to support the original instructions from the 8086, allowing old binary machine code to run on new processors.

[Complex Instruction Set Computer (CISC)](https://en.wikipedia.org/wiki/Complex_instruction_set_computer) - ISA in which each instruction performs many operations. In the early days of computing, memory was scarce and expensive. Instructions were designed to do many operations so that the machine code was both compact and effective. But these instructions were too complex to be run directly on the hardware. Instead there needed to be a program running on the hardware that would translate the CISC instructions into simpler machine code that would be easier and faster for the hardware to interpret and run directly. This intermediate software is called [Microcode](https://en.wikipedia.org/wiki/Microcode). 

While the microcode allows for large numbers of complex and compact instructions to be implemented on many different hardware structures, it also uses up expensive memory on the chip and increases the time needed to decode and execute each instruction. But the most important contribution of microcode is that it provides a means by which software can be compiled into machine code and be able to run on many generations of hardware. This is the reason why the x86 ISA has been able to be implemented on many different CPUs from a large number of manufacturers over the course of many years. More on that later. 

[Reduced Instruction Set Computer (RISC)](https://en.wikipedia.org/wiki/Reduced_instruction_set_computer) - As CPUs got more and more transistors, the cost of RAM decreased. CPU designers wanted to eliminate the intermediate microcode and run the instructions produced by the compliers directly on the hardware. To do this, they designed instructions that would execute in a single clock cycle. The result is that CPUs could be designed with fewer transistors dedicated to computation and more could be used for high-speed cache RAM. The trade-off is that RISC processors tend to need more RAM to store the program. There are two reasons for this. First, each RISC instruction is a fixed length of 32 bits, while CISC instructions are variable length to save space in RAM. Secondly, each CISC instruction performs many operations that would take many RISC instructions to complete. 

The next topic which is necessary to compare RISC vs CISC is the concept of "pipelining". The illustration that is often used is the assembly line for a car. As a car goes through a manufacturing assembly line there are several stages performed by skilled specialists. For example:

1) Weld the chassis together then mount the wheels.
3) Mount the engine. 
4) Connect transmission to wheels.
5) Install seats.
6) Install body.
7) Paint the body.
8) Drive the car off. 

Of course, this is an oversimplification. It is important to note that there is a completely different assembly line to build the engine and transmission so they can be installed pre-built. Each stage is performed simultaneously so that while one team is welding the chassis, the next is mounting the wheels, and so forth. There should be enough work in each stage so that workers would not be sitting around waiting for the previous stage to be complete.  If each stage takes 20 minutes (driving is not counted) the whole process would take 120 minutes, but a car would be driven off the lot every 20 minutes.







[Classic RISC pipeline](https://en.wikipedia.org/wiki/Classic_RISC_pipeline) - CPUs also have an internal "assembly line" called a "pipeline". The CPU has an internal clock which is often measured in Giga Hertz (Ghz). This clock controls how instructions move through the pipeline. One of the goals of RISC processors is to execute an instruction every clock cycle. In order to achieve this goal the processing of instructions has clearly defined stages of execution that are performed simultaneously. 

These stages are:

1).  <u>Instruction Fetch (IF)</u> - The Instruction Pointer is used to tell the CPU where the next instruction is in RAM. The instruction is read into the Instruction Register and then passed to the Decode Stage. Meanwhile, the CPU increments the IP by 4 (32 bits) and reads the next instruction.

2). <u>Instruction Decode (ID)</u> - The decoding phase converts the various fields of the instruction into control lines that tell the various execution units what they must do. These control lines set up the 





| Clock Cycles  |  T1  |  T2  |  T3  |  T4  |  T5  |  T6  |  T7  |  T8  |
| ------------- | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
| Instruction 1 |  IF  |  ID  |  EX  | MEM  |  WB  |      |      |      |
| Instruction 2 |      |  IF  |  ID  |  EX  | MEM  |  WB  |      |      |
| Instruction 3 |      |      |  IF  |  ID  |  EX  | MEM  |  WB  |      |
| Instruction 4 |      |      |      |  IF  |  ID  |  EX  | MEM  |  WB  |







Below is a comparison of RISC vs CISC:

| RISC - <u>R</u>educed <u>I</u>nstruction <u>S</u>et <u>C</u>omputer | CISC - <u>C</u>omplex <u>I</u>nstruction <u>S</u>et <u>C</u>omputer |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Fixed length instructions. Usually 32-bit                    | Variable length instructions                                 |
| Programs use more RAM                                        | Programs use RAM efficiently                                 |
| Single clock cycle per instruction                           | Many clock cycles per instuction                             |
| Simple instruction pipeline with few stages                  | Complex pipeline with many stages                            |
| Most instructions are register to register                   | Many memory to memory instructions                           |
| More  architected registers                                  | Fewer architected registers                                  |
| Small number of instructions                                 | Large number of instructions                                 |
| Few addressing modes                                         | Many addressing modes                                        |
| Complex operations performed by software                     | Complex operations performed by microcode                    |
| Instructions run directly on hardware                        | Instructions translated by microcode                         |
| Examples: IBM 360/370/390; DEC VAX; Intel/AMD x86            | Examples: IBM PowerPC; DEC Alpha; ARM; MIPS; RISC-V          |



#### From RISC++ to TD-ISA

As I said earlier, I have designed an ISA with a close fit to the C++ language. In the course of developing RISC++, I have studied the evolution of various RISC and CISC ISAs and their microarchitectures. I have familiarized myself with the X86, PowerPC, Itanium, DEC Alpha,  SPARC, Sun MAJC, Sun Niagara, and others. 

I have been writing and re-writing RISC++ for over 20 years, all the time studying advances in "real" architectures and trying not to be obsolete before I publish it. Thomas Edison is quoted as saying that all his "failed" experiments were not failures at all. He just found 2000 ways to **NOT** make a light bulb, before he found the one that worked. I do not claim to have found the "perfect light bulb". But I do think this document itself, can be an effective educational tool to learn about the history of computer architecture from the 1980s to the present. It also shows how various High-Level Languages can be compiled into simpler, more efficient machine language code than in current architectures.

The idea that I could design a new hardware architecture that would actually be made into silicone is a dream that may never become real. But I think I might be able to offer ideas that others would incorporate into their designs. The idea that RISC++ would become an actual RISC ISA running directly on a specific micro-architecture is unrealistic and unnecessary. But one of the key features of RISC++ is typed data operands which I believe is a very important feature to include in middleware for compilers or even as a  micro-code front end for RISC processors such as RISC-V. In this document, I will fully define the RISC++ architecture as the underlying architecture of the CISC architecture called TD-ISA. 

By the way, TD-ISA means "Typed Data ISA" but if you want to call it "Tom Dunkerton's ISA" I won't be offended. Dear me, am I really that vain?

####  What is the Typed Data Instruction Set Architecture (TD-ISA)?

Anyone who has ever programmed in the X86 Assembler language can tell you that there are literally thousands of machine language instructions to choose from. This is especially true if the programming is being done using  [Single Instruction Multiple Data](https://en.wikipedia.org/wiki/Single_instruction,_multiple_data)  (SIMD) instructions for X86. To illustrate this point, I will devote a whole chapter to the many X86 instruction sets for SIMD. For now, try scanning over the [x86 instruction listings](https://en.wikipedia.org/wiki/X86_instruction_listings) for an overview.

One of the reasons why X86 has so many instructions is that the architecture evolved over decades to accommodate the exponential growth of CPU capability. In order to support backward compatibility the old instructions needed to be kept as new ones were added. Another reason for a large number of instructions in most (if not all) CPUs is that the data type of memory or registers is coded into the instruction rather than associated with the data. This means that there needs to be a separate Operation Code for each data type, each length of SIMD data, and each combination of data types. As SIMD registers grew from 128 to 256 to 512 bits in width, the number of x86 instructions exploded.

Consider the following example of C++ code:



```cpp
// Illustrates how various data types can be intermixed in C++      
// Operators mix data types and convert as needed   
// Output from floating point inputs are rounded to integer          

int i = 3;
int short unsigned j = 10;
int k = 0;
float x = 20.5;
double y = 8.3;
float z = 0;

k = i + J;    // k = 13      
z = x + y;    // z = 28.3   
z = x + j;    // z = 30.5  
j = x + j;    // k = 30    
z = j / i;    // z = 3.33  
k = j / i;    // k = 3     

```



These few examples illustrate the point that in the C++ language there is a limited number of arithmetic operators ( + - * / % ) which work with any data type or combination of data types. This is not the way in most machine language codes. Most machine languages have separate instructions for each data type. 

Furthermore, in the machine code, the input and output operands must be the same type. They cannot be mixed. Even though the Operation Code in the listing of Instructions may simply show an ADD, SUB, MUL, DIV, or REM, there are many modifiers that specify data length and the type, and whether the source and destination are in memory or registers. When the whole instruction is decoded using the OP code together with modifier fields, the instruction will not be able to mix operands of different types. 

As we study the instruction pipeline of RISC pro







```cpp
//  These examples are in a pseudo-code for a generic assembly language 
//  Assume the integer and floating point registers are 64 bits
//  However different data lengths in those registers require specific instructions
//  The source-1, source-2 and destination registers must be the same type and length 

		      //   
    addibu  id,is1,is2    	// 
    addih 	id,is1,is2      // Add Integer Half (16 bits)
    addihu	id,is1,is2  	// Add Integer Half unsigned
    addiw	id,is1,is2		// Add Integer Word (32 bits)
    addiwu  id,is1,is2      // Add Integer Word unsigned
    addid   id,is1,is2      // Add Integer Double (64 bits) 
    addidu  id,is1,is2      // Add Integer Double unsigned 
    addfw   fd,fs1,fs2      // Add Float Word (32 bits)  
    addfd   fd,fs1,fs2		// Add Float Double (64 bits)
        
// It is also neccesary to specifically convert data from on type to another
// Assuming both the Integer and Floating point registers are 64 bits  
// 8, 16, 32 and 64 bit signed have the same format in the integer register
// The same is true for unsigned integers of those lengths        
// At a minimum the following convert instructions are needed  
      
    cvtfwd   fd,fs1         // convert 32 bit float to 64 bit float
    cvtfdw   fd,fs1         // convert 64 bit float to 32 bit float
    cvtfwi   id,fs1         // convert 32 bit float to signed integer
    cvtfwiu  id,fs1         // convert 32 bit float to unsigned integer
    cvtfdi   id,fs1         // convert 64 bit float to signed integer
    cvtfdiu  id,fs1         // convert 64 bit float to unsigned integer
    cvtifw   fd,is1         // convert signed integer to 32 bit float
    cvtifw   fd,is1         // convert signed integer to 64 bit float
    cvtiufw  fd,is1         // convert unsigned integer to 32 bit float
    cvtiufd  fd,is1         // convert unsigned integer to 64 bit float
        
```



The point of this example is to show that in c++ there is one operator (+) to handle the add operation no matter what data type, in typical ISAs (both CISC and RISC), have a unique operation code for each data type. 

