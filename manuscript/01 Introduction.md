{sample: true}
{class: part}

# Introduction

#### About this Book

The intended audience for this book is people who understand both C++ and Assembler language programming. Familiarity with various CPUs and instruction sets from Intel, AMD, IBM, ARM, Sun, etc. helps but is not essential. I desire to use this document as an educational tool for people studying Computer Science at a College level, and those who are self-taught. I will do my best to explain new ideas as they come up in future chapters. For now, if I use unfamiliar terms, please be patient, I'll explain them later.

This document was written using the Typora Markdown Editor under Windows 10. I also use the <u>Draw.io</u> desktop app for diagrams. The book will be published on [leanpub.com](https://leanpub.com/) and Amazon in PDF, Kindle, and paperback formats. The online formats have the added benefit of being able to follow various links for further education. This includes both text and video links.

I use Wikipedia extensively, but I caution that these articles cannot necessarily be considered authoritative, but only as a starting point for further study. After reading this book and following the links, may I suggest that you donate to Wikipedia for providing such a rich set of relevant online information?

I would welcome comments and suggestions from experts in various fields. I would greatly appreciate input from people who design compilers for various languages such as C++. I would also welcome comments from hardware designers, operating system experts, and people who design critical performance scientific software. I am not an expert in any of those fields, but I have spent a great deal of time studying various computer architectures both at the ISA level and the implementations in hardware. I have built on top of what others have done and tried to streamline and pick the best features.

This document is my personal intellectual property. I don't plan on getting a patent but I rely on the copyright protection of my ideas. I desire to be given credit for the ideas in this document. I will also make every effort to credit the works of others, and I ask that I be credited with my ideas. I want this document to be used for educational purposes and I want the ideas to be as widespread as possible. In the end, it all comes down to trust.

####  What is the Typed Data Instruction Set Architecture (TD-ISA)?

The <u>TD-ISA</u> defines a bytecode that can be the target of various High-Level Languages like C, C++, FORTRAN, Python, and other languages which use vectors and matrixes of Integer Floating Point data types. This bytecode could be implemented as:

1) Software emulators such as QEMU. 
2) Just In Time (JIT) compiler such as used by JAVA. 
3) Microcode front end to RISC processors such as ARM and RISC-V.
4) Extension set of instructions for X86.

Anyone who has ever programmed in the X86 Assembler language can tell you that there are literally thousands of machine language instructions to choose from. This is especially true if the programming is being done using Single Instruction Multiple Data (SIMD) instructions for X86. To illustrate this point, I will devote a whole chapter to the many X86 instruction sets for SIMD.

One of the reasons why X86 has so many instructions is that the architecture evolved to accommodate the exponential growth of CPU capability over the decades. In order to support backward compatibility the old instructions needed to be kept as new ones were added. Another reason for a large number of instructions in most (if not all) CPUs is that the data type of memory or registers is coded into the instruction rather than associated with the data. Let me explain: 









#### About the Author

I was born in Nairobi, Kenya the son of missionaries. I spent much of my childhood in Kenya and Tanzania. I graduated from Rift Valley Academy in 1974 and returned to the US for college. I graduated from Geneva College in 1978 with a double major in Physics and Computer science. I was very fortunate to get a job with IBM in Endicott N.Y. where I worked as a Computer Programmer testing the design of mid-range mainframes based on the 370 Architecture. A large part of my job involved writing software that verified that the hardware worked according to specifications.

I was involved in a project which used the RISC 801 processor developed by the research division of IBM. The system presented two architectures to the user, the RISC 801 and the IBM 370. After a long time of development, the system was finally announced and sold as the IBM 9370. While it could have run in "native mode" using a Unix-like operating system, that capability was never announced or sold. The 9370 did not do well in the marketplace, but eventually, it was used as the platform for the AS/400 system which was very successful.

To learn more, please read over the following Wikipedia Articles:

  * [IBM 801 RISC](https://en.wikipedia.org/wiki/IBM_801)
  * [IBM 9370](https://en.wikipedia.org/wiki/IBM_9370).

I was responsible to design the software that would run self-diagnosing test cases to verify that 801 ISA was properly implemented in hardware. The test cases were designed to set up initial conditions, execute a few instructions, predict the results and then compare actual results to those predicted. The software which controlled the test cases was a micro Operating System written in 801 Assembly language.

My job also involved testing various I/O devices connected to IBM 370 Mainframes. I tested various hard drives, printers, and telecommunication devices and protocols. My job required me to know the 370 CPU Architecture and I/O Architecture as well as IBM internal CPU Architectures used as I/O controllers. I also tested telecommunication architectures such as SNA/SDLC (System Network Architecture/Synchronous Data Link Control).

In 1986 I resigned from IBM and went to Seminary to become a pastor. Some saw it as a mid-life crisis, but I considered it a new calling. I will never regret that decision. It was incredibly rewarding to be able to help other people spiritually. I was a bi-vocational pastor and helped to support myself through free-lance programming, substitute teaching, and working in the mental health field.

But I never lost my passion for trying to learn about computer architecture. I can't remember when I started writing ideas in notebooks for my own RISC architecture. What started as "doodling", morphed into a hobby, then became something I wanted to publish. I was studying the C++ programming language and mentally "compiling" various operators, control structures, and data structures into my own RISC architecture which would be a close fit for the C++ language. I named it RISC++.

I want to make it clear from the outset that **any** High-Level Language can be compiled into the RISC++ machine code. C++ is merely the model used to define the instructions and data types. In my specification, I use examples of C++ code and show the resulting RISC++ Assembler language that would be created to support those structures. 

#### From RISC++ to TD-ISA

In the course of developing RISC++, I have studied the evolution of various ISAs and the implementation of ISAs. I have familiarized myself with the X86, PowerPC, Itanium, DEC Alpha,  SPARC, Sun MAJC, Sun Niagara, and others. I became aware of RISC-V back in 2020, and I realized how advanced it had become in terms of the development tools when I started watching the YouTube videos of the RISC-V Workshop, Barcelona May 7th, 2018. I knew that I wanted to get on board and help as I can in helping review the RISC-V ISA Specifications for clarity and offering ideas for improvement.

I have been writing and re-writing RISC++ for over 20 years, all the time studying advances in "real" architectures and trying not to be obsolete before I publish it. Thomas Edison is quoted as saying that all his "failed" experiments were not failures at all. He just found 2000 ways to **NOT** make a light bulb, before he found the one that worked. I do not claim to have found the "perfect light bulb". But I do think this document itself, can be an effective educational tool to learn about the history of computer architecture from the 1980s to the present. It also shows how various High-Level Languages can be compiled into simpler, more efficient machine language code than in current architectures.

As I have recently been studying the RISC-V processor, I have come to realize in a greater way that there are many good reasons
