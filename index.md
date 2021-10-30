## NASM Tutorial on macos

This page contains several notes on learning assembly programming with NASM.


macos / LLDB Cheat Sheet

* show all strings
  $ strings <binary>

* show symbol tables
  $ nm <binary>
  or
  (lldb) image dump symtab <binary>

## Tutorials and Documentation
  
## Using NASM on macos
  
The following applies to Big Sur / macos 11.4, possibly also to earlier and/or later versions.

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
