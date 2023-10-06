# Brainfuck

http://www.muppetlabs.com/~breadbox/bf/

_An Eight-Instruction Turing-Complete Programming Language_

Brainfuck is the ungodly creation of Urban Müller, whose goal was apparently to create a Turing-complete language for which he could write the smallest compiler ever, for the Amiga OS 2.0. His compiler was 240 bytes in size. (Though he improved upon this later -- he informed me at one point that he had managed to bring it under 200 bytes.)

I originally started playing around with Brainfuck because of my own interest in writing very small programs for x86 Linux. I also used it as a vehicle for writing a program that created ELF files. Eventually, however, I too succumbed to the Imp of the Perverse and wrote some actual Brainfuck programs of my own.

The Language
A Brainfuck program has an implicit byte pointer, called "the pointer", which is free to move around within an array of 30000 bytes, initially all set to zero. The pointer itself is initialized to point to the beginning of this array.

The Brainfuck programming language consists of eight commands, each of which is represented as a single character.

```
> 	Increment the pointer.
< 	Decrement the pointer.
+ 	Increment the byte at the pointer.
- 	Decrement the byte at the pointer.
     . 	Output the byte at the pointer.
     , 	Input a byte and store it in the byte at the pointer.
     [ 	Jump forward past the matching ] if the byte at the pointer is zero.
     ] 	Jump backward to the matching [ unless the byte at the pointer is zero.
     The semantics of the Brainfuck commands can also be succinctly expressed in terms of C, as follows (assuming that p has been previously defined as a char*):

> 	becomes 	++p;
< 	becomes 	--p;
+ 	becomes 	++*p;
- 	becomes 	--*p;
     . 	becomes 	putchar(*p);
     , 	becomes 	*p = getchar();
     [ 	becomes 	while (*p) {
     ] 	becomes 	}
     
```     
## Resources
   
- http://www.muppetlabs.com/~breadbox/software/tiny/bf.asm.txt -   A Brainfuck compiler for Linux. This is the final result of my attempt at the tiny-compiler challenge. The executable is 166 bytes in size.
- http://www.muppetlabs.com/~breadbox/bf/quine.b.txt - My own Brainfuck programs include an 822-byte quine, and factor, a program that factors arbitrarily large integers in an arbitrarly large amount of time — and what may have been, at the time that I wrote it, the largest Brainfuck program in existence.
- http://www.muppetlabs.com/~breadbox/bf/standards.html - Portable Brainfuck. My informal standards guide for programmers and implementers alike.
- http://www.muppetlabs.com/~breadbox/software/elfkickers.html -  My ELF Kickers package contains several programs that deal with the ELF file format. Included is ebfc, a real Brainfuck compiler (compiles executables and object files).
     
- https://catseye.tc/article/Language_Implementations#pibfi - pibfi. Cat's Eye Technologies provides pibfi, the Platonic Ideal Brainfuck Interpreter. This interpreter can emulate most if not all other interpreters.
     
- http://esoteric.sange.fi/brainfuck/ - The Brainfuck archive. Panu Kalliokoski provides a collection of Brainfuck compilers, interpreters, preprocessors, and of course extant Brainfuck programs.
- http://en.wikipedia.org/wiki/Brainfuck - Wikipedia's entry on Brainfuck. The entry contains a in-depth description of the language and several basic algorithms, as well as a list of derivative languages.






# Rust - brain

https://github.com/brain-lang/brain

brain is a strongly-typed, high-level programming language that compiles into brainfuck. Its syntax is based on the Rust programming language (which it is also implemented in). Though many Rust concepts will work in brain, it deviates when necessary in order to better suit the needs of brainfuck programming.

brainfuck is an esoteric programming language with only 8 single-byte instructions: +, -, >, <, ,, ., [, ]. These limited instructions make brainfuck code extremely verbose and difficult to write. It can take a long time to figure out what a brainfuck program is trying to do. brain makes it easier to create brainfuck programs by allowing you to write in a more readable and understandable language.

The type system makes it possible to detect a variety of logical errors when compiling, instead of waiting until runtime. This is an extra layer of convenience that brainfuck does not have. The compiler takes care of generating all the necessary brainfuck code to work with the raw bytes in the brainfuck turing machine.

The brain programming language compiles directly into brainfuck. The generated brainfuck code can be run by a brainfuck interpreter. brain only targets this interpreter which means that its generated programs are only guaranteed to work when run with that. The interpreter implements a brainfuck specification specially designed and written for the brain programming language project.



# Algorithms


https://esolangs.org/wiki/Brainfuck_algorithms

