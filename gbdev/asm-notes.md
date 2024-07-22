# assembly notes (for RGBASM)

I am following this tutorial: https://gbdev.io/gb-asm-tutorial

- assembly is a line-based language
- each line can contain one of two things
    - directive (tells RGBASM to do something)
    - instruction (written directly into the ROM)
- binary values are prefixed with `%` and hex values with `$`

## instructions
- made up of a _mnemonic_ and _operands_
- example is `ld` ("load") which copies data from its right operand into its left operand
```
; this copies the value 0 into the 8-bit register a (and this line is a comment i.e. everything after the semicolon)
ld a, 0
```
- `ds` is used for statically allocating memory
```
ds $150 - @, 0 ; the first part is how many bytes to reserve (this is an expression) and the second is what to set each byte to
```

## directives
- intended for the assembler itself
- `INCLUDE` for example includes a file (same effect as copy-pasting the whole thing into the code)
- `.inc` files are usually INCLUDE-ed
- `.inc` files define constants, which are names with a value attached
- when you write the name of a constant, it is replaced with its value

## sections
- represent contiguous ranges of memory
- you can ask RGBLINK to generate a map file to see where the named sections end up:
```
rgblink hello-world.o -m hello-world.map
```
- you declare a new section in code for bits that all go together topically:
```
SECTION "Header", ROM0[$100]
```
- this example has the name "Header" and ROM0 indicates which memory type this section belongs to
- the $100 part: some memory locations are special, and so sometimes we need a specific section to span a specific range of memory. To enable this, RGBASM provides the `[addr]` syntax, which forces the section’s starting address to be `addr`

## about memory
- The Game Boy CPU has seven 8-bit General Purpose Registers: a, b, c, d, e, h, and l. “8-bit” means that they store 8 bits. Thus, they can store integers from 0 to 255 (%1111_1111 aka $FF).
- Memory stores numbers (8-bit on the game boy)
- Memory is made of "cells" that store current (used to encode binary numbers)
- To uniquely identify each cell, it's given a number (an address) 0, 1, 2, etc ...
- Since the game boy has several chips, there are several places with memory (addresses)
- there are two types of memory: ROM (cannot write to) and RAM (can write to)
- addresses dealt with by the CPU do not directly correspond to the chips’ addresses, we talk about logical addresses (here, the CPU’s) versus physical addresses (here, the chips’), and the correspondence is called a memory map
- we work with logical addresses since we program for the CPU
- the chip selector is tasked with translating the CPU’s addresses into chip + its physical address to get the logical address
- here is the game boy memory map: https://gbdev.io/pandocs/Memory_Map.html

## header
- the region of memory from $0104 to $014F (inclusive). It contains metadata about the ROM, such as its title, Game Boy Color compatibility, size, two checksums, and the Nintendo boot logo
- a small program called the boot ROM, burned within the CPU’s silicon, is “overlaid” on top of our ROM. it is responsible for the startup animation, but it also checks the ROM’s header. Specifically, it verifies that the Nintendo logo and header checksums are correct

## arithmetic
- operations: `add` `sub` `inc` `dec`
- `cp` is compare, and works like `sub` but discards the result instead of writing it back
- `cp` updates flags,  though

### flags
- the Z flag gets set when an operations result is 0 and gets reset otherwise
- the C flag gets set when an operation overflows or underflows (registers are 8 bit and can only store a limited range of integers)