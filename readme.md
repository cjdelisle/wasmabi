# Wasabi - WebAssembly ABI

Status: unmaintained, core concepts are implemented in [wasi](https://wasi.dev/)

This is the plan: https://www.destroyallsoftware.com/talks/the-birth-and-death-of-javascript

[![Build Status](https://travis-ci.org/cjdelisle/wasmabi.svg?branch=master)](https://travis-ci.org/cjdelisle/wasabi)

The idea is to get [WebAssembly](http://webassembly.org/) code to run in Linux as if it were
native code, with full system call support. Then since it is already software bounds-checked,
migrate it into kernelspace because it cannot trash kernel memory. The idea being to put an
end to context switches.

## Todo list
* [x] [Toolchain for building C/C++ to wasm](https://github.com/WebGHC/wasm-cross)
* [x] [libc port to wasm](https://github.com/WebGHC/musl)
* [x] [Syscall handler w/ pointer translation](https://github.com/cjdelisle/wasabi/blob/master/SyscallHandler.c)
* [x] Hello_world.wasm running in nodejs with syscall handler
* [ ] Syscall FFI (rewrite pointers in structures, convert from 32 bit to 64 bit)
* [ ] Demonstrate a busybox shell with standard unix tools compiled to WASM
* [ ] Migrate from nodejs to [WAVM](https://github.com/AndrewScheidecker/WAVM/)
* [ ] Debugging support
* [ ] Run multiple processes in the same OS process
  * [ ] implement `fork()`
* [ ] Kernelspace demo

## See it in action

The objective is to be cross-platform but this probably won't work on anything other than amd64 for now.
Getting this to run on systems other than linux is out of scope.

To reproduce:

```bash
node ./syscall_tbl/syscall.js /usr/src/linux-headers-`uname -r` > ./syscall_autogenerated.h
CC=gcc CFLAGS=-std=c99 npm install --verbose
node ./hello.js
```

Also check the [TravisCI Build](https://travis-ci.org/cjdelisle/wasabi) to see how it works there.
