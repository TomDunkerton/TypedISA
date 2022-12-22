{sample: true}
{class: part}

# Preface

## About the Author

I was born in Nairobi, Kenya the son of missionaries. I spent much of my childhood in Kenya and Tanzania. I graduated from Rift Valley Academy in 1974 and returned to the US for college. I graduated from Geneva College in 1978 with a double major in Physics and Computer science. I was very fortunate to get a job with IBM in Endicott N.Y. where I worked as a Computer Engineer testing the design of mid-range mainframes based on the 370 Architecture. A large part of my job involved writing software that verified that the hardware worked according to specifications.

I was involved in a project which used the RISC 801 processor developed by the research division of IBM. The system presented two architectures to the user, the RISC 801 and the IBM 370. After a long time of development, the system was finally announced and sold as the IBM 9370. While it could have run in "native mode" using a Unix-like operating system, that capability was never announced or sold. The 9370 did not do well in the marketplace, but eventually, it was used as the platform for the AS/400 system which was very successful.

To learn more, please read over the following Wikipedia Articles:

  * [IBM 801 RISC](https://en.wikipedia.org/wiki/IBM_801)
  * [IBM 9370](https://en.wikipedia.org/wiki/IBM_9370).

I was responsible to write the specification and design the software that would run self-diagnosing test cases to verify that 801 ISA was properly implemented in hardware. The test cases were designed to set up initial conditions, execute a few instructions, predict the results and then compare actual results to those predicted. The software which controlled the test cases was a micro Operating System written in 801 Assembly language.

My job also involved testing various I/O devices connected to IBM 370 Mainframes. I tested various hard drives, printers, and telecommunication devices and protocols. My job required me to know the 370 CPU Architecture and I/O Architecture as well as IBM internal CPU Architectures used as I/O controllers. I also tested telecommunication architectures such as SNA/SDLC (System Network Architecture/Synchronous Data Link Control).

In 1986 I resigned from IBM and went to Seminary to become a pastor. Some saw it as a mid-life crisis, but I considered it a new calling. I will never regret that decision. It was incredibly rewarding to be able to help other people spiritually. I was a bi-vocational pastor and helped to support myself through free-lance programming, substitute teaching, and working in the mental health field.

But I never lost my passion for trying to learn about computer architecture. I can't remember when I started writing ideas in notebooks for my own RISC architecture. What started as "doodling", morphed into a hobby, then became something I wanted to publish. I was studying the C++ programming language and mentally "compiling" various operators, control structures, and data structures into my own RISC architecture which would be a close fit for the C++ language. I named it RISC++. The preliminary version of RISC++ was published in April 2022 and is available upon request.

I want to make it clear from the outset that **any** High-Level Language can be compiled into the RISC++ machine code. C++ is merely the model used to define the instructions and data types. In my specification, I use examples of C++ code and show the resulting RISC++ Assembler language that would be created to support those structures. I think that RISC++ may never be directly implemented in hardware, but it might be useful as "middleware" that can be dynamically translated into binary machine code for RISC-V, ARM, X86, and other architectures through Just In Time (JIT) compilation.

In the course of developing RISC++, I have studied the evolution of various ISAs and the implementation of ISAs. I have familiarized myself with the X86, ARM, PowerPC, Itanium, DEC Alpha,  Sun MAJC, Sun Niagara, and others. I became aware of RISC-V back in 2020, but I only realized how advanced it had become in terms of the development tools when I started watching the YouTube videos of the RISC-V Workshop, Barcelona May 7th, 2018. I knew that I wanted to get on board and help as I can in helping review the ISA Specifications for clarity and offering ideas for improvement.

I have been writing and re-writing RISC++ for over 20 years, all the time studying advances in "real" architectures and trying not to be obsolete before I publish it. Thomas Edison is quoted as saying that all his "failed" experiments were not failures at all. He just found 2000 ways to **NOT** make a light bulb, before he found the one that worked. I do not claim to have found the "perfect light bulb". But I do think this document itself, can be an effective educational tool to learn about the history of computer architecture from the 1980s to the present. It also shows how various High-Level Languages can be compiled into simpler, more efficient machine language code than in current architectures.

Now, in my semi-retirement, I continue working as a part-time substitute teacher, often in Special Education classes. I work with students with Downs Syndrome, Autism, and various kinds of emotional needs because of abuse. I also enjoy volunteering at the City Mission and doing Bible studies with men in the drug and alcohol rehabilitation program. I also substitute teach in the Catholic Diocese of Erie.

I love gardening, hiking, and playing with my grandchildren. I am a Star Trek fan and I like to binge-watch all the various series. My favorite is "Enterprise" and my second favorite is "Deep Space Nine", but I like them all. I enjoy board games, but I'm not as much of a fanatic as my wife. For music, I enjoy the "oldies but goodies", like Simon and Garfunkel, Kenny Rogers, Johnny Cash, John Denver, Peter Paul, and Mary, but also Scott Joplin and Classical. But my favorite music of all is the old Hymns. I'm a pastor/geek who is still stuck in the seventies.




## About This Book

This book is a proposal for an Instruction Set Architecture (ISA) modeled after the C++ language.

The intended audience is people who understand both C++ and Assembler language programming. Familiarity with various CPUs from Intel, AMD, IBM, ARM, Sun, etc. helps but is not essential. I desire to use this document as an educational tool for people studying Computer Science at a College level, and those who are self-taught. I will do my best to explain new ideas as they come up in future chapters. For now, if I use unfamiliar terms, please be patient, I'll explain them later.

This document was written using the Typora Markdown Editor under Windows 10. I also use the <u>Draw.io</u> desktop app for diagrams. The book will be published on [leanpub.com](https://leanpub.com/) and Amazon in PDF, Kindle, and paperback formats. The online formats have the added benefit of being able to follow various links for further education. This includes both text and video links.

I use Wikipedia extensively, but I caution that these articles cannot necessarily be considered authoritative, but only as a starting point for further study. After reading this book and following the links, may I suggest that you donate to Wikipedia for providing such a rich set of relevant online information?

At the time of this book's publication, no hardware implements the RISC++ Architecture. I claim the ideas in this book as my personal intellectual property. I ask that the copyright be honored and that I will be given credit for any of the ideas that are original to me.

Part of me hopes that some of these ideas may be patented and even make me some money. But at present, I rely on the copyright protection of my ideas. I will also make every effort to credit the works of others, and I ask that I be credited with my ideas. I want this document to be used for educational purposes and I want the ideas to be as widespread as possible. In the end, it all comes down to trust.

I would welcome comments and suggestions from experts in various fields. I would greatly appreciate input from people who design compilers for various languages such as C++. I would also welcome comments from hardware designers, from operating system experts, and from people who design critical performance scientific software. I am not an expert in any of those fields, but I have spent a great deal of time studying various computer architectures both at the ISA level and the implementations in hardware. I have built on top of what others have done and tried to streamline and pick the best features.

After reading this book I would encourage people to:

1. Offer suggestions for improvement of the instruction set or the hardware.
2. Help develop an "official" standardized definition of the RISC++ ISA.
3. Work cooperatively to develop the following tools:
	* A macro Assembler to generate RISC++ binary code.
	* A C++ compiler that generates RISC++ Assembler code.
	* A RISC++ emulator, with debug capability.
	* A new C++ math library optimized for RISC++.
4. Develop Verilog code to implement the RISC++ hardware.

I value the input of others, and I want these ideas discussed and evaluated.

Please be kind. This is my baby.

