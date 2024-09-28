# Valgrind

Valgrind is a GPL'd system for debugging and profiling Linux programs. With Valgrind's tool suite you can automatically detect many memory management and threading bugs, avoiding hours of frustrating bug-hunting, making your programs more stable. You can also perform detailed profiling to help speed up your programs.

## Why should you use Valgrind?
- Valgrind will save you hours of debugging time. With Valgrind tools you can automatically detect many memory management and threading bugs. This gives you confidence that your programs are free of many common bugs, some of which would take hours to find manually, or never be found at all. You can find and eliminate bugs before they become a problem.
- Valgrind can help you speed up your programs. With Valgrind tools you can also perform very detailed profiling to help find bottlenecks in your programs.
- Valgrind is free. Free-as-in-speech: you can download it, read the source code, make modifications, and pass them on, all within the limits of the GNU GPL. And free-as-in-beer: we aren't charging for it.
- Valgrind runs on several popular platforms, such as x86/Linux, AMD64/Linux and PPC32/Linux. Valgrind works with all the major Linux distributions, including Red Hat, SuSE, Debian, Gentoo, Slackware, Mandrake, etc.
- Valgrind is easy to use. Valgrind uses dynamic binary instrumentation, so you don't need to modify, recompile or relink your applications. Just prefix your command line with valgrind and everything works.
- Valgrind is not a toy. Valgrind is first and foremost a debugging and profiling system for large, complex programs. We have had feedback from users working on projects with up to 25 million lines of code. It has been used on projects of all sizes, from single-user personal projects, to projects with hundreds of programmers.
- Valgrind is suitable for any type of software. Valgrind has been used with desktop applications, libraries, databases, games, web browsers, network servers, distributed control systems, virtual reality frameworks, transaction servers, compilers, interpreters, virtual machines, telecom applications, embedded software, medical imaging, scientific programs, signal processing programs, video/audio programs, business intelligence software, financial/banking software, operating system daemons, etc, etc. See a list of projects using Valgrind.
- Valgrind is widely used. Valgrind has been used by thousands of programmers across the world. We have received feedback from users in over 30 countries.
- Valgrind works with programs written in any language. Because Valgrind works directly with program binaries, it works with programs written in any programming language, be they compiled, just-in-time compiled, or interpreted. The Valgrind tools are largely aimed at programs written in C and C++, because programs written in these languages tend to have the most bugs! But it can, for example, be used to debug and profile systems written in a mixture of languages. Valgrind has been used on programs written partly or entirely in C, C++, Java, Perl, Python, assembly code, Fortran, Ada, and many others.
- Valgrind gives 100% coverage of user-space code, even within system libraries. You can even use Valgrind on programs for which you don't have the source code.
- Valgrind is extensible. Anyone can write powerful new tools that add arbitrary instrumentation to programs. This is much easier than writing such tools from scratch. This makes Valgrind ideal for experimenting with new kinds of program analysis tools. It has been used for research purposes by people at the following universities: Cambridge, MIT, UC Berkeley, UC Santa Barbara, Carnegie Mellon, Cornell, University of New Mexico, Australian National University, University of Melbourne, TU Muenchen (Munich) and Graz University of Technology.
- Valgrind is actively maintained. The Valgrind developers are constantly working to fix bugs, improve Valgrind, and ensure it works as new Linux distributions and libraries come out. There are also mailing lists you can subscribe to, and contact if you're having problems.
- So what's the catch? The main one is that programs run significantly more slowly under Valgrind. Depending on which tool you use, the slowdown factor can range from 5--100. This slowdown is similar to that of similar debugging and profiling tools. But since you don't have to use Valgrind all the time, this usually isn't too much of a problem. The hours you'll save debugging will more than make up for it.

## When should you use Valgrind?
It depends on your exact needs. Here are some examples of when people use Valgrind's bug-detecting tools.

- All the time. For small programs with short run-times, when developing you can always run the program under a Valgrind tool (usually Memcheck), knowing that memory bugs will be found immediately.
- In automatic testing. By using Valgrind tools in your automatic unit, integration, system, or regression test, you can be confident no code will be unchecked.
- After big changes. To ensure new bugs haven't been introduced in the new code.
- When a bug occurs. Get instant feedback about what the bug is, where it occurred, and why.
- When a bug is suspected. Is your program behaving oddly? Use a Valgrind tool to discover if a bug is the cause.
- Before a release. To give you confidence that your new release is as stable and bug-free as possible.
- As for Valgrind's profiling tools, use those whenever you want information about how your program is spending its time, or you want to speed it up.

## MAC:

