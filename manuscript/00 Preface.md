# Preface

### About this Book

The intended audience for this book is people who understand both C++ and Assembly language programming. This book will give overviews of various CPUs and instruction sets from Intel, AMD, IBM, ARM, DEC, Sun, and others. I desire to use this document as an educational tool for people studying Computer Science at a College level, and those who are self-taught. I will do my best to explain new ideas as they come up in future chapters. For now, if I use unfamiliar terms, please be patient, I'll explain them later.

This document was written using the Typora Markdown Editor. I also use the <u>Draw.io</u> desktop app for diagrams. The book will be published on [leanpub.com](https://leanpub.com/) and Amazon in PDF, Kindle, and paperback formats. Some chapters will also be available for free on GitHub. The online formats have the added benefit of being able to follow various links for further education. There are shorthand links in the text with the full URL address in the footnotes. The links are underlined in the electronic document text for easy navigation online. Then a benefit of using Lean Pub is that the full links are in the footnotes so that readers on the paper copy of the book can type them into their browser.

I use Wikipedia extensively, but I caution that these articles cannot necessarily be considered authoritative, but only as a starting point for further study. After reading this book and following the links, may I suggest that you donate to Wikipedia for providing such a rich set of relevant online information? Wikipedia articles tend to be comprehensive and might be overwhelming for beginners. There are many online courses that explain things more simply but some of them are copyrighted or need to be paid for. I don't want to risk copyright violation by linking to them. I will give my own brief explanations and if further study is needed, Wikipedia is a good place to start.

I would welcome comments and suggestions from experts in various fields. I would greatly appreciate input from people who design compilers for various languages such as C++. I would also welcome comments from hardware designers, operating system experts, and people who design critical performance scientific software. I am not an expert in any of those fields, but I have spent a great deal of time studying various computer architectures both at the ISA level and the implementations in hardware. I have built on top of what others have done and tried to streamline and pick the best features.

This document is my personal intellectual property. I don't plan on getting a patent but rely on my ideas' copyright protection. I desire to be given credit for the ideas in this document. I will also make every effort to credit the works of others, and I ask that I be credited with my ideas. I want this document to be used for educational purposes and I want the ideas to be as widespread as possible. In the end, it all comes down to trust.

### About the Author

I was born in Nairobi, Kenya the son of missionaries. I spent much of my childhood in Kenya and Tanzania. I graduated from Rift Valley Academy in 1974 and returned to the US for college. I graduated from Geneva College in 1978 with a double major in Physics and Computer Science. I was very fortunate to get a job with IBM in Endicott N.Y. where I worked as a Computer Programmer testing the design of mid-range mainframes based on the 370 Architecture. A large part of my job involved writing software that verified that the hardware worked according to specifications.

I was involved in a project which used the RISC 801 processor developed by the research division of IBM. The system presented two architectures to the user, the RISC 801 and the IBM 370. After a long time of development, the system was finally announced and sold as the IBM 9370. While it could have run in "native mode" using a Unix-like operating system, that capability was never announced or sold. The 9370 did not do well in the marketplace, but eventually, it was used as the platform for the AS/400 system which was very successful.

To learn more, please read over the following Wikipedia Articles:

  * [IBM 801 RISC](https://en.wikipedia.org/wiki/IBM_801)
  * [IBM 9370](https://en.wikipedia.org/wiki/IBM_9370).

I was responsible to design the software that would run self-diagnosing test cases to verify that 801 ISA was properly implemented in hardware. The test cases were designed to set up initial conditions, execute a few instructions, predict the results and then compare actual results to those predicted. The software which controlled the test cases was a micro Operating System that I wrote in 801 Assembly language.

My job also involved testing various I/O devices connected to IBM 370 Mainframes. I tested various hard drives, printers, and telecommunication devices and protocols. My job required me to know the 370 CPU Architecture and I/O Architecture as well as IBM internal CPU Architecture used in controllers for various I/O devices.

In 1986 I resigned from IBM and went to Seminary to become a pastor. Some saw it as a mid-life crisis, but I considered it a new calling. I will never regret that decision. It was incredibly rewarding to be able to help other people spiritually. I was a bi-vocational pastor and helped to support myself through free-lance programming, substitute teaching, and working in the mental health field.

But I never lost my passion for trying to learn about computer architecture. 

### Reduced Instruction Set for C++ (RISC++)

I can't remember when I started writing ideas in notebooks for my own RISC architecture. What started as "doodling", morphed into a hobby, then became something I wanted to publish. I was studying the C++ programming language and mentally "compiling" various operators, control structures, and data structures into my own RISC architecture which would be a close fit for the C++ language. I named it RISC++.

I want to make it clear from the outset that **any** High-Level Language can be compiled into the RISC++ machine code. C++ is merely the model used to define the instructions and data types. In my specification, I use examples of C++ code and show the resulting RISC++ Assembler language that would be created to support those structures. 

In the course of developing RISC++, I have studied the evolution of various RISC and CISC ISAs and their microarchitectures. I have familiarized myself with the X86, PowerPC, Itanium, DEC Alpha,  SPARC, Sun MAJC, Sun Niagara, and others. We will devote a chapter to reviewing many different CPUs with their ISAs and micro-architectures.

I have been writing and re-writing RISC++ for over 20 years, all the time studying advances in "real" architectures and trying not to be obsolete before I publish it. Thomas Edison is quoted as saying that all his "failed" experiments were not failures at all. He just found 2000 ways to **NOT** make a light bulb, before he found the one that worked. I do not claim to have found the "perfect light bulb". But I do think this document itself, can be an effective educational tool to learn about the history of computer architecture from the 1980s to the present. It also shows how various High-Level Languages can be compiled into simpler, more efficient machine language code than in current architectures.

The idea that I could design a new hardware architecture that would actually be made into silicone is a dream that may never become real. But I think I might be able to offer ideas that others would incorporate into their designs. The idea that RISC++ would become an actual RISC ISA running directly on a specific micro-architecture is unrealistic and unnecessary. But one of the key features of RISC++ is typed data operands which I believe is a very important feature to include in middleware for compilers or even as a  micro-code front end for RISC processors such as RISC-V. In this document, I will fully define the RISC++ architecture as the underlying architecture of a new CISC architecture called TD-ISA. 

### Typed Data Instruction Set Architecture (TD-ISA)

Anyone who has ever programmed in the X86 Assembler language can tell you that there are literally thousands of machine language instructions to choose from. This is especially true if the programming is being done using  [Single Instruction Multiple Data](https://en.wikipedia.org/wiki/Single_instruction,_multiple_data)  (SIMD) instructions for X86. To illustrate this point, I will devote a whole chapter to the many X86 instruction sets for SIMD. For now, try scanning over the [x86 instruction listings](https://en.wikipedia.org/wiki/X86_instruction_listings) for an overview.

One of the reasons why X86 has so many instructions is that the architecture evolved over decades to accommodate the exponential growth of CPU capability. In order to support backward compatibility the old instructions needed to be kept as new ones were added. Another reason for a large number of instructions in most (if not all) CPUs is that the data type of memory or registers is coded into the instruction rather than associated with the data. This means that there needs to be a separate Operation Code for each data type, each length of SIMD data, and each combination of data types. As SIMD registers grew from 64 to 128 to 256 to 512 bits in width, the number of x86 instructions exploded.

The key feature of TD-ISA is to simplify the job of various compilers so they can produce bytecode that will run on many different CISC and RISC CPUs. The goal is not only portability but also high performance, especially in the use of dynamically typed Vector Processing. The TD-ISA Assembler instructions are very close to what "*you would expect*" a C++ compiler to produce. This will become more apparent as we get into the details which are beyond the scope of this chapter.







