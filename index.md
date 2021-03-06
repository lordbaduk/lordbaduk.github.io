**Table of Contents**

- [NASM on macos](#nasm-on-macos)
  - [x86-64 Documentation](#x86-64-documentation)
  - [System Calls](#system-calls)
  - [Linking](#linking)
- [Useful Links](#useful-links)
  - [Link Collections](#link-collections)
  - [Tutorials](#tutorials)
  - [macos specific information](#macos-specific-information)


# NASM on macos

## x86-64 Documentation

x86-64 architecture documentation can be found here:
[Intel 64 and IA-32 Architectures Software Developer’s Manuals](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html)


## System Calls

Get the current XNU kernel sources from here: [https://opensource.apple.com](https://opensource.apple.com).

Find the list of system calls and their ID in the kernel source code, in file `bsd/kern/syscalls.master`.

Register usage is determined by the [x86-64 System V ABI](https://gitlab.com/x86-psABIs/x86-64-ABI) (see 3.2.3 Parameter Passing, Figure 3.4 Register Usage). Note that 32bit uses a different convention.

For documentation on individual system calls, check out section 2 of the manual pages, e.g.:

```
$ man 2 write
```


## Linking

When calling the linker (`ld`) outside Xcode, the system libraries are not in the library path:
  
```
$ nasm -f macho64 01_hello_world.asm
$ ld 01_hello_world.o
ld: dynamic main executables must link with libSystem.dylib for architecture x86_64
$ ld -lSystem 01_hello_world.o
ld: library not found for -lSystem
```

Copies of the system libraries are no longer present on the file system but stored in a cache; see [Big Sur Release Notes](https://developer.apple.com/documentation/macos-release-notes/macos-big-sur-11_0_1-release-notes).

Depending on the specific SDK you are working with, the necessary directory must be given to ld explicitly:

```
$ ld -L /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/lib -lSystem 01_hello_world.o
```

Also note that due to the "System Integrity Protection (SIP)" feature of macos, the LD_LIBRARY_PATH environment variable cannot be changed, so the library path must be given to ld via command line. Alternatively, the SIP feature must be disabled.


# Useful Links

## Link Collections

* [osx-re-101](https://github.com/michalmalik/osx-re-101)

## Tutorials

* [NASM Tutorial](https://cs.lmu.edu/~ray/notes/nasmtutorial)
* [ASM Tutor](https://asmtutor.com)
  see also the macos adapted code at [Learning NASM](https://github.com/lordbaduk/learning-nasm)

## macos specific information

* [Writing 64 Bit Assembly on Mac OS X](http://www.idryman.org/blog/2014/12/02/writing-64-bit-assembly-on-mac-os-x)
* [System Call Path](https://gist.github.com/yrp604/23e86dce9ca12bf514ef)
