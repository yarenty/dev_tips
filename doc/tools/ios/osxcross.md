# OSX Cross

https://github.com/tpoechtrager/osxcross


The goal of OSXCross is to provide a well working macOS cross toolchain for
Linux, FreeBSD, OpenBSD, and Android (Termux).

OSXCross works on x86, x86_64, arm and AArch64/arm64,
and is able to target arm64, arm64e, x86_64, x86_64h and i386.

arm64 requires macOS 11.0 SDK (or later).
arm64e requires a recent Apple clang compiler.

There is also a ppc test branch that has recently seen some daylight.

HOW DOES IT WORK?
For cross-compiling for macOS you need

the Clang/LLVM compiler
the cctools (lipo, otool, nm, ar, ...) and ld64
the macOS SDK.
Clang/LLVM is a cross compiler by default and is now available on nearly every Linux distribution, so we just need a proper port of the cctools/ld64 and the macOS SDK.

OSXCross includes a collection of scripts for preparing the SDK and building the cctools/ld64.

It also includes scripts for optionally building

Clang using gcc (for the case your distribution does not include it),
an up-to-date vanilla GCC as a cross-compiler for target macOS,
the "compiler-rt" runtime library, and
the llvm-dsymutil tool required for debugging.
