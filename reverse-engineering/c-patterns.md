# C and C++ Constructs in Assembly

## Conventions uSed in these Notes ##

 - Intel syntax
 - Usually, 64-/32-bit registers will be interchangeable; will note when they are not.

## File Formats ## 

### ELF ###

 - Headers include (but are not limited to) the following info:
    - compiler
    - OS
    - List of sections in the executable 

## Basic Constructs ##

### Functions ###

 - GCC adds a function prologue and epilogue:
   ```
   PUSH RBP
   MOV RBP, RSP
   ...
   POP RBP
   ```
   This shifts the stack frame down, and then back up again.
 - `ret` instruction has an optional parameter indicating the number of butes that should be popped when the function returns
 - Calling conventions:
   - `cdecl`: Caller cleans up parameters:
     ```
     push       eax
     call       someFunc
     add        esp, 4
     ```
     Is roughly what this would look like; i.e. setting up function parameters, calling the function, then cleaning the stack by shifting `esp` up.
     - Parameters are passed right-to-left on the stack
       - i.e. the right-most parameter is first onto the stack
     - This setup allows for `cdecl` to support variadic functions (i.e. with variable-length arg lists)
     - Compiler usually prepends an underscore to `cdecl` functions (e.g. `_SomeFunction`)
     - Return values go in `eax`
   - `stdcall` is the opposite; the callee (the called function, that is) cleans up its own mess, as it were.
     - This is the standard calling convention (as the name implies) across the Win32 API
     - Args are passed right-to-left
     - Return values passed in `eax`
     - The fact that the callee cleans up the arguments means that `stdcall` does not support variadic functions
     - Uses the `ret` instruction's optional argument to ensure the correct number of bytes are popped when returning
     - Compiler decorates the function names with `_`, `@`, and the number (in bytes) of params passed on the stack (e.g. `_SomeOtherFunc@4`). 
   - `fastcall` is non-standardised across compilers, so it should be used with caution
     - The first two or three 32-bit (or smaller, on a 32-bit machine; 64-bit on x86_64) are passed in registers (usually `edx`, `eax`, and `ecx`).
     - Larger, or additional, arguments are passed on the stack
     - Stack arguments are usually passed right-to-left
     - Usually the caller cleans up the stack
     - The name is decorated with a leading and trailing `@`, and the number (in bytes) of args passed (e.g. `@AThirdFunction@8`)
     - `fastcall` may not create a stack frame, if one is unnecessary and the compiler is optimising.
 - You can have caller-save, and callee-save:
   - Caller-save:
        ```
        push    ecx
        call
        pop     ecx
        ```
   - Callee-save has the `push`/`pop` within the called function itself.

## Memory Management ##

- When allocating a buffer on the stack, the compiler might over-allocate, and then fill the whole allocation with something like `0xC` or other interrupt opcode.
  - This creates stack guards above and below the actual buffer, and initialises the buffer to something that will cause an interrupt (which can be caught and handled).
  - The stack guards can then be checked for integrity once the buffer is no longer in use
  - Note that these kinds of stack guards are not for security (you'd want randomised values, not a constant like `0xC`, for that)