# Cortex-M4 Bare-Metal Rust Sandbox 🦀⚡
A zero-cost, hardware-free embedded development workspace running pure `no_std` Rust on emulated ARM Cortex-M4 processors via QEMU.

## 🚀 Overview
This repository is a modular Cargo Workspace designed to learn, experiment, and build professional bare-metal firmware for ARM Cortex-M microcontrollers (such as the Nordic nRF52833 / micro:bit v2 architecture) without requiring physical hardware.

By leveraging QEMU, every project inside this monorepo runs entirely in software, allowing deep register manipulation, memory-mapped I/O experimentation, and low-level systems programming with zero financial cost.

## 🛠️ Prerequisites
Make sure you have the following tools installed on your system:

- Rust Toolchain
- QEMU, GDB & ARM Toolchain:

```bash
sudo apt update && sudo apt install qemu-system-arm gcc-arm-none-eabi gdb-multiarch
```
### Embedded Target Architecture:

~~~bash
rustup target add thumbv7em-none-eabihf
~~~
## 📂 Workspace Structure
This repository uses Cargo Workspaces to organize multiple firmware experiments under a single roof:
~~~text
cortex-m4-sandbox/
├── Cargo.toml              # Workspace master configuration
├── .cargo/
│   └── config.toml         # Global target, linker (-Tlink.x) and QEMU runner settings
└── projects/               # Individual firmware exercises
    └── basic-asm/          # Basic asm/loop test bench
~~~
## 🚀 Quick Start & Emulation

Clone the repository:
~~~bash
git clone https://github.com/cinnamon17/cortex-m4-sandbox.git
cd cortex-m4-sandbox
~~~
### Option A: Automatic via Cargo (Recommended)

~~~bash
cargo run --bin <binary-name>
~~~
### Option B: Manual QEMU Execution
If you prefer to run the compiled binary manually inside the QEMU virtual machine:

~~~bash
qemu-system-arm \
    -cpu cortex-m4 \
    -machine mps2-an386 \
    -nographic \
    -semihosting-config enable=on,target=native \
    -kernel target/thumbv7em-none-eabihf/debug/<binary-name>
~~~  
## 🐛 Debugging with GDB

To debug step-by-step using GDB:

1.  Run `cargo run` (QEMU will start paused waiting for a connection).
    
2.  In a second terminal, launch the multiarch debugger:
    
   
~~~bash
    gdb-multiarch target/thumbv7em-none-eabihf/debug/<binary-name>
~~~
    
3.  Inside GDB, connect to the emulation server:
    
    ~~~text
    (gdb) target remote :1234
    (gdb) break main
    (gdb) continue
    ~~~
    
## 💡 Engineering Principles
* **Shift-Left Testing**: Fast, automated, and hardware-independent firmware verification via QEMU simulation before physical deployment.
* **Bare-Metal (`no_std`)**: Direct register control, custom memory mapping, and low-level systems programming.

## 📄 License
Distributed under the GPLv3 License. See LICENSE for more information.
