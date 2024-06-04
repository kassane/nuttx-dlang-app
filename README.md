# nuttx-dlang-app

D Apps for Apache NuttX RTOS and QEMU RISC-V 

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