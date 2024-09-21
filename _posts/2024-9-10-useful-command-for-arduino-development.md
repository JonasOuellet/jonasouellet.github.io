This is a list of useful commands that can be used while developing for arduino with rust or c++.  Most of the command come from this [video](https://youtu.be/q8irLfXwaFM).

To demonstrate how the commands work, I have compiled the **hello world** of embeded programming (onboard led blinking) obtained with the `cargo generate --git https://github.com/Rahix/avr-hal-template.git` command.

## file

Determine type of files.  This command print out useful information about a file.

```bash
❯  file ./target/avr-atmega328p/debug/hello_world.elf
./target/avr-atmega328p/debug/hello_world.elf: ELF 32-bit LSB executable, Atmel AVR 8-bit, version 1 (SYSV), statically linked, with debug_info, not stripped
```


## hd

Display file contents in hexadecimal, decimal, octal, or ascii.

Note here that we pipe the output of the hd command to **head**.  This will print only the first line.

```bash
❯  hd ./target/avr-atmega328p/debug/hello_world.elf | head -n 1
```

## objdump

Display information from object <file(s)>.

Note that in our case, since we are working with avr file we should use **avr-objdump** instead.


```bash
❯ avr-objdump -d target/avr-atmega328p/debug/hello_world.elf

target/avr-atmega328p/debug/hello_world.elf:     file format elf32-avr


Disassembly of section .text:

00000000 <__vectors>:
   0:   0c 94 34 00     jmp     0x68    ; 0x68 <__ctors_end>
   4:   0c 94 46 00     jmp     0x8c    ; 0x8c <__bad_interrupt>
   8:   0c 94 46 00     jmp     0x8c    ; 0x8c <__bad_interrupt>
   c:   0c 94 46 00     jmp     0x8c    ; 0x8c <__bad_interrupt>
  10:   0c 94 46 00     jmp     0x8c    ; 0x8c <__bad_interrupt>

```

## ldd

Display information about dynamic linking.

## readelf

Display information about the contents of ELF format files

```bash
❯ readelf -h target/avr-atmega328p/release/hello_world.elf
ELF Header:
  Magic:   7f 45 4c 46 01 01 01 00 00 00 00 00 00 00 00 00 
  Class:                             ELF32
  Data:                              2's complement, little endian
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI Version:                       0
  Type:                              EXEC (Executable file)
  Machine:                           Atmel AVR 8-bit microcontroller
  Version:                           0x1
  Entry point address:               0x0
  Start of program headers:          52 (bytes into file)
  Start of section headers:          42560 (bytes into file)
  Flags:                             0x5, avr:5
  Size of this header:               52 (bytes)
  Size of program headers:           32 (bytes)
  Number of program headers:         3
  Size of section headers:           40 (bytes)
  Number of section headers:         15
  Section header string table index: 12

 
```

