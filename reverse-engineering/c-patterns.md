# C and C++ Constructs in Assembly

## Conventions

 - Intel syntax
 - Usually, 64-/32-bit registers will be interchangeable; will note when they are not.

## File Formats

### ELF

 - Headers include (but are not limited to) the following info:
        - compiler
        - OS
        - List of sections in the executable
        - 

## Basic Constructs

### Functions

 - GCC adds a function prologue and epilogue:
 ```
 PUSH RBP
 MOV RBP, RSP
 ...
 POP RBP
 ```
 This shifts the stack frame down, and then back up again.
 - Return values go in EAX/RAX