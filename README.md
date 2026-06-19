# ByteOS

ByteOS is a minimal, ultra-optimized x86 operating system written in 16-bit Assembly (`NASM`) designed to run completely inside a single 512-byte master boot record (MBR). 

Despite strict hardware real-estate limitations, ByteOS eschews "toy" features in favor of core architectural baselines: a working memory-buffered command-line interpreter (CLI), direct BIOS clock interrupt parsing, and dedicated keyboard line disciplines.

## Features

* **Bare-Metal Real Mode OS:** Runs directly on x86 emulators or raw legacy hardware.
* **Minimalistic 16-Bit Architecture:** Avoids high-level overhead, communicating directly with the CPU registers and BIOS interrupts.
* **Hardware RTC Clock Synchronization:** The `time` command taps directly into the hardware Real-Time Clock (RTC) chip via `int 0x1a` and handles binary-coded decimal (BCD) to ASCII string conversion locally.
* **Custom TUI Style:** Overrides default console behavior to project a custom light-gray visual environment with bright-white output.
* **Escape Overrides:** Includes a hardware hotkey hook that captures the `ESC` key (ASCII 27) mid-loop for atomic system clearing and refresh.
* **Safe Hard Rebooting:** The `reboot` utility forces a cold-reset sequence by executing a far jump directly to the BIOS initialization block (`0xFFFF:0x0000`).

## File Anatomy (Memory Allocation)

Even though the final boot sector is structurally locked to 512 bytes due to legacy BIOS constraints, the engine itself is highly compact:
* **Active Code & Utilities:** ~140 bytes
* **Data, Strings & Buffers:** ~40 bytes
* **Zero-Padding Alignment:** ~330 bytes
* **Magic Boot Signature (`0xAA55`):** 2 bytes

## Getting Started

### Prerequisites
You need an x86 assembler (`NASM`) and a processor emulator (`QEMU`). In WSL/Linux, install them via:
```bash
sudo apt update
sudo apt install nasm qemu-system-x86
