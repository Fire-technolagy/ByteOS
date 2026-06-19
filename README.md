<p align="center">
  <img src="______  _   _)   _   _)  ______ BYTEOS.png" alt="ByteOS Logo" width="300">
</p>
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

## Command Utilities
* `help` - Dispatches all currently registered internal OS utilities.
* `cls` - Flushes the terminal view buffers and recalibrates the text pointer matrix.
* `time` - Pulls live, local time data straight from the host's motherboard hardware.
* `info` - Queries current system platform metadata.
* `reboot` - Instructs the underlying CPU to drop all runtime states and undergo a complete hard cycles restart.

---

## How to Run the ByteOS ISO

Since ByteOS is compiled into a standard hybrid bootable image (`byteos.iso`), it can be run instantly using modern virtualization software.

### 1. Oracle VM VirtualBox
1. Open VirtualBox and click **New** to create a virtual machine.
2. Configure the basic settings:
   * **Name:** ByteOS
   * **Type:** Other
   * **Version:** Other/Unknown (64-bit) or Other/Unknown (32-bit)
3. Set the base memory (RAM) to something small (e.g., **64 MB** or **128 MB** is more than enough). Do **not** create a virtual hard disk (choose *Do not add a virtual hard disk*).
4. Once the VM is created, select it and click **Settings** -> **Storage**.
5. Click on the empty Optical Drive controller, look to the right side under *Attributes*, click the CD disk icon, and select **Choose a disk file...**.
6. Select your downloaded `byteos.iso`.
7. Click **OK** and hit **Start** to boot the OS.

### 2. VMware Workstation Player / Pro
1. Open VMware and select **Create a New Virtual Machine**.
2. Choose **Installer disc image file (iso)**, click **Browse**, and select `byteos.iso`.
3. VMware might say it cannot detect the operating system; click **Next**.
4. Set the Guest Operating System type:
   * **Guest OS:** Other
   * **Version:** Other
5. Name the virtual machine `ByteOS` and allocate the minimum allowed RAM/Disk configurations (you can choose to split/store the disk but you won't need it).
6. Click **Finish**.
7. Power on the virtual machine to watch ByteOS initialize directly into the custom gray TUI.
