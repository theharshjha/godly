# Godly

## CH2

* The first step is __*scanning or (lexing, lexical analysis)*__, it takes in stream of characters and identifies tokens.
* The second step is parsing, it takes in sequence of __*tokens*__ and builds a __*parese tree or (abstract syntax tree - AST)*__  that mirrors the nested nature of language grammer.
* The initial stages of language implementation involve understanding the syntactic structure, such as nesting of expressions, without knowing variable details. The next step, called binding or resolution, identifies where each identifier is defined, linking names to declarations based on scope. For statically typed languages, this stage also involves type checking to ensure variable types support the intended operations. In dynamically typed languages, type checking occurs at runtime. Semantic information from this analysis is stored in several ways: as attributes on syntax tree nodes, in lookup tables (symbol tables), or by transforming the syntax tree into a new data structure to better represent the code's semantics.
* Intermediate representations (IRs) play a crucial role in the compilation process by serving as a bridge between the source language and the target architecture. The compiler can be seen as a pipeline, where each stage organizes the user's code to simplify the next stage. The front end of this pipeline is tailored to the source language, while the back end focuses on the final architecture.<br/><br/>The IR is a form of code representation that is not tightly coupled with either the source or target language, acting as an interface between them. Well-known IR styles include control flow graphs, static single-assignment, continuation-passing style, and three-address code.
<br/><br/>Using IRs allows for greater efficiency in supporting multiple source languages and target platforms. Instead of writing separate compilers for each source-to-target combination, one can write a front end for each source language to produce the IR, and a back end for each target architecture to generate the native code. This approach drastically reduces the effort required to support various language-architecture combinations.<br/><br/>For example, GCC supports numerous languages and architectures by using IRs like GIMPLE and RTL. Language front ends convert source code into these IRs, and target back ends transform the IRs into native code for specific architectures, such as Motorola 68k.
* Once we understand a user's program, we can optimize it by replacing it with a more efficient version that retains the same semantics. One basic optimization technique is constant folding, where expressions that always evaluate to the same value are precomputed at compile time.
* **Code Generation**  
 After applying all possible optimizations to the user's program, the final step is code generation. This process converts the optimized code into a form the machine can execute, typically primitive, assembly-like instructions that a CPU understands. At this stage, the compiler transitions from high-level representations to more primitive forms, resembling a descent to something the machine can comprehend.
  
  A crucial decision in code generation is whether to produce instructions for a real CPU or a virtual one. Generating real machine code results in an executable that the OS can load directly onto the chip, offering high performance but requiring significant effort due to the complexity of modern architectures. Additionally, machine code ties the compiler to a specific architecture, limiting portability across different devices (e.g., x86 vs. ARM).  
  
  To address portability issues, some early computer scientists, like Martin Richards and Niklaus Wirth, designed their compilers to produce virtual machine code, also known as bytecode. This code targets a hypothetical, idealized machine rather than a specific real-world CPU. Bytecode instructions are more closely aligned with the language's semantics and less dependent on the quirks of particular hardware architectures. This approach facilitates portability and simplifies the compilation process, making it easier to support multiple platforms.

* **Virtual Machine**  
 Once your compiler generates bytecode, the next step is to translate it into native code. You have two main approaches:
  1. **Target-Specific Compilers**: Create a mini-compiler for each target architecture that converts bytecode into native code. This method allows you to reuse the compiler pipeline across different platforms, but you still need to handle architecture-specific details like register allocation and instruction selection.
  2. **Virtual Machine (VM)**: Develop a VM that emulates a hypothetical architecture supporting your bytecode. This VM interprets the bytecode at runtime, offering portability across platforms. For example, a VM written in C can run on any system with a C compiler, but interpreting bytecode at runtime is generally slower than executing native code directly.
 
  The choice between these methods involves balancing performance and portability. VMs offer simplicity and broad compatibility but may introduce runtime overhead. Additionally, "virtual machine" can refer to system virtual machines, which emulate entire hardware and OS environments for functionalities like running multiple operating systems on a single physical machine or providing cloud-based virtual servers. In this context, the focus is on language virtual machines, which execute bytecode for specific programming languages.
* **Runtime**
  
  After compiling a program, the final step is execution. For machine code, you instruct the operating system to load and run the executable. For bytecode, you must start a virtual machine (VM) to interpret the bytecode.
  
  Regardless of the execution method, most languages require runtime services. These might include memory management, like garbage collection, or type tracking for features such as "instance of" tests. This support is collectively known as the runtime.
  
  In fully compiled languages, the runtime code is embedded directly into the executable. For example, Go includes its runtime within each compiled application. In languages run within an interpreter or VM, such as Java, Python, or JavaScript, the runtime is part of the interpreter or VM itself, providing necessary services during execution.
* **Single-Pass Compilers**
 
  Single-pass compilers perform parsing, analysis, and code generation in one go, producing output directly during parsing without using intermediate representations like syntax trees. This design approach limits language features because the compiler processes each part of the code only once and must have all the necessary information available immediately.  
 
  In single-pass compilers, a technique called syntax-directed translation is used. This involves associating actions with grammar rules to generate output code directly. As the parser matches syntax rules, it executes corresponding actions to build the target code incrementally.
  
  Languages like Pascal and C were designed with these limitations in mind. For instance, Pascal requires type declarations to precede their usage within a block, and C mandates explicit forward declarations for functions used before their definitions. These constraints were influenced by the need to optimize memory usage when compilers had limited resources and could not hold entire source files in memory.
* Transpilers  
  Transpilers, also known as source-to-source compilers, offer an alternative approach to writing a complete compiler back end. Instead of translating directly to machine code or another low-level form, a transpiler generates source code in another high-level language that has existing compilation tools. This method leverages the compilation infrastructure of the target language to simplify the process of producing executable code.
  
  The process involves:
  
  1. Writing a Front End: Create a front end for your language to parse and analyze the source code.
  2. Generating Target Code: Instead of producing machine code, the back end of the transpiler outputs source code in a target language that is roughly as high-level as the original language.
  3. Using Existing Tools: The generated source code is then processed by the existing compilation tools of the target language, which handle the final compilation steps.
  
  The term "transpiler" became popular with the rise of languages that compile to JavaScript for browser execution. For instance, many languages now compile to JavaScript, which acts as a sort of universal "machine code" for web browsers. Additionally, WebAssembly has introduced a new lower-level language that transpilers can target for more efficient web execution.
  
  Historically, the first transcompiler, XLT86, translated 8080 assembly code to 8086 assembly code. It performed data flow analysis to map registers between different chip architectures. The concept has evolved, with modern transpilers targeting higher-level languages and focusing on compatibility and efficiency.

* Just-in-time (JIT) compilation is a technique used to improve code execution efficiency by compiling it to machine code at runtime, tailored to the user's machine architecture. Unlike precompiled code, JIT compiles code on-the-fly when a program is loaded, such as in JavaScript or platform-independent bytecode for the Java Virtual Machine (JVM) and Common Language Runtime (CLR). 
  
  The process involves:
  
  1. **Initial Compilation**: Source code or bytecode is initially interpreted or compiled to an intermediate form.
  2. **Runtime Compilation**: When the program runs, the JIT compiler translates this intermediate form into native machine code specific to the user’s hardware.
  
  Advanced JIT compilers include profiling mechanisms to monitor code execution and identify performance-critical areas, or "hot spots." Over time, these hot spots are recompiled with optimizations to enhance performance. This dynamic optimization is a key feature of sophisticated JIT systems, such as the HotSpot JVM, which is named for its ability to optimize these frequently executed code regions.

* **Compilers vs. Interpreters**
  1. **Compiler**:
     - **Function**: Translates source code into another form, often lower-level code like machine code or bytecode.
     - **Process**: The entire code is translated before execution. The output is an executable or bytecode that must be run separately.
     - **Example**: GCC and Clang for C, which compile code into machine code. The user runs the compiled executable directly.

  2. **Interpreter**:
     - **Function**: Executes source code directly, parsing and running it line-by-line or statement-by-statement.
     - **Process**: Executes code directly from its source form without generating a separate executable.
     - **Example**: Older Ruby implementations, which parsed and executed Ruby code directly.

  3. **Hybrid Approach**:
     - **Compiling and Interpreting**: Some systems combine both techniques.
     - **Example**: CPython, the standard Python interpreter, converts Python code to bytecode (compiling) and then executes it within a virtual machine (interpreting). This makes CPython both an interpreter and a compiler.
     - **Go Tool**: Offers dual functionality; `go build` compiles code to machine code without execution, while `go run` compiles and executes in one step.
  
  In essence, while some languages and tools fit neatly into either the compiler or interpreter category, many modern implementations blend both approaches, showcasing the overlap in functionality.

