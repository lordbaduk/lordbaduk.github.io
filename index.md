# NASM on macos

This page collects various information on assembly programming on macos, essentially my learning notes.

## Tips

### Linking

When calling the linker (`ld`) outside Xcode, the system libraries are not in the library path:
  
```
$ ld hello_world.o
ld: dynamic main executables must link with libSystem.dylib for architecture x86_64
```

Copies of the system libraries are no longer present on the file system but stored in a cache:
[https://developer.apple.com/documentation/macos-release-notes/macos-big-sur-11_0_1-release-notes]

Depending on the specific SDK you are working with, the necessary directory must be given to ld explicitly:

```
$ ld -L /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/lib hello_world.o
```

Also note that due to the "System Integrity Protection (SIP)" feature of macos, the LD_LIBRARY_PATH environment variable cannot be changed, so the library path must be given to ld via command line. Alternatively, the SIP feature must be disabled.

## Documentation

* [syscall list](https://github.com/opensource-apple/xnu/blob/master/bsd/kern/syscalls.master)
* [syscall classes](http://dustin.schultz.io/mac-os-x-64-bit-assembly-system-calls.html)
* [syscall register usage](https://courses.cs.washington.edu/courses/cse378/10au/sections/Section1_recap.pdf)

## ASMTutor programs on macos

see (https://asmtutor.com/)

## Other sample programs


## Useful links

* (NASM Tutorial)[https://cs.lmu.edu/~ray/notes/nasmtutorial/]


# Software Reverse Engineering on macos


## LLDB cheat sheet

* show all strings
```
$ strings <binary>
```

* show symbol tables
```
$ nm <binary>
```
or
```
(lldb) image dump symtab <binary>
```
