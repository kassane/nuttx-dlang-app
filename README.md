# D Apps for Apache NuttX RTOS and QEMU RISC-V 

### upstream PR: https://github.com/apache/nuttx/pull/12457 [**MERGED**]

Inspired in [Rust Apps for Apache NuttX RTOS and QEMU RISC-V](https://github.com/lupyuen/nuttx-rust-app) by [Lup Yuen Lee](https://github.com/lupyuen).

Read https://lupyuen.github.io/articles/rust3

## How to work

- **Get nuttx and nuttx-apps**
```bash
git clone --depth=1 --recursive https://github.com/apache/nuttx -b nuttx-12.5.1
git clone --depth=1 --recursive https://github.com/apache/nuttx-apps -b nuttx-12.5.1 apps
# optional (kconfig-frontends) build or use your distro pkg-manager
git clone  --depth=1 https://bitbucket.org/nuttx/tools.git
```

- **apply patches**
- **build nuttx**
- **Output**
```bash
qemu-system-riscv32 \
    -semihosting \
    -M virt,aclint=on \
    -cpu rv32 -smp 8 \
    -bios none \
    -kernel nuttx -nographic
nsh> hello_d
Hello World, [generic-rv32]!
hello_d_main: Saying hello from the dynamically constructed instance
DHelloWorld.HelloWorld: CONSTRUCTION FAILED!
hello_d_main: Saying hello from the instance constructed on the stack
DHelloWorld.HelloWorld: Hello, World!!
```

## Notes

During some tests you may discover druntime (if used) and the compiler builtin require libunwind.
There are some common errors to work around (not just in rv32/64).

- need `static` keyword (TLS) - use `--emulated-tls`.
- like C/C++ custom `#ifdef/ifndef FOO` (`-DFOO=1`) to `Version(Foo)` (`--d-version=Foo`)
- verbose/tips keywords:
    - `-vgc`: list all gc allocations including hidden ones (if use DRT/GC alloc)
    - `-vtls`: list all variables going into TLS (thread local storage)
    - `-errors=context`: show error messages with the context of the erroring source line (show LoC error)
```diff
--- a/examples/hello_d/Makefile
+++ b/examples/hello_d/Makefile
@@ -26,7 +26,10 @@ MAINSRC = hello_d_main.d
 
 # hello_d built-in application info
 
-DFLAGS += -oq -betterC
+DFLAGS += -oq -betterC -vtls --emulated-tls
+DFLAGS += -verrors=context -O
+DFLAGS += --d-version=NuttX_D_Initialize
```

### NuttX dlang-app supports betterC mode only?

Official DRT(D runtime) isn't ideal for embedded (particularly microcontrollers). However, it's possible make your own mini/tiny DRT, like:
- https://github.com/KitsunebiGames/tinyd-rt (used on [D_on_Dreamcast](https://dreamcast.wiki/D_on_Dreamcast))
- [How to work custom Druntime - by Adam D. Ruppe](http://dpldocs.info/this-week-in-d/Blog.Posted_2020_08_10.html#druntime) - [my fork updated](https://github.com/kassane/wasmd)
