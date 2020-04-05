# General Assembly Stuff #

## Intel vs. AT&T Syntax ##

- Intel: `OP DEST, SRC`
- AT&T: `OP SRC, DEST`

## Binary ##

- Signed numbers (using two's-complement) use the most significant bit to indicate their sign.
  - So if the most significant nibble is > 7, the value is negative
- One hex digit is four binary digits, which is a nibble (i.e. 0x33 is 0011 0011 in binary) 

## Registers ##

### x86 ###

- `EAX` (Further broken down into)
  - `ax`
    - `ah`
    - `al`
  - Often used for function return values
- `EBX`
  - `bx`
    - `bh`
    - `bl`
- `ECX`
  - `cx`
  - Often used for counting
- `EDX`
- `ESI`
  - Source register for instructions such as `rep`
- `EDI`
  - Used to point to the current memory location for operations which walk along a buffer or similar
  - Destination register for instructions such as `rep`
- `EIP` 
  - Instruction Pointer
  - Always points to the next instruction
  - We don't modify this ourselves
- `ESP` 
  - Stack pointer
  - Points to the "top" (actually the bottom, in memory) of the stack.
- `EBP`
  - Base pointer
  - Points to the "base" (actually the top, in memory) of the stack
- `EFLAGS`
  - Comprises all the flags
  - One bit per flag
  - Combinations of flags can be set by various logical, bitwise, and arithmetic operations
  - The flags can then be used to drive control flow.
  - Flags:
    - `ZF`: Zero flag
    - `OF`: Overflow flag
    - `SF`: Sign flag
    - `CF`: Carry flag
    - `DF`: Direction flag (0 == up, 1 == down)
  - For disassembling/RE/exploit dev, generally not necessary to memorise exactly which ops set which flags and in what ways. If needed, can always consult a manual for that.

## Instructions ##

- `sub`, `add`, `cmp`, etc. apply their operands left-to-right, and store in the left (except `cmp`, which doesn't save its result)
- `cmp` does a subtraction, but only sets flags
- `test` does a *logical* `AND`, but only sets flags
- `jb`/`jbe` are for unsigned values (`jl`/`jge` etc. are for signed values)
- `shr`/`shl` are the shift (logical) right/left instructions. 
  - These do the following: `x = x / (2^y)` and `x = x * (2^y)` respectively, where `x` and `y` are the operands: `shr x, y`.
  - `x` is a register or memory location
  - `y` is an immediate, or the `cx` sub-register
  - Flags (e.g. `CF`) are set when bits drop off either end 
- `lea` (Load Effective Address) puts the value given by the source operand, into the register in the destination operand. 
  - This takes a form like `lea eax, [ebx+8]` (which would load the memory address given by `ebx + 8` into `eax`)
  - This is as opposed to `mov eax, [ebx+8]`, which would load the *value* at `ebx + 8` into `eax`  
- `imul` is a signed multiply
  - Can have either one, two, or three operands:
    - One operand: multiply `eax` by the given r/m32, and store the result in `edx:eax`
    - Two operands: like `add` and `sub`
    - Three operands: `dest = src * third operand`. Third operand must be an immediate.
- `div`: unsigned divide. Two operands.
  - divide `ax` by r/m8, and put the quotient in `al` and the remainder in `ah`
  - divide `edx:eax` by r/m32, and put the quotient in `eax` and the remainder in `edx`
  - If using the second form, must remember to set `edx` to zero beforehand, if dealing with a <= 32-bit number in the numerator. This is because the value in `edx` will get prefixed to that in `eax`.
- `neg`: 2's complement, i.e. take the negative of a value
- `dec`: decrement a register
- `inc`: increment a register 
- `rep X`: repeat `X` until `ecx` reaches zero. After each repetition, increment `edi` by an appropriate amount (determined by the operation being repeated) in the direction given by the `DF` flag, and then decrement `ecx`.
- `stos`: Usually used in conjunction with `rep`. Fills the byte at `[edi]` with data from `al`, or the dword at `[edi]` with data from `eax`
  - `rep stos` often used to prepare a buffer

## Misc ##

- `dword ptr` means we are using the `dword` form of an instruction which has multiple options for size of operand (e.g. `stos`)
  - Similarly with e.g. `byte ptr`.
- Caller-save registers: `eax`, `ecx`, `edx`
- Callee-save: `edi`, `esi`, `ebp`, `ebx`
- `AND <register>, 3` and similar can be used to check for (in this example, 4-byte) aligned addresses.
- `ebp + <value>` takes you above the current stack frame, `ebp + 8` for example is the first parameter passed to the current function (in C, at least - i.e. it is the last param passed onto the stack)
- To quickly `AND` two values in hex, take the minimum of the individual nibbles:
  ```
      0xbbfcd1322
  AND 0xf19cc7367
  =   0xb19cc1322
  ```
  