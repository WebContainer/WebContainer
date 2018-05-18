# WebContainer

## Goals

Unclear, but important items are:

1. compile once, run anywhere
2. secure against untrusted code
3. functional and useful
4. fits a reasonable performance profile

## Authors

- Jacob Groundwater ([@groundwater](https://github.com/groundwater))
- Jeremy Apthorp ([@nornagon](https://github.com/nornagon))

## Status

- [ ] LLVM Toolchain
  - [ ] WASM-compatible musl-libc
    - [ ] `libclang_rt.builtins_wasm32.a`
      - currently compiling _builtins_ as part of `libc.a`, since all WASM targets are identical
    - [ ] Dockerized built tools
      - Would like to have a docker environment that can easily build a c/c++ project
- [ ] WebContainer
  - [ ] Mojo IPC
    - [x] `open/read/close/readv/writev`
    - [ ] `brk/sbrk`
  - [ ] One Security Policy
    - [ ] CoW Access to FileSystem
    - [ ] Threaded `fork()`
      - Calling `fork()` creates new WebAssembly container in another thread.

## Notes

- WebAssembly does not play well with asynchronous JavaScript. Most C/C++ programs, especially _libc_ programs rely heavily on _blocking_ calls e.g. `read()/open()/write()`. It is not possible to implement these calls with asynchronous operations in JavaScript because `read()` in your C program ends up being a JavaScript frame, and _must_ unwind before any async operations can resolve. The consequence of this seems to mean _blocking_ in C requires _blocking_ the underly OS thread.
- Dynamic linking does not seem possible. The WebAssembly instnace _must_ resolve all symbols at instantiation.
- It's unclear how `fork()` could work without being able to copy the execution stack, which is not accessible.

## Wants

It would be nice if concepts like threadding, forking, signals, etc, could entirely be implemented by the JavaScript sandbox, and not require underlying OS support. Having OS support would be a nice optimization when required.

## Other Projects / Inspirations / References

- https://github.com/cjdelisle/clang-wasm-toolchain
- https://github.com/jfbastien/musl
- https://github.com/yurydelendik/wasmception
- https://github.com/kripken/musl-emscripten
