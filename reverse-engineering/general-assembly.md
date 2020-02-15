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
- 