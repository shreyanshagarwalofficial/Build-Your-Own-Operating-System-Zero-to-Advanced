# Build Your Own Operating System — Zero to Advanced
**Primary implementation path:** x86-64 + C + a maintained bootloader + QEMU  
**Alternative paths:** RISC-V + C; Rust + a maintained bootloader  
**Repository type:** Learning roadmap and curated resource directory

---

## What You Will Learn

You will move from programming and computer architecture fundamentals to a bootable kernel, exception handling, physical and virtual memory, kernel allocation, scheduling, user mode, system calls, executable loading, filesystems, device I/O, a shell, testing, security, and advanced kernel design.

This is an educational roadmap, not a promise to produce a production-ready desktop operating system. A small OS that boots reliably, documents its design, and tests its core mechanisms is a meaningful result.

## Who This Is For

- Beginners who can commit to learning C and low-level programming.
- Computer science students studying systems and architecture.
- C/C++ programmers interested in kernels, compilers, or embedded systems.
- Rust programmers who want to understand the hardware and unsafe boundaries beneath abstractions.
- Systems programmers and advanced learners exploring operating-system internals.

### Prerequisites

You do **not** need prior kernel experience. You should be prepared to learn:

- Basic programming concepts, then C pointers, structs, bitwise operations, and debugging.
- Binary and hexadecimal notation.
- Shell usage, Git, compiling, linking, and reading compiler errors.
- Basic data structures and the distinction between a value, an address, and memory storage.

If C is new, complete Stages 0–2 before attempting a kernel. If you already know C and computer architecture, use the mastery checks to skip material you can demonstrate confidently.

## How to Use This Repository

1. Follow the **sequential roadmap** in order; each stage lists prerequisites, exercises, a project, testing, common mistakes, and a mastery gate.
2. Use the **subject index** to revisit a topic when implementation exposes a gap.
3. Read the **reference manuals** when a tutorial's assumptions are unclear. Architecture and boot-protocol details must match your chosen target.
4. Keep an engineering notebook: design decisions, build commands, logs, failing tests, and the reason for each fix.
5. Work in QEMU or another disposable virtual machine first. Do not experiment on a machine or disk image containing important data.

### Recommended starting choices

| Decision | Recommended first choice | Why |
|---|---|---|
| Architecture | x86-64 PC-style target | Broad OSDev documentation and widely available emulation |
| Language | C plus a small amount of assembly | Makes calling conventions, layout, and hardware boundaries visible |
| Boot | Limine or another maintained, documented bootloader | Lets you focus on kernel design before writing firmware-facing code |
| Emulator | QEMU system emulation | Repeatable boots, virtual devices, snapshots, and GDB remote debugging |
| Debugger | GDB | Registers, memory, symbols, breakpoints, and remote debugging |
| Theory text | Operating Systems: Three Easy Pieces (OSTEP) | Free online textbook on virtualization, concurrency, and persistence |
| Teaching kernel | MIT xv6-RISC-V | Compact source code and labs that expose core OS mechanisms |

**Important:** A tutorial's toolchain, boot protocol, CPU mode, linker layout, and executable format are a matched set. Do not combine commands from unrelated tutorials without understanding the differences.

---

## Table of Contents

- [Learning Roadmap](#learning-roadmap)
  - [Stage 0 — OS Concepts and Scope](#stage-0--os-concepts-and-scope)
  - [Stage 1 — C and Data Structures](#stage-1--c-and-data-structures)
  - [Stage 2 — Assembly and Machine Execution](#stage-2--assembly-and-machine-execution)
  - [Stage 3 — Computer Architecture](#stage-3--computer-architecture)
  - [Stage 4 — Toolchain and Reproducible Builds](#stage-4--toolchain-and-reproducible-builds)
  - [Stage 5 — OS Theory](#stage-5--operating-systems-theory)
  - [Stage 6 — Boot and Boot Protocols](#stage-6--boot-and-boot-protocols)
  - [Stage 7 — First Kernel](#stage-7--first-kernel)
  - [Stage 8 — Interrupts, Exceptions, and Traps](#stage-8--interrupts-exceptions-and-traps)
  - [Stage 9 — Physical Memory](#stage-9--physical-memory-management)
  - [Stage 10 — Virtual Memory](#stage-10--virtual-memory-and-paging)
  - [Stage 11 — Kernel Heap](#stage-11--kernel-heap-and-memory-safety)
  - [Stage 12 — Processes and Threads](#stage-12--processes-and-threads)
  - [Stage 13 — Scheduling and Synchronization](#stage-13--scheduling-and-synchronization)
  - [Stage 14 — User Mode and System Calls](#stage-14--user-mode-and-system-calls)
  - [Stage 15 — Executables and Program Loading](#stage-15--executables-and-program-loading)
  - [Stage 16 — Filesystems and Storage](#stage-16--filesystems-and-storage)
  - [Stage 17 — Device Drivers and I/O](#stage-17--device-drivers-and-hardware-io)
  - [Stage 18 — Shell and User Space](#stage-18--shell-and-user-space)
  - [Stage 19 — Networking](#stage-19--networking)
  - [Stage 20 — Multiprocessor Systems](#stage-20--multiprocessor-and-advanced-kernel-design)
  - [Stage 21 — Security and Reliability](#stage-21--kernel-security-and-reliability)
  - [Stage 22 — Testing, Debugging, and Performance](#stage-22--testing-debugging-and-performance)
  - [Stage 23 — Kernel Architecture Alternatives](#stage-23--advanced-os-architecture-and-design)
  - [Stage 24 — Rust OS Track](#stage-24--rust-operating-system-development)
  - [Stage 25 — Study Existing OSes](#stage-25--study-existing-educational-operating-systems)
  - [Stage 26 — Capstone OS](#stage-26--final-capstone-build-a-complete-educational-os)
- [Environment Setup](#development-environment)
- [Subject-Wise Resource Directory](#subject-wise-resource-directory)
- [Resource Collections by Format](#resource-collections-by-format)
- [Project Ladder](#project-ladder)
- [Study Schedules](#learning-schedules)
- [Glossary](#glossary)
- [Progress Checklist](#progress-checklist)
- [Contribution Guidelines](#contribution-guidelines)
- [Verification Methodology](#resource-verification-and-maintenance)
- [Disclaimer](#disclaimer)

---

# Learning Roadmap

Each stage should produce a concrete artifact: a diagram, program, test suite, implementation, design note, or reproducible build. Do not advance solely because you watched a video or read a chapter.

## Stage 0 — OS Concepts and Scope

**Why first:** Kernel work makes more sense once you can describe what the kernel does and what the bootloader, firmware, libraries, and applications do around it.

**Learn**
- Operating system vs. kernel; user space vs. kernel space.
- Applications, libraries, system calls, devices, firmware, and bootloaders.
- Monolithic, modular, microkernel, hybrid, and exokernel ideas.
- Privileged execution, isolation, resource management, and protection.
- The path from power-on to firmware, bootloader, kernel entry, and user program.
- The difference between a bare-metal program and an operating system.

**Practice**
- Draw a system diagram from power-on to an application.
- Compare Linux, Windows, FreeBSD, and xv6 at a high level.
- Write a one-page scope statement for a small educational OS.

**Project:** Architecture diagram plus a glossary of 20 terms.  
**Common mistakes:** Treating the kernel as the entire OS; trying to build a GUI before memory management or user mode exists.  
**Test/mastery gate:** Explain the boot path, kernel responsibilities, and three kernel-architecture trade-offs without relying on a diagram.

**Resources:** [OSDev Getting Started](https://wiki.osdev.org/Getting_Started), [OSTEP](https://pages.cs.wisc.edu/~remzi/OSTEP/), [MIT xv6 course](https://pdos.csail.mit.edu/6.1810/).

## Stage 1 — C and Data Structures

**Prerequisites:** Basic programming concepts.  
**Why now:** Kernel code often runs without a hosted C runtime and must make memory and ownership assumptions explicit.

**Learn**
- Compilation, preprocessing, object files, linking, symbols, and headers.
- Pointers, pointer arithmetic, arrays, strings, structs, unions, enums, and function pointers.
- Stack vs. heap, object lifetime, alignment, padding, and layout.
- Integer widths, signedness, bitwise operations, overflow, undefined behavior, and strict aliasing.
- Separate compilation, Makefiles, compiler warnings, and GDB basics.
- Arrays, linked lists, stacks, queues, hash tables, trees, heaps, bitmaps, circular buffers, and intrusive lists.

**Practice**
- Compile with strong warnings; inspect warnings rather than silencing them.
- Implement a circular buffer, bitmap, linked list, and fixed-size allocator simulation.
- Write unit tests for empty, full, boundary, and invalid-input cases.
- Inspect struct layout with `sizeof`, `_Alignof`, and debugger memory views.

**Project:** A small C systems library with tests and documented ownership rules.  
**Common mistakes:** Assuming pointers are integers; assuming zeroed memory is always a valid object; using `volatile` as a general concurrency primitive.  
**Test/mastery gate:** Explain the lifetime and layout of each object in a non-trivial C function; debug a memory error with GDB.

**Resources:** [Beej's Guide to C](https://beej.us/guide/bgc/), [GNU C manual](https://www.gnu.org/software/c-intro-and-ref/manual/), [cppreference C](https://en.cppreference.com/w/c), [GDB manual](https://www.sourceware.org/gdb/current/onlinedocs/gdb/).

## Stage 2 — Assembly and Machine Execution

**Prerequisites:** Stage 1.  
**Learn**
- Bits, bytes, hexadecimal, two's complement, and endianness.
- Registers, instructions, flags, loads/stores, branches, calls, returns, and stack frames.
- Calling conventions, ABI rules, register preservation, and alignment.
- Assembly syntax and assembler directives.
- Machine code, object files, symbols, relocations, disassembly, and ELF basics.
- Privileged vs. unprivileged instructions; exceptions and traps conceptually.

Use x86-64 for the main path. Treat AArch64 or RISC-V as separate tracks rather than mixing registers and calling conventions.

**Practice:** Compile a C function to assembly; use `objdump` and `readelf`; single-step a function in GDB; write a tiny assembly routine callable from C.  
**Project:** A documented C/assembly program with a repeatable build.  
**Common mistakes:** Mixing AT&T and Intel syntax; assuming one ABI applies to all targets; overlooking stack alignment.  
**Test/mastery gate:** Trace a function call from arguments through registers/stack to return, and explain the resulting object file.

**Resources:** [Intel Software Developer Manuals](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html), [GNU assembler manual](https://sourceware.org/binutils/docs/as/), [System V ABI](https://gitlab.com/x86-psABIs/x86-64-ABI), [NASM documentation](https://www.nasm.us/doc/), [RISC-V specifications](https://docs.riscv.org/).

## Stage 3 — Computer Architecture

**Prerequisites:** Stage 2.  
**Learn:** ISA vs. microarchitecture, execution modes, caches, memory hierarchy, buses, MMU, physical and virtual addresses, paging hardware, memory-mapped I/O, port I/O where relevant, timers, interrupt controllers, DMA concepts, atomic instructions, cache coherence, SMP, firmware, UEFI, x86-64 modes, and RISC-V privilege levels.

**Practice**
- Diagram the memory hierarchy and a peripheral's path to the CPU.
- Trace a virtual-address translation through a multi-level page table.
- Read the relevant architecture manual section for one instruction or exception.
- Inspect the emulated machine using QEMU options and monitor commands.

**Project:** Architecture notebook: register references, address translation, and boot-mode diagrams.  
**Common mistakes:** Treating architectural guarantees as universal across CPUs; assuming hardware devices behave identically in all emulators.  
**Test/mastery gate:** Explain how privilege level, interrupts, and address translation enforce a kernel/user boundary.

**Resources:** [Intel SDM](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html), [AMD64 Architecture Programmer's Manual](https://www.amd.com/en/search/documentation/hub.html), [RISC-V specifications](https://docs.riscv.org/), [UEFI specifications](https://uefi.org/specifications), [Nand2Tetris](https://www.nand2tetris.org/).

## Stage 4 — Toolchain and Reproducible Builds

**Prerequisites:** Stages 1–3.  
**Learn**
- Linux shell, processes, permissions, environment variables, and filesystem paths.
- Git, branches, commits, tags, and clean checkouts.
- Host compiler vs. cross-compiler; hosted vs. freestanding C.
- GCC/Clang, assembler, linker, linker scripts, and build systems.
- `objdump`, `readelf`, `nm`, `file`, `make`, and debugging symbols.
- QEMU system emulation, GDB remote debugging, serial output, disk images, and boot media.
- Reproducible builds, pinned tool versions, and build logs.

**Practice:** Build a freestanding object, link it with a custom linker script, inspect symbols, and run a tiny kernel under QEMU.  
**Project:** A one-command build and boot workflow from a clean checkout.  
**Common mistakes:** Using the host ABI accidentally; linking against the host C runtime; omitting required low-level compiler runtime support; following stale installation commands blindly.  
**Test/mastery gate:** A second clean checkout can build and launch the same kernel using the README instructions.

**Resources:** [GCC documentation](https://gcc.gnu.org/onlinedocs/), [Clang documentation](https://clang.llvm.org/docs/), [GNU Binutils](https://sourceware.org/binutils/docs/), [GNU Make](https://www.gnu.org/software/make/manual/), [QEMU system emulation](https://qemu-project.gitlab.io/qemu/system/), [GDB](https://www.sourceware.org/gdb/current/onlinedocs/gdb/).

## Stage 5 — Operating Systems Theory

**Prerequisites:** Stages 0–4.  
**Learn:** Processes, threads, context switches, scheduling, concurrency, synchronization, deadlocks, virtual memory, paging, file systems, I/O, device drivers, system calls, IPC, networking, protection, virtualization, multiprocessor systems, and reliability.

**Practice:** Simulate round-robin scheduling and page replacement; solve producer/consumer and readers/writers problems; explain a system call from user code to kernel and back.  
**Project:** A set of small simulators and written explanations of design trade-offs.  
**Common mistakes:** Memorizing definitions without reasoning about failure cases; assuming a textbook's simplified system is identical to Linux or Windows.  
**Test/mastery gate:** Explain the three OSTEP pillars—virtualization, concurrency, and persistence—and connect each to a kernel subsystem.

**Resources:** [OSTEP — free online book](https://pages.cs.wisc.edu/~remzi/OSTEP/), [MIT Operating System Engineering](https://pdos.csail.mit.edu/6.1810/), [xv6 book and source](https://github.com/mit-pdos/xv6-riscv), [University of Virginia CS 4414 materials](https://www.cs.virginia.edu/~cr4bd/4414/).

## Stage 6 — Boot and Boot Protocols

**Prerequisites:** Stages 1–5.  
**Learn:** Firmware responsibilities, BIOS vs. UEFI, boot selection, bootloader contracts, kernel loading, memory maps, boot information, ELF loading, physical vs. virtual addresses, entry points, stack setup, early logging, and boot failures.

**Recommended first implementation:** Use a maintained bootloader and a documented protocol. Learn custom bootloaders later. A bootloader is a separate project with different firmware and compatibility concerns.

**Practice:** Read the chosen boot protocol; inspect the kernel ELF; document what the loader passes to the kernel; intentionally break one boot assumption and diagnose the failure.  
**Project:** A minimal kernel that reaches its entry point and emits serial output in QEMU.  
**Common mistakes:** Mixing incompatible boot protocols; assuming a tutorial's old GRUB setup applies to a modern 64-bit kernel; relying on graphical output before serial diagnostics exist.  
**Test/mastery gate:** Reproduce a boot from a clean build and explain every interface between loader and kernel.

**Resources:** [OSDev boot sequence](https://wiki.osdev.org/Boot_Sequence), [OSDev tutorials](https://wiki.osdev.org/Tutorials), [Limine boot protocol](https://github.com/limine-bootloader/limine-protocol), [UEFI specifications](https://uefi.org/specifications), [GNU GRUB manual](https://www.gnu.org/software/grub/manual/).

## Stage 7 — First Kernel

**Prerequisites:** Stages 1–6.  
**Learn:** Kernel entry, linker layout, freestanding compilation, stack setup, runtime support, serial logging, panic and assertion behavior, build artifacts, symbol tables, and early debugging.

**Project progression**
1. Kernel image and reproducible build.
2. Entry point and stack initialization.
3. `Hello, kernel` on serial.
4. Logging with levels and formatted values.
5. Panic and assertion paths.
6. Documentation of expected output and common failures.

**Test:** Run in QEMU repeatedly; capture serial output; confirm panic output is deterministic.  
**Common mistakes:** Using unavailable libc functions; forgetting compiler-generated runtime helpers; assuming the host's process startup code runs in a kernel.  
**Mastery gate:** Kernel boots repeatedly and reports failures without silently hanging.

**Resources:** [OSDev Bare Bones](https://wiki.osdev.org/Barebones), [OSDev Limine Bare Bones entry point](https://wiki.osdev.org/Getting_Started), [Limine protocol](https://github.com/limine-bootloader/limine-protocol), [Phil Opp's Rust OS series](https://os.phil-opp.com/) (different language path; do not mix code directly).

## Stage 8 — Interrupts, Exceptions, and Traps

**Prerequisites:** Stage 7 and architecture/calling-convention knowledge.  
**Learn:** Synchronous exceptions vs. asynchronous interrupts, vector tables, x86-64 IDT, interrupt service routines, saved CPU state, interrupt controllers, timers, keyboard events, masking, interrupt-safe code, deferred work, and fault diagnosis. For RISC-V, study trap entry and supervisor-level CSRs separately.

**Project:** Exception handlers that print the vector, error information when available, and saved instruction state; then add a timer interrupt.  
**Test:** Trigger a controlled exception; confirm the handler records useful context; test interrupt enable/disable behavior.  
**Common mistakes:** Incorrect stack layout; clobbering registers; calling code that is not safe in interrupt context; enabling interrupts before handlers are ready.  
**Mastery gate:** Explain the CPU-to-handler transition and diagnose a deliberate exception.

**Resources:** [OSDev Interrupts](https://wiki.osdev.org/Interrupts), [OSDev IDT](https://wiki.osdev.org/Interrupt_Descriptor_Table), [Intel SDM](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html), [RISC-V specifications](https://docs.riscv.org/).

## Stage 9 — Physical Memory Management

**Prerequisites:** Stage 8 and boot memory-map understanding.  
**Learn:** Usable/reserved memory, page frames, allocation metadata, bitmaps, free lists, buddy allocation, alignment, early allocators, fragmentation, reserved regions, out-of-memory behavior, and allocator invariants.

**Project:** A physical page-frame allocator with allocate/free operations and a diagnostic summary.  
**Test:** Exhaust memory, release frames, test boundaries, verify reserved ranges are never returned, and run randomized allocation sequences.  
**Common mistakes:** Freeing bootloader/kernel memory; integer overflow in range arithmetic; double-free; ignoring alignment.  
**Mastery gate:** Allocator invariants survive stress tests and are documented.

**Resources:** [OSDev Memory Management](https://wiki.osdev.org/Memory_management), [OSDev Memory Architecture](https://wiki.osdev.org/Memory_Architecture), [OSTEP memory virtualization](https://pages.cs.wisc.edu/~remzi/OSTEP/), [xv6 source](https://github.com/mit-pdos/xv6-riscv).

## Stage 10 — Virtual Memory and Paging

**Prerequisites:** Stage 9 and architecture paging documentation.  
**Learn:** Address spaces, page tables, page permissions, page sizes, TLBs, invalidation, page faults, kernel mappings, mapping/unmapping, user isolation, shared mappings, demand paging, copy-on-write, memory-mapped files, and ASLR concepts.

**Project progression:** Enable paging; map kernel memory; create/destroy address spaces; handle faults; add isolated user address spaces; implement copy-on-write as an advanced extension.  
**Test:** Map/unmap test pages, verify permissions, trigger controlled faults, and ensure one address space cannot access another's protected memory.  
**Common mistakes:** Stale TLB entries; incorrect physical/virtual conversions; mapping a page with overly broad permissions; freeing a frame still mapped elsewhere.  
**Mastery gate:** Explain and test the complete virtual-to-physical translation and protection path.

**Resources:** [OSDev Paging](https://wiki.osdev.org/Paging), [OSDev Memory Architecture](https://wiki.osdev.org/Memory_Architecture), [Intel SDM](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html), [OSTEP](https://pages.cs.wisc.edu/~remzi/OSTEP/), [xv6 source](https://github.com/mit-pdos/xv6-riscv).

## Stage 11 — Kernel Heap and Memory Safety

**Prerequisites:** Stages 9–10.  
**Learn:** Kernel allocation APIs, free lists, slab/object allocator concepts, alignment, fragmentation, metadata protection, failure handling, double-free, use-after-free, poisoning, guard regions, leak detection, stress testing, and concurrency-safe allocation.

**Project:** A kernel heap with tests, allocation statistics, and debug checks.  
**Test:** Repeated allocate/free cycles, mixed sizes, exhaustion, alignment, invalid frees, and fragmentation patterns.  
**Common mistakes:** Returning misaligned memory; allocator recursion during initialization; corrupting metadata; assuming all allocations succeed.  
**Mastery gate:** The allocator survives stress tests and reports failure predictably.

**Resources:** [OSDev Heap](https://wiki.osdev.org/Heap), [OSDev Memory Management](https://wiki.osdev.org/Memory_management), [OSTEP](https://pages.cs.wisc.edu/~remzi/OSTEP/).

## Stage 12 — Processes and Threads

**Prerequisites:** Virtual memory, interrupts, and context-saving fundamentals.  
**Learn:** Process/thread control blocks, saved CPU context, kernel stacks, lifecycle, identifiers, cooperative and preemptive multitasking, parent/child semantics, wait/exit, scheduling queues, and per-thread state.

**Project:** Kernel threads and context switching, initially cooperative.  
**Test:** Switch between multiple tasks; preserve registers and stacks; test task exit and cleanup.  
**Common mistakes:** Sharing one stack between tasks; freeing a running task's stack; losing interrupt state during a switch.  
**Mastery gate:** Multiple independent tasks run predictably without corrupting one another's state.

**Resources:** [OSTEP — processes](https://pages.cs.wisc.edu/~remzi/OSTEP/), [MIT xv6 course](https://pdos.csail.mit.edu/6.1810/), [xv6 source](https://github.com/mit-pdos/xv6-riscv), [Pintos project](https://web.stanford.edu/class/cs140/projects/pintos/).

## Stage 13 — Scheduling and Synchronization

**Prerequisites:** Stage 12 and atomic-operation basics.  
**Learn:** Scheduling goals, round-robin, priorities, multilevel feedback queues, preemption, timer-driven scheduling, locks, spinlocks, mutexes, semaphores, condition variables, atomics, race conditions, deadlocks, starvation, priority inversion, interrupt-safe synchronization, and SMP concerns.

**Project:** Timer-driven preemptive scheduler with concurrent kernel tasks.  
**Test:** Stress shared counters and queues; test lock ordering; detect starvation; verify interrupts are restored correctly.  
**Common mistakes:** Holding a spinlock while sleeping; using blocking primitives in interrupt context; confusing atomicity with ordering; unbounded critical sections.  
**Mastery gate:** Explain the scheduler policy and demonstrate tests for races and deadlocks.

**Resources:** [OSTEP — concurrency](https://pages.cs.wisc.edu/~remzi/OSTEP/), [MIT xv6 course](https://pdos.csail.mit.edu/6.1810/), [Pintos](https://web.stanford.edu/class/cs140/projects/pintos/), [Linux kernel locking documentation](https://docs.kernel.org/locking/index.html).

## Stage 14 — User Mode and System Calls

**Prerequisites:** Virtual memory, processes, interrupts, and calling conventions.  
**Learn:** Privilege separation, user/kernel transitions, syscall entry/exit, syscall numbers, argument validation, register preservation, safe copying across address spaces, error conventions, user stacks, permissions, and return-to-user behavior.

**Project:** Enter user mode and implement a minimal syscall API such as write, exit, and yield.  
**Test:** Invalid pointers, invalid syscall numbers, user attempts to access protected kernel memory, and normal syscall returns.  
**Common mistakes:** Trusting user-provided pointers; allowing user-controlled kernel addresses; failing to validate sizes; returning with incorrect CPU state.  
**Mastery gate:** A user task can request services without receiving unrestricted kernel privileges.

**Resources:** [MIT xv6 course](https://pdos.csail.mit.edu/6.1810/), [xv6 source](https://github.com/mit-pdos/xv6-riscv), [OSTEP](https://pages.cs.wisc.edu/~remzi/OSTEP/), [Intel SDM](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html).

## Stage 15 — Executables and Program Loading

**Prerequisites:** Stage 14 and ELF fundamentals.  
**Learn:** ELF headers, program headers vs. sections, segments, relocations, symbol tables, static linking, dynamic-linking concepts, executable segment permissions, user stack layout, arguments/environment, entry-point transfer, and position-independent code.

**Project:** Load and execute a small statically linked user program.  
**Test:** Reject malformed headers, out-of-range segments, overlapping or invalid mappings, and unsupported file types.  
**Common mistakes:** Treating sections as the runtime loading contract; ignoring alignment; trusting unvalidated file sizes; mapping writable executable pages unnecessarily.  
**Mastery gate:** The kernel loads a known executable and rejects malformed input safely.

**Resources:** [System V ABI / x86-64 ABI](https://gitlab.com/x86-psABIs/x86-64-ABI), [ELF specification project](https://refspecs.linuxfoundation.org/elf/elf.pdf), [GNU Binutils](https://sourceware.org/binutils/docs/), [MIT xv6](https://github.com/mit-pdos/xv6-riscv).

## Stage 16 — Filesystems and Storage

**Prerequisites:** Kernel allocation, device abstractions, and basic process/syscall design.  
**Learn:** Block devices, sectors, disk images, files/directories, file descriptors, path resolution, inodes and metadata, free-space management, caches, mounting, permissions, consistency, journaling concepts, crash recovery, FAT-style filesystems, ext-family concepts, and VFS layers.

**Project progression:** In-memory filesystem; block-device abstraction; persistent disk image; file/directory operations; integrity checks.  
**Test:** Reboot persistence, invalid paths, full disk, partial writes, malformed metadata, and interrupted-operation scenarios where supported.  
**Common mistakes:** Assuming writes are atomic; mixing logical blocks and sectors; trusting on-disk metadata; testing on valuable disks.  
**Mastery gate:** Files survive reboot in a disposable image, and corruption/unsupported formats fail safely.

**Resources:** [OSDev File Systems](https://wiki.osdev.org/File_Systems), [OSDev ATA PIO Mode](https://wiki.osdev.org/ATA_PIO_Mode), [OSTEP — persistence](https://pages.cs.wisc.edu/~remzi/OSTEP/), [xv6 source](https://github.com/mit-pdos/xv6-riscv), [Linux VFS documentation](https://docs.kernel.org/filesystems/vfs.html).

## Stage 17 — Device Drivers and Hardware I/O

**Prerequisites:** Interrupts, memory mapping, and a bootable kernel.  
**Learn:** Driver boundaries, device discovery, PCI/PCIe concepts, MMIO, port I/O where applicable, polling vs. interrupts, DMA concepts, timers, serial, keyboard, framebuffer, storage, USB fundamentals, initialization, resource ownership, and error handling.

**Project:** Serial output, timer, keyboard input, and one simple display or virtual device path.  
**Test:** Device absent, malformed input, repeated initialization, timeout, and interrupt storm simulations where feasible.  
**Common mistakes:** Treating device registers as ordinary memory; assuming a physical device's behavior matches QEMU exactly; unsafe DMA assumptions.  
**Mastery gate:** Drivers use documented interfaces and recover or fail clearly when a device is unavailable.

**Resources:** [OSDev Wiki](https://wiki.osdev.org/Main_Page), [QEMU system emulation](https://qemu-project.gitlab.io/qemu/system/), [PCI-SIG specifications](https://pcisig.com/specifications), [Linux driver API documentation](https://docs.kernel.org/driver-api/index.html), [UEFI specifications](https://uefi.org/specifications).

## Stage 18 — Shell and User Space

**Prerequisites:** User mode, syscalls, executable loading, and filesystems.  
**Learn:** Command parsing, arguments, environment variables, stdin/stdout/stderr, file descriptors, process execution, exit codes, pipes, redirection, system libraries, minimal C runtime, syscall wrappers, and utility programs.

**Project:** A command shell plus utilities such as `echo`, `help`, `cat`, and `ls` appropriate to the available filesystem API.  
**Test:** Empty input, long input, unknown command, argument quoting policy, file-not-found, and program exit.  
**Common mistakes:** Buffer overflow in the command line; confusing shell built-ins with external programs; assuming a full POSIX environment exists.  
**Mastery gate:** A user can interactively launch multiple programs and inspect files.

**Resources:** [MIT xv6](https://github.com/mit-pdos/xv6-riscv), [OSTEP](https://pages.cs.wisc.edu/~remzi/OSTEP/), [GNU C Library manual](https://www.gnu.org/software/libc/manual/), [POSIX specifications](https://pubs.opengroup.org/onlinepubs/9799919799/).

## Stage 19 — Networking

**Prerequisites:** Drivers, buffers, interrupts, and user/kernel interfaces.  
**Learn:** Ethernet, MAC addressing, ARP, IP, routing, ICMP, UDP, TCP, checksums, retransmission, timeouts, network interface drivers, packet buffers, socket abstractions, DNS concepts, configuration, and security boundaries.

**Project:** First implement packet parsing or a loopback path; then use a documented existing stack or build a small protocol subset in QEMU.  
**Test:** Malformed packets, checksum failures, timeouts, packet loss, and bounds checking.  
**Common mistakes:** Writing a complete TCP/IP stack too early; assuming every packet arrives in order; trusting packet lengths.  
**Mastery gate:** Explain the path from virtual NIC through packet processing to a user-space API.

**Resources:** [OSDev Networking](https://wiki.osdev.org/Networking), [QEMU networking documentation](https://qemu-project.gitlab.io/qemu/system/devices/net.html), [RFC Editor](https://www.rfc-editor.org/), [lwIP](https://www.nongnu.org/lwip/), [VirtIO specifications](https://docs.oasis-open.org/virtio/).

## Stage 20 — Multiprocessor and Advanced Kernel Design

**Prerequisites:** Scheduler and synchronization stages.  
**Learn:** SMP startup, application processors, per-CPU state, interprocessor interrupts, scheduler scalability, lock contention, atomic memory ordering, cache coherence, NUMA concepts, interrupt affinity, and concurrency debugging.

**Project:** Enable a second emulated CPU and safely run independent tasks.  
**Test:** Stress shared data, scheduler queues, allocator operations, and shutdown paths. Document which locks protect which invariants.  
**Common mistakes:** Assuming single-CPU correctness implies SMP correctness; missing memory-ordering requirements; global locks that make progress impossible.  
**Mastery gate:** Demonstrate repeatable multi-CPU behavior with explicit synchronization invariants.

**Resources:** [Linux kernel documentation](https://docs.kernel.org/), [Intel SDM](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html), [RISC-V specifications](https://docs.riscv.org/), [OSTEP](https://pages.cs.wisc.edu/~remzi/OSTEP/).

## Stage 21 — Kernel Security and Reliability

**Prerequisites:** User mode, memory protection, syscalls, and filesystem fundamentals.  
**Learn:** Threat modeling, least privilege, attack surface, integer overflow, out-of-bounds access, use-after-free, race conditions, syscall validation, kernel hardening, randomization concepts, stack protection, capabilities, access control, fault containment, secure-boot concepts, fuzzing, and secure development practices.

**Project:** A threat model plus negative tests for syscall boundaries, executable parsing, and filesystem metadata.  
**Test:** Include malformed input and permission-boundary tests in CI; record known limitations and security assumptions.  
**Common mistakes:** Claiming a feature is secure without a threat model; treating secure boot as a substitute for runtime protections; exposing debug interfaces without understanding the risk.  
**Mastery gate:** State what is protected, from whom, under which assumptions, and how each claim is tested.

**Resources:** [MITRE CWE](https://cwe.mitre.org/), [NIST Computer Security Resource Center](https://csrc.nist.gov/), [Linux kernel self-protection documentation](https://docs.kernel.org/security/self-protection.html), [OSTEP security](https://pages.cs.wisc.edu/~remzi/OSTEP/).

## Stage 22 — Testing, Debugging, and Performance

**Prerequisites:** A kernel with multiple subsystems.  
**Learn:** Unit and integration tests, boot tests, regression tests, emulator automation, GDB remote debugging, QEMU logging, serial diagnostics, panic reports, crash reproduction, coverage concepts, static analysis, fuzzing, latency/throughput measurement, benchmark design, reproducibility, and CI.

**Project:** An automated pipeline that builds the kernel, boots it in QEMU, captures output, and checks expected behavior.  
**Test:** Clean builds, boot failure, panic path, allocator stress, syscall negative cases, and disk-image reset.  
**Common mistakes:** Testing only successful paths; relying on screenshots instead of logs; measuring performance without a baseline; flaky timing-based assertions.  
**Mastery gate:** Another developer can reproduce the build and run automated checks from the documentation.

**Resources:** [QEMU](https://qemu-project.gitlab.io/qemu/system/), [QEMU GDB usage](https://www.qemu.org/docs/master/system/gdb.html), [GDB manual](https://www.sourceware.org/gdb/current/onlinedocs/gdb/), [Clang Static Analyzer](https://clang.llvm.org/docs/ClangStaticAnalyzer.html), [LLVM libFuzzer](https://llvm.org/docs/LibFuzzer.html), [GitHub Actions documentation](https://docs.github.com/actions).

## Stage 23 — Advanced OS Architecture and Design

**Prerequisites:** Stages 5, 12–22.  
**Learn:** Monolithic, modular, microkernel, hybrid, exokernel, capability-based design, message passing, user-space drivers, hypervisors, virtualization, containers/namespaces, isolation, real-time OS concepts, embedded OSes, distributed systems concepts, and persistent memory.

**Project:** Compare two real kernels with an architecture decision record. State requirements, failure model, performance goals, security boundary, and trade-offs.  
**Test:** Defend the design using source code and documentation rather than preference alone.  
**Mastery gate:** Make a reasoned architecture choice and identify the cost of the alternatives.

**Resources:** [MINIX 3](https://www.minix3.org/), [seL4 documentation](https://docs.sel4.systems/), [FreeBSD documentation](https://docs.freebsd.org/), [Linux kernel documentation](https://docs.kernel.org/), [Xen documentation](https://xenproject.org/help/documentation/).

## Stage 24 — Rust Operating System Development

**Prerequisites:** Rust fundamentals; architecture and OS theory remain required.  
**Learn:** Ownership, borrowing, lifetimes, traits, generics, `no_std`, `core`, `alloc`, panic handling, custom targets, cross-compilation, bootloader integration, unsafe Rust, inline assembly, memory mapping, interrupt handling, allocators, synchronization, and safe/unsafe boundaries.

**Project:** Follow the current edition of Writing an OS in Rust, then document where the compiler cannot prove hardware or concurrency assumptions.  
**Common mistakes:** Assuming Rust removes all kernel unsafety; following the archived first edition as if it were current; mixing its bootloader versions with another tutorial.  
**Mastery gate:** Explain the safety invariant around each `unsafe` block and test the surrounding contract.

**Resources:** [Writing an OS in Rust — current series](https://os.phil-opp.com/), [source repository](https://github.com/phil-opp/blog_os), [The Rust Book](https://doc.rust-lang.org/book/), [Rust Reference](https://doc.rust-lang.org/reference/), [`no_std` documentation](https://docs.rust-embedded.org/book/intro/no-std.html).

## Stage 25 — Study Existing Educational Operating Systems

**Prerequisites:** OS theory and basic kernel code reading. These systems are complementary study tracks, not all mandatory prerequisites.

### xv6-RISC-V
- **Purpose:** Compact Unix-like teaching kernel used in MIT's OS engineering course.
- **Study order:** Boot and entry; traps; process table; scheduler; virtual memory; syscalls; filesystem; locking.
- **Practice:** Build it, run its tests, trace one syscall, then implement a small course-style extension.
- **Limitation:** It is intentionally much smaller than a production OS.

Resources: [MIT course](https://pdos.csail.mit.edu/6.1810/), [source](https://github.com/mit-pdos/xv6-riscv), [book source](https://github.com/mit-pdos/xv6-riscv-book).

### Pintos
- **Purpose:** Teaching OS projects centered on threads, user programs, virtual memory, and filesystems.
- **Study order:** Threads and synchronization; user processes; virtual memory; filesystem.
- **Resource:** [Stanford Pintos](https://web.stanford.edu/class/cs140/projects/pintos/). Course availability and assignment details can change.

### JOS
- **Purpose:** Educational OS labs that expose low-level x86 mechanisms and kernel/user interaction.
- **Resource:** [MIT 6.828 materials archive](https://pdos.csail.mit.edu/6.828/).

### Linux
- **Purpose:** Learn real-world subsystem organization, APIs, drivers, memory management, and development process.
- **Study order:** Kernel docs for one subsystem; then trace a small path in source code.
- **Resources:** [Linux kernel documentation](https://docs.kernel.org/), [Linux source](https://git.kernel.org/).

### FreeBSD
- **Purpose:** Study a mature BSD system, kernel interfaces, networking, storage, and documentation practices.
- **Resources:** [FreeBSD Handbook](https://docs.freebsd.org/en/books/handbook/), [FreeBSD source](https://github.com/freebsd/freebsd-src).

### Zephyr
- **Purpose:** Embedded and real-time-oriented OS concepts, device model, configuration, and supported hardware.
- **Resources:** [Zephyr documentation](https://docs.zephyrproject.org/), [Zephyr source](https://github.com/zephyrproject-rtos/zephyr).

### MINIX 3 and seL4
- **Purpose:** Explore microkernel design and capability-based security.
- **Resources:** [MINIX 3](https://www.minix3.org/), [seL4 docs](https://docs.sel4.systems/), [seL4 source](https://github.com/seL4/seL4).

## Stage 26 — Final Capstone: Build a Complete Educational OS

Define the minimum viable OS first. Every feature after that is an extension, not a prerequisite for calling the project a successful educational OS.

### Minimum Viable OS

- [ ] Boots reproducibly in QEMU using a documented boot protocol.
- [ ] Initializes the kernel and prints serial diagnostics.
- [ ] Handles basic exceptions with useful fault context.
- [ ] Tracks usable and reserved physical memory.
- [ ] Implements a physical frame allocator.
- [ ] Enables virtual memory with kernel mappings.
- [ ] Provides a tested kernel heap.
- [ ] Supports at least basic scheduling and multiple tasks.
- [ ] Enters user mode on the chosen architecture.
- [ ] Implements a minimal syscall interface.
- [ ] Loads a simple executable format subset safely.
- [ ] Provides a simple filesystem, initially in-memory if needed.
- [ ] Offers an interactive shell or test runner.
- [ ] Includes build instructions, design notes, tests, known limitations, and license information.

### Intermediate OS
- [ ] Preemptive multitasking.
- [ ] User-space utilities and a small runtime.
- [ ] Persistent disk image and file/directory operations.
- [ ] Keyboard input and improved device abstractions.
- [ ] Better memory diagnostics and failure handling.
- [ ] Automated build and boot tests.

### Advanced optional OS
- [ ] SMP support.
- [ ] Networking.
- [ ] Additional filesystems and drivers.
- [ ] Dynamic linking.
- [ ] Advanced isolation and security features.
- [ ] Performance profiling and documented benchmarks.
- [ ] Real hardware support after emulated behavior is stable.

### Capstone milestone definition

For every feature, record: **dependency, implementation, normal test, failure test, expected output, debugging procedure, known limitation, and completion criteria.** Do not mark a subsystem complete merely because it compiles.

---

# Development Environment

## Recommended environment

Use a Linux environment for the primary build. Windows users can use WSL for toolchain work, but hardware access and some emulator/boot workflows may differ. Keep the first development target inside QEMU and use disposable disk images.

### Tool categories and official documentation

| Tool | Purpose | Documentation |
|---|---|---|
| Linux | Host environment | [Kernel docs](https://docs.kernel.org/) and your distribution's official docs |
| GCC | C compiler | [GCC manual](https://gcc.gnu.org/onlinedocs/) |
| Clang/LLVM | Alternative compiler and analysis tools | [Clang docs](https://clang.llvm.org/docs/) |
| GNU Binutils | Assembler, linker, symbol and binary tools | [Binutils docs](https://sourceware.org/binutils/docs/) |
| NASM | Optional x86 assembler | [NASM manual](https://www.nasm.us/doc/) |
| GNU Make | Build orchestration | [Make manual](https://www.gnu.org/software/make/manual/) |
| CMake | Optional build generator | [CMake docs](https://cmake.org/documentation/) |
| GDB | Debugging | [GDB docs](https://www.sourceware.org/gdb/current/onlinedocs/gdb/) |
| QEMU | System emulation | [QEMU docs](https://qemu-project.gitlab.io/qemu/system/) |
| GRUB | Bootloader option | [GRUB manual](https://www.gnu.org/software/grub/manual/) |
| Limine | Bootloader and boot protocol | [Limine repository](https://github.com/limine-bootloader/limine) |
| Git | Version control | [Git book](https://git-scm.com/book/en/v2) |
| Rust toolchain | Rust track | [Rust installation](https://www.rust-lang.org/tools/install) |

### Host compiler vs. cross-compiler

A host compiler normally targets the host operating system and its runtime conventions. A kernel generally needs a **freestanding** build: it does not start as a normal host process and cannot assume a host libc, process loader, or OS services. A cross-compiler makes the target explicit and helps prevent accidental use of host headers, libraries, or ABI assumptions.

Do not copy compiler flags from a tutorial without understanding its target triple, CPU mode, ABI, boot protocol, and linker layout. Keep a build log and document the versions that actually work.



---

# Subject-Wise Resource Directory

The directory below is a curated starting index. Resource availability, version, licensing, and course access can change. **Free to read does not necessarily mean open-licensed**, and a public repository does not automatically grant redistribution rights. The categories intentionally reuse foundational references when they are relevant; they are not independent counts of unique resources.

## 1. OS fundamentals and theory
- [OSTEP](https://pages.cs.wisc.edu/~remzi/OSTEP/) — free online textbook; virtualization, concurrency, persistence.
- [MIT 6.1810 Operating System Engineering](https://pdos.csail.mit.edu/6.1810/) — course, xv6, and labs; RISC-V teaching kernel.
- [MIT OCW OS Engineering syllabus](https://ocw.mit.edu/courses/6-1810-operating-system-engineering-fall-2023/pages/syllabus/) — course outline and prerequisites.
- [xv6 source](https://github.com/mit-pdos/xv6-riscv) — compact Unix-like teaching kernel.
- [xv6 book source](https://github.com/mit-pdos/xv6-riscv-book) — explanatory book.
- [OSDev Wiki](https://wiki.osdev.org/Main_Page) — community-maintained OS development reference.
- [OSDev Getting Started](https://wiki.osdev.org/Getting_Started) — scope and first steps.
- [OSDev Tutorials](https://wiki.osdev.org/Tutorials) — tutorials organized by topic.
- [Pintos](https://web.stanford.edu/class/cs140/projects/pintos/) — teaching OS projects.
- [JOS / MIT 6.828 materials](https://pdos.csail.mit.edu/6.828/) — x86 OS labs and references.
- [Nand2Tetris](https://www.nand2tetris.org/) — build a computer/software stack from first principles; free for non-profit use under site terms.
- [Linux kernel documentation](https://docs.kernel.org/) — real production-kernel concepts and subsystem references.
- [FreeBSD Handbook](https://docs.freebsd.org/en/books/handbook/) — practical BSD system documentation.
- [MINIX 3](https://www.minix3.org/) — microkernel-oriented OS project.
- [seL4 documentation](https://docs.sel4.systems/) — microkernel and capability-oriented documentation.

## 2. C and systems programming
- [Beej's Guide to C](https://beej.us/guide/bgc/) — approachable C tutorial.
- [GNU C manual](https://www.gnu.org/software/c-intro-and-ref/manual/) — C language concepts.
- [cppreference: C](https://en.cppreference.com/w/c) — reference for C language and library.
- [SEI CERT C Coding Standard](https://wiki.sei.cmu.edu/confluence/display/c/SEI+CERT+C+Coding+Standard) — secure C coding guidance.
- [GCC documentation](https://gcc.gnu.org/onlinedocs/) — compiler behavior and options.
- [Clang documentation](https://clang.llvm.org/docs/) — compiler and analysis tools.
- [GNU Make manual](https://www.gnu.org/software/make/manual/) — build automation.
- [CMake documentation](https://cmake.org/documentation/) — alternative build system.
- [GDB manual](https://www.sourceware.org/gdb/current/onlinedocs/gdb/) — debugging.
- [AddressSanitizer](https://clang.llvm.org/docs/AddressSanitizer.html) — hosted-program memory bug detection before kernel work.
- [UndefinedBehaviorSanitizer](https://clang.llvm.org/docs/UndefinedBehaviorSanitizer.html) — hosted-program undefined-behavior detection.
- [Compiler Explorer](https://godbolt.org/) — inspect generated assembly; online tool, do not upload private code.
- [The Open Group Base Specifications](https://pubs.opengroup.org/onlinepubs/9799919799/) — POSIX interfaces for comparison; not all apply to a custom OS.

## 3. C++ systems programming
- [C++ reference](https://en.cppreference.com/w/) — language and standard-library reference.
- [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines) — design guidance.
- [Clang documentation](https://clang.llvm.org/docs/) — compiler and tooling.
- [GCC documentation](https://gcc.gnu.org/onlinedocs/) — compiler and language options.
- [Itanium C++ ABI](https://itanium-cxx-abi.github.io/cxx-abi/) — ABI reference used by many toolchains; target details matter.
- [LLVM](https://llvm.org/docs/) — compiler infrastructure documentation.
- [CMake](https://cmake.org/documentation/) — build system.
- [GDB](https://www.sourceware.org/gdb/current/onlinedocs/gdb/) — debugger.

C++ can be used in kernels, but runtime initialization, exceptions, RTTI, global constructors, allocator assumptions, and ABI choices must be controlled deliberately. It is not required for the recommended first path.

## 4. Rust for OS development
- [Writing an OS in Rust](https://os.phil-opp.com/) — current tutorial series.
- [blog_os source repository](https://github.com/phil-opp/blog_os) — accompanying code.
- [The Rust Book](https://doc.rust-lang.org/book/) — language foundations.
- [Rust Reference](https://doc.rust-lang.org/reference/) — language details.
- [Rust standard library documentation](https://doc.rust-lang.org/std/) — hosted environment reference.
- [`core` documentation](https://doc.rust-lang.org/core/) — core library without OS services.
- [Embedded Rust Book](https://docs.rust-embedded.org/book/) — embedded and `no_std` concepts.
- [Rustonomicon](https://doc.rust-lang.org/nomicon/) — unsafe Rust and invariants.
- [Rust by Example](https://doc.rust-lang.org/rust-by-example/) — examples.
- [Cargo book](https://doc.rust-lang.org/cargo/) — build tooling.
- [Rust target list](https://doc.rust-lang.org/rustc/platform-support.html) — target support.
- [Rust inline assembly](https://doc.rust-lang.org/reference/inline-assembly.html) — architecture-specific assembly interface.

The current Writing an OS in Rust series differs from its first edition. Follow one edition and its matching source tree; do not silently combine older bootloader instructions with the current edition.

## 5. Assembly, CPU architecture, and ABIs
- [Intel Software Developer Manuals](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html) — x86 and Intel 64 architecture.
- [AMD developer documentation](https://www.amd.com/en/search/documentation/hub.html) — AMD architecture manuals.
- [GNU assembler manual](https://sourceware.org/binutils/docs/as/) — GNU `as`.
- [NASM manual](https://www.nasm.us/doc/) — NASM syntax and directives.
- [x86-64 psABI project](https://gitlab.com/x86-psABIs/x86-64-ABI) — calling convention and ABI material.
- [RISC-V specifications](https://docs.riscv.org/) — ratified ISA and privileged specifications.
- [Arm documentation](https://developer.arm.com/documentation) — AArch64 and Arm architecture.
- [LLVM documentation](https://llvm.org/docs/) — compiler backend and IR materials.
- [GNU Binutils](https://sourceware.org/binutils/docs/) — assembler, linker, and binary inspection.
- [OSDev CPU Registers](https://wiki.osdev.org/CPU_Registers_x86) — community reference; cross-check details against architecture manuals.
- [OSDev GDT](https://wiki.osdev.org/Global_Descriptor_Table) — x86 descriptor concepts.
- [OSDev IDT](https://wiki.osdev.org/Interrupt_Descriptor_Table) — x86 interrupt descriptor table.
- [OSDev Paging](https://wiki.osdev.org/Paging) — paging overview.

## 6. Computer organization and digital logic
- [Nand2Tetris](https://www.nand2tetris.org/) — hardware/software hierarchy and projects.
- [RISC-V specifications](https://docs.riscv.org/) — ISA and privilege architecture.
- [Intel SDM](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html) — processor architecture.
- [AMD developer documentation](https://www.amd.com/en/search/documentation/hub.html) — processor architecture.
- [Arm documentation](https://developer.arm.com/documentation) — processor architecture.
- [Open Source Hardware Association](https://www.oshwa.org/) — open hardware context and resources.
- [QEMU](https://qemu-project.gitlab.io/qemu/system/) — explore emulated machines and devices.

## 7. Data structures, algorithms, and memory layout
- [OSTEP](https://pages.cs.wisc.edu/~remzi/OSTEP/) — OS-focused algorithms and mechanisms.
- [Open Data Structures](https://opendatastructures.org/) — freely available data structures text.
- [VisuAlgo](https://visualgo.net/en) — visual algorithm learning.
- [cppreference C](https://en.cppreference.com/w/c) — C language reference.
- [GNU C manual](https://www.gnu.org/software/c-intro-and-ref/manual/) — C fundamentals.
- [OSDev Memory Management](https://wiki.osdev.org/Memory_management) — allocator concepts.
- [OSDev Heap](https://wiki.osdev.org/Heap) — heap design overview.
- [Linux kernel linked-list documentation](https://docs.kernel.org/core-api/list.html) — intrusive-list patterns in Linux.

## 8. Compilers, linkers, build systems, and ELF
- [GCC](https://gcc.gnu.org/onlinedocs/) — compiler.
- [Clang](https://clang.llvm.org/docs/) — compiler.
- [GNU Binutils](https://sourceware.org/binutils/docs/) — `as`, `ld`, `objdump`, `readelf`, and related tools.
- [GNU ld linker scripts](https://sourceware.org/binutils/docs/ld/Scripts.html) — linker script language.
- [GNU Make](https://www.gnu.org/software/make/manual/) — build automation.
- [CMake](https://cmake.org/documentation/) — build generation.
- [ELF reference](https://refspecs.linuxfoundation.org/elf/elf.pdf) — ELF format reference.
- [x86-64 ABI](https://gitlab.com/x86-psABIs/x86-64-ABI) — ABI details.
- [LLVM](https://llvm.org/docs/) — compiler infrastructure.
- [NASM](https://www.nasm.us/doc/) — assembler.
- [GDB](https://www.sourceware.org/gdb/current/onlinedocs/gdb/) — symbols and debugging.
- [OSDev Meaty Skeleton](https://wiki.osdev.org/Meaty_Skeleton) — project organization example; check its assumptions before adopting it.

## 9. BIOS, UEFI, bootloaders, and boot protocols
- [UEFI specifications](https://uefi.org/specifications) — official firmware interface specifications; read-only access terms apply.
- [Limine protocol](https://github.com/limine-bootloader/limine-protocol) — modern boot protocol.
- [Limine bootloader](https://github.com/limine-bootloader/limine) — bootloader implementation.
- [GNU GRUB manual](https://www.gnu.org/software/grub/manual/) — bootloader documentation.
- [OSDev Boot Sequence](https://wiki.osdev.org/Boot_Sequence) — boot overview.
- [OSDev Bare Bones](https://wiki.osdev.org/Barebones) — minimal bootable kernel example.
- [OSDev Tutorials](https://wiki.osdev.org/Tutorials) — alternative tutorial paths.
- [TianoCore EDK II](https://github.com/tianocore/edk2) — UEFI development environment source.
- [TianoCore documentation](https://www.tianocore.org/) — EDK II documentation and project information.

## 10. Interrupts, exceptions, memory, processes, and concurrency
- [OSDev Interrupts](https://wiki.osdev.org/Interrupts) — interrupt overview.
- [OSDev IDT](https://wiki.osdev.org/Interrupt_Descriptor_Table) — x86 IDT.
- [OSDev Memory Management](https://wiki.osdev.org/Memory_management) — physical and virtual allocation concepts.
- [OSDev Memory Architecture](https://wiki.osdev.org/Memory_Architecture) — page tables and address spaces.
- [OSTEP](https://pages.cs.wisc.edu/~remzi/OSTEP/) — processes, concurrency, and memory.
- [MIT xv6 course](https://pdos.csail.mit.edu/6.1810/) — labs on core OS mechanisms.
- [xv6 source](https://github.com/mit-pdos/xv6-riscv) — implementation reference.
- [Pintos](https://web.stanford.edu/class/cs140/projects/pintos/) — threads, user programs, and VM projects.
- [Linux locking documentation](https://docs.kernel.org/locking/index.html) — production-kernel locking guidance.
- [Linux memory-management documentation](https://docs.kernel.org/mm/index.html) — real kernel memory subsystem.
- [Intel SDM](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html) — hardware details.
- [RISC-V specifications](https://docs.riscv.org/) — trap and privilege behavior.

## 11. Filesystems, block devices, and storage
- [OSDev File Systems](https://wiki.osdev.org/File_Systems) — filesystem overview.
- [OSDev ATA PIO](https://wiki.osdev.org/ATA_PIO_Mode) — legacy ATA access concepts; not a universal modern storage path.
- [OSTEP persistence](https://pages.cs.wisc.edu/~remzi/OSTEP/) — persistence and storage.
- [xv6 source](https://github.com/mit-pdos/xv6-riscv) — teaching filesystem.
- [Linux VFS documentation](https://docs.kernel.org/filesystems/vfs.html) — virtual filesystem layer.
- [Linux filesystem documentation](https://docs.kernel.org/filesystems/index.html) — subsystem references.
- [FAT specification resources](https://learn.microsoft.com/en-us/windows/win32/fileio/file-systems) — Windows filesystem documentation portal; check the specific format reference.
- [VirtIO specifications](https://docs.oasis-open.org/virtio/) — virtualized device interfaces.
- [QEMU disk image documentation](https://qemu-project.gitlab.io/qemu/system/images.html) — virtual disks and images.

## 12. Drivers, graphics, input, and hardware I/O
- [OSDev Wiki](https://wiki.osdev.org/Main_Page) — device and driver topics.
- [Linux driver API](https://docs.kernel.org/driver-api/index.html) — driver interfaces.
- [Linux device model](https://docs.kernel.org/driver-api/driver-model/index.html) — device model.
- [QEMU system emulation](https://qemu-project.gitlab.io/qemu/system/) — emulated hardware and configuration.
- [VirtIO specifications](https://docs.oasis-open.org/virtio/) — virtual device interfaces.
- [PCI-SIG](https://pcisig.com/specifications) — PCI/PCIe specifications; access terms may apply.
- [UEFI specifications](https://uefi.org/specifications) — firmware services and interfaces.
- [Intel SDM](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html) — architecture details.
- [RISC-V specifications](https://docs.riscv.org/) — alternative architecture.

## 13. Networking
- [OSDev Networking](https://wiki.osdev.org/Networking) — OS networking overview.
- [RFC Editor](https://www.rfc-editor.org/) — primary protocol specifications.
- [lwIP](https://www.nongnu.org/lwip/) — lightweight TCP/IP stack.
- [QEMU network devices](https://qemu-project.gitlab.io/qemu/system/devices/net.html) — emulator networking.
- [VirtIO specifications](https://docs.oasis-open.org/virtio/) — virtual network devices.
- [Linux networking documentation](https://docs.kernel.org/networking/index.html) — production stack reference.
- [IETF](https://www.ietf.org/standards/) — standards and protocol process.

## 14. Testing, debugging, emulation, and performance
- [QEMU system emulation](https://qemu-project.gitlab.io/qemu/system/) — full-system emulator.
- [QEMU GDB usage](https://www.qemu.org/docs/master/system/gdb.html) — remote debugging.
- [GDB manual](https://www.sourceware.org/gdb/current/onlinedocs/gdb/) — debugger.
- [Clang Static Analyzer](https://clang.llvm.org/docs/ClangStaticAnalyzer.html) — static analysis.
- [LLVM libFuzzer](https://llvm.org/docs/LibFuzzer.html) — coverage-guided fuzzing.
- [AddressSanitizer](https://clang.llvm.org/docs/AddressSanitizer.html) — hosted-code memory checking.
- [UndefinedBehaviorSanitizer](https://clang.llvm.org/docs/UndefinedBehaviorSanitizer.html) — hosted-code undefined behavior.
- [GitHub Actions](https://docs.github.com/actions) — automation/CI.
- [GNU Binutils](https://sourceware.org/binutils/docs/) — inspect binary artifacts.
- [Compiler Explorer](https://godbolt.org/) — inspect generated code; avoid private source.

## 15. Security, architecture alternatives, and research
- [MITRE CWE](https://cwe.mitre.org/) — weakness taxonomy.
- [NIST CSRC](https://csrc.nist.gov/) — security publications and guidance.
- [Linux kernel self-protection](https://docs.kernel.org/security/self-protection.html) — kernel hardening ideas.
- [seL4 documentation](https://docs.sel4.systems/) — microkernel and capabilities.
- [MINIX 3](https://www.minix3.org/) — microkernel system.
- [Xen documentation](https://xenproject.org/help/documentation/) — hypervisor.
- [FreeBSD documentation](https://docs.freebsd.org/) — mature BSD system.
- [Linux kernel documentation](https://docs.kernel.org/) — production kernel.
- [USENIX](https://www.usenix.org/publications) — systems research publications.
- [ACM Digital Library](https://dl.acm.org/) — research papers; some content may require access.
- [arXiv](https://arxiv.org/) — preprints; assess peer-review status independently.

## 16. Communities and reference hubs
- [OSDev Wiki](https://wiki.osdev.org/Main_Page) — community reference.
- [OSDev forum](https://forum.osdev.org/) — OS development discussion.
- [Linux kernel documentation](https://docs.kernel.org/) — developer documentation.
- [FreeBSD documentation](https://docs.freebsd.org/) — handbook and manuals.
- [Rust community](https://www.rust-lang.org/community) — Rust learning and discussion.
- [RISC-V International](https://riscv.org/) — ISA ecosystem.
- [TianoCore](https://www.tianocore.org/) — UEFI/EDK II project.
- [QEMU project](https://www.qemu.org/) — emulator project.
- [LLVM project](https://llvm.org/) — compiler infrastructure.

---

# Resource Collections by Format

## Free courses and lecture materials
- [MIT 6.1810](https://pdos.csail.mit.edu/6.1810/) — OS engineering using xv6-RISC-V.
- [MIT OCW 6.1810 syllabus](https://ocw.mit.edu/courses/6-1810-operating-system-engineering-fall-2023/pages/syllabus/) — OS course structure.
- [Nand2Tetris](https://www.nand2tetris.org/) — computer architecture and software hierarchy; free materials for non-profit use under site terms.
- [OSTEP](https://pages.cs.wisc.edu/~remzi/OSTEP/) — free online book with exercises and projects.

## Free textbooks and substantial tutorials
- [OSTEP](https://pages.cs.wisc.edu/~remzi/OSTEP/)
- [xv6 book source](https://github.com/mit-pdos/xv6-riscv-book)
- [Writing an OS in Rust](https://os.phil-opp.com/)
- [OSDev tutorials](https://wiki.osdev.org/Tutorials)
- [Nand2Tetris](https://www.nand2tetris.org/)
- [Open Data Structures](https://opendatastructures.org/)
- [GNU C manual](https://www.gnu.org/software/c-intro-and-ref/manual/)

## Official documentation and specifications
- [Intel SDM](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html)
- [AMD developer documentation](https://www.amd.com/en/search/documentation/hub.html)
- [Arm documentation](https://developer.arm.com/documentation)
- [RISC-V specifications](https://docs.riscv.org/)
- [UEFI specifications](https://uefi.org/specifications)
- [Limine protocol](https://github.com/limine-bootloader/limine-protocol)
- [GCC](https://gcc.gnu.org/onlinedocs/)
- [Clang](https://clang.llvm.org/docs/)
- [GNU Binutils](https://sourceware.org/binutils/docs/)
- [GDB](https://www.sourceware.org/gdb/current/onlinedocs/gdb/)
- [QEMU](https://qemu-project.gitlab.io/qemu/system/)
- [Linux kernel docs](https://docs.kernel.org/)
- [RFC Editor](https://www.rfc-editor.org/)

## Source repositories and educational kernels
- [xv6-RISC-V](https://github.com/mit-pdos/xv6-riscv)
- [xv6 book](https://github.com/mit-pdos/xv6-riscv-book)
- [Writing an OS in Rust code](https://github.com/phil-opp/blog_os)
- [Limine bootloader](https://github.com/limine-bootloader/limine)
- [Limine protocol](https://github.com/limine-bootloader/limine-protocol)
- [TianoCore EDK II](https://github.com/tianocore/edk2)
- [Linux kernel](https://git.kernel.org/)
- [FreeBSD source](https://github.com/freebsd/freebsd-src)
- [Zephyr RTOS](https://github.com/zephyrproject-rtos/zephyr)
- [seL4](https://github.com/seL4/seL4)

## Development and debugging tools
- [GCC](https://gcc.gnu.org/)
- [Clang/LLVM](https://clang.llvm.org/)
- [GNU Binutils](https://sourceware.org/binutils/)
- [GDB](https://www.gnu.org/software/gdb/)
- [QEMU](https://www.qemu.org/)
- [GNU Make](https://www.gnu.org/software/make/)
- [CMake](https://cmake.org/)
- [NASM](https://www.nasm.us/)
- [Git](https://git-scm.com/)
- [Limine](https://github.com/limine-bootloader/limine)
- [GRUB](https://www.gnu.org/software/grub/)

## Research and advanced reading
- [USENIX publications](https://www.usenix.org/publications)
- [ACM Digital Library](https://dl.acm.org/)
- [arXiv](https://arxiv.org/)
- [Linux kernel documentation](https://docs.kernel.org/)
- [FreeBSD documentation](https://docs.freebsd.org/)
- [seL4 documentation](https://docs.sel4.systems/)
- [Xen documentation](https://xenproject.org/help/documentation/)

---

# Project Ladder

Complete projects in order and keep every stage under version control.

| # | Project | Required evidence |
|---:|---|---|
| 1 | C systems library | Unit tests, compiler warnings enabled, memory ownership notes |
| 2 | Assembly inspection lab | C source, disassembly, ABI notes, debugger screenshots/logs |
| 3 | Toolchain smoke test | Reproducible freestanding build, linker map, ELF inspection |
| 4 | Bootable kernel | QEMU command, serial output, build instructions |
| 5 | Exception diagnostics | Controlled fault, saved-state report, regression test |
| 6 | Physical frame allocator | Invariant tests, boundary tests, exhaustion behavior |
| 7 | Virtual memory manager | Mapping tests, permission tests, page-fault diagnostics |
| 8 | Kernel heap | Alignment, fragmentation, exhaustion, double-free debug tests |
| 9 | Cooperative threads | Context-switch tests, stack isolation, lifecycle notes |
| 10 | Preemptive scheduler | Timer tests, fairness measurements, concurrency stress |
| 11 | User mode and syscalls | Normal calls, invalid numbers, invalid-pointer tests |
| 12 | ELF subset loader | Valid fixture and malformed executable tests |
| 13 | Filesystem | Persistence tests on disposable image, integrity checks |
| 14 | Shell and utilities | Command parsing tests and user-program demos |
| 15 | Device input | Keyboard/serial tests, timeout and missing-device behavior |
| 16 | Network experiment | Packet bounds tests and documented protocol subset |
| 17 | CI and regression suite | Clean build, automated boot, log assertions |
| 18 | Capstone release | Architecture notes, limitations, reproducible release artifact |

## Definition of Done

A project is complete only when:
- [ ] Its requirements and non-goals are written down.
- [ ] Its interfaces and invariants are documented.
- [ ] It builds from a clean checkout.
- [ ] It has normal-path and failure-path tests.
- [ ] Its output or behavior is reproducible.
- [ ] Known limitations are explicit.
- [ ] Dependencies and licenses are recorded.
- [ ] The next stage can build on it without hidden manual steps.

---

# Learning Schedules

These are example schedules, not guarantees. Progress depends on prior programming experience, weekly study time, hardware familiarity, and how ambitious the OS is.

## 3-month starter path
- **Weeks 1–3:** C pointers, data structures, Linux shell, Git, debugging.
- **Weeks 4–5:** Assembly basics, ABI, object files, ELF, computer architecture.
- **Weeks 6–7:** OSTEP core chapters and a teaching kernel.
- **Weeks 8–10:** Toolchain, boot protocol, QEMU, serial output, first kernel.
- **Weeks 11–12:** Kernel logging, panic path, basic exception investigation, project documentation.

**Goal:** A bootable minimal kernel and a solid foundation, not a complete OS.

## 6-month kernel development path
- **Month 1:** C, assembly, architecture, OS theory, toolchain.
- **Month 2:** Bootable kernel, exceptions, logging, GDB/QEMU.
- **Month 3:** Physical memory allocator and paging.
- **Month 4:** Kernel heap, context switching, cooperative tasks.
- **Month 5:** Scheduler, synchronization, user-mode experiment.
- **Month 6:** Minimal system calls, automated tests, documentation and cleanup.

**Goal:** A kernel with core memory and task mechanisms; scope depends on study hours.

## 12-month OS development path
- **Months 1–2:** Programming, architecture, OS theory, toolchain.
- **Months 3–4:** Boot, exceptions, memory management.
- **Months 5–6:** Processes, scheduling, synchronization.
- **Months 7–8:** User mode, syscalls, executable loading.
- **Months 9–10:** Filesystem, storage, shell, basic devices.
- **Months 11–12:** Tests, security review, capstone stabilization and release.

**Goal:** A more complete educational OS with explicit limitations.

## Long-term advanced path
Add SMP, networking, more devices, additional filesystems, dynamic linking, stronger isolation, fuzzing, performance work, real hardware support, and research reading only after the core system is stable.

---

# Architecture and Project Decision Guide

## x86-64 + C + Limine or GRUB
A practical PC-style path with extensive documentation and emulation. It requires attention to boot protocol, CPU mode, linker layout, ABI, and interrupt architecture. Prefer a maintained bootloader initially.

## RISC-V + C
A useful route for studying a relatively clean ISA and privileged architecture. MIT xv6-RISC-V is a strong complementary teaching kernel. It is a different architecture path; do not reuse x86 register or interrupt code.

## Rust + maintained bootloader
Useful if you already know Rust or specifically want to study systems programming with explicit safe/unsafe boundaries. It does not remove the need to understand CPU state, memory mapping, interrupts, synchronization, or hardware.

**Rule:** Choose one architecture and one primary implementation language for the first kernel. Portability and multiple-architecture support are later projects.

---

# Glossary

- **ABI:** Application Binary Interface; calling conventions, binary formats, and low-level interoperability rules.
- **Address space:** The set of virtual addresses available to a process or execution context.
- **BIOS:** Legacy PC firmware interface.
- **Bootloader:** Software that loads a kernel and provides boot-time information.
- **Buddy allocator:** Allocator that splits and merges blocks in power-of-two sizes.
- **Context switch:** Saving one execution context and restoring another.
- **Cross-compiler:** Compiler that generates code for a target different from the host.
- **DMA:** Direct Memory Access; device data transfer that can access memory under platform-specific rules.
- **Deadlock:** A state where tasks wait indefinitely for resources held by one another.
- **Device driver:** Kernel or system software that manages a device through its defined interface.
- **ELF:** Executable and Linkable Format, a common object and executable file format.
- **Exception:** A synchronous CPU event caused by instruction execution or a detected condition.
- **Filesystem:** Data structures and rules used to store and retrieve named files and metadata.
- **Firmware:** Platform software that initializes hardware and exposes early interfaces.
- **GDT:** Global Descriptor Table, an x86 structure relevant to segmentation and system state.
- **IDT:** Interrupt Descriptor Table, an x86 structure describing interrupt/exception entry points.
- **Interrupt:** An event that causes execution to transfer to a handler, often asynchronously.
- **Kernel:** The privileged core responsible for mechanisms such as scheduling, memory, and device access.
- **Kernel heap:** Kernel-managed allocator for variable-sized allocations.
- **Kernel panic:** A deliberate fatal stop when the kernel cannot safely continue.
- **Linker:** Tool that combines object files and resolves symbols/relocations.
- **Linker script:** Rules controlling output sections and memory layout during linking.
- **Memory-mapped I/O:** Device registers exposed through addressable memory ranges.
- **Microkernel:** Architecture that keeps a small privileged core and moves more services to user space.
- **MMU:** Memory Management Unit; hardware that translates addresses and enforces page permissions.
- **Mutex:** Mutual-exclusion primitive used to protect shared state.
- **Page fault:** Exception caused by an address translation or permission condition.
- **Page table:** Data structures used by the MMU to map virtual to physical addresses.
- **Physical allocator:** Kernel subsystem that allocates physical memory frames.
- **Process:** Execution and resource container, commonly with its own address space.
- **Race condition:** Behavior whose correctness depends on the timing/interleaving of concurrent operations.
- **Scheduler:** Kernel component selecting which runnable task executes next.
- **Semaphore:** Synchronization primitive using a counter and wait/signal operations.
- **SMP:** Symmetric multiprocessing; a system with multiple CPUs/cores sharing the OS.
- **Spinlock:** Lock where a contending CPU waits by repeatedly checking until the lock becomes available.
- **System call:** Controlled entry from user code into a kernel service.
- **TLB:** Translation Lookaside Buffer, a cache of address translations and permissions.
- **Trap:** Architecture-specific term for a controlled transfer into privileged handling; exact semantics depend on the ISA.
- **UEFI:** Modern firmware interface for boot and runtime services.
- **User space:** Execution environment with restricted privileges and address-space permissions.
- **Virtual memory:** Address translation and protection mechanisms that provide virtual address spaces.

---

# Progress Checklist

## Foundations
- [ ] I can explain the boot path and kernel boundary.
- [ ] I can write and debug modular C.
- [ ] I understand pointers, object lifetime, alignment, and bitwise operations.
- [ ] I can inspect assembly, symbols, and ELF metadata.
- [ ] I understand my chosen CPU architecture's execution modes and ABI.
- [ ] I can build a freestanding target reproducibly.

## Kernel core
- [ ] The kernel boots under an explicitly documented protocol.
- [ ] Serial logs and panic diagnostics work.
- [ ] Exceptions are handled or diagnosed deliberately.
- [ ] Physical and virtual memory invariants are tested.
- [ ] The heap passes stress and failure tests.
- [ ] Context switching and scheduling work predictably.
- [ ] User/kernel boundaries and syscall argument checks are tested.

## OS features
- [ ] A program can be loaded from an executable file.
- [ ] Filesystem operations pass normal and failure tests.
- [ ] A shell can launch user-space programs.
- [ ] Device behavior and unsupported hardware are documented.
- [ ] Optional networking/SMP features have regression tests.

## Engineering quality
- [ ] Clean builds and automated boot tests pass.
- [ ] The README explains setup, architecture, tests, and known limitations.
- [ ] Third-party code licenses are checked.
- [ ] Release artifacts are reproducible.
- [ ] The project does not claim production readiness without evidence.

---

# Contribution Guidelines

Contributions should improve accuracy, sequencing, accessibility, or practical value.

For each new resource, submit:
- Resource title and canonical URL.
- Subject and roadmap stage.
- Level and prerequisites.
- A concise description of the actual content.
- Practical labs, source code, or exercises included.
- Cost/access notes and registration requirements.
- License information when applicable; otherwise mark it **not verified**.
- Verification status and date checked.
- Why it adds value beyond existing resources.

Reject duplicates, fabricated URLs, misleading descriptions, unlicensed copies, and resources that are unrelated to the subject. If a tutorial is archived or architecture-specific, label that fact instead of silently presenting it as current.

## Resource status labels

- **Free access:** Read/use without payment under the resource's stated terms.
- **Open source:** Source code is available under a stated license; check the actual license.
- **Openly licensed:** The content has a license that permits specified reuse.
- **Registration required:** Account required, with cost/access conditions described.
- **Freemium/mixed:** Some materials or features may require payment.
- **Paid/restricted:** Payment or specific access conditions apply.
- **Archived:** Useful historical material that may be outdated.
- **Not verified:** Pricing, license, or current status has not been confirmed.

---

# Resource Verification and Maintenance

This README is a curated roadmap, not a claim that every available resource on the internet has been discovered. The research review date is **2026-10-25**. Several primary pages were checked for relevance and existence during preparation, but the full link set has not been independently click-tested end-to-end. Therefore, do not interpret every entry as individually verified on the review date.

For ongoing maintenance:

1. Open each canonical resource and verify that it resolves to the expected material.
2. Prefer official manuals, specifications, and original project repositories.
3. Check that architecture, ABI, boot protocol, compiler, and tutorial editions agree.
4. Mark outdated or archived tutorials; do not silently merge their commands into the primary path.
5. Check cost, registration, and license separately. Publicly accessible is not synonymous with open-licensed.
6. Avoid duplicate counting: one website may contain many distinct pages, but repeated links do not become separate resources merely by appearing in multiple categories.
7. Check Markdown anchors and table formatting after edits.
8. For code examples, test the toolchain and target where possible; state what was actually tested.
9. Record the date and scope of future reviews.
10. Remove broken or irrelevant entries instead of inventing replacements to meet a numeric target.

### Resource-count transparency

This directory prioritizes relevance and dependable starting points over a quota. It does **not** claim to contain 40 individually verified resources in every one of the 48 requested subject categories. Some subjects are narrow and authoritative resources are limited; many foundational resources legitimately apply to multiple topics. Expand each category only with distinct, relevant materials that can be verified.

---

# License and Disclaimer

- This README is educational guidance; external resources remain subject to their respective owners' licenses and terms.
- Do not copy source code, book chapters, course solutions, or specification text into your repository unless the applicable license permits it.
- Some official specifications are available for reading under restrictions that do not automatically permit redistribution or implementation. Check the terms for the particular document.
- CPU architecture manuals and boot-protocol specifications are authoritative for architecture-specific behavior. Tutorials can simplify or become outdated.
- OS development is platform-dependent. Code for one CPU, boot protocol, compiler, or emulator may not work unchanged with another.
- Kernel experiments can crash the guest or corrupt virtual disk images. Use disposable images, snapshots, and virtual machines.
- This project does not promise a production-ready operating system, compatibility with all hardware, or security certification.
- Test real hardware only after emulator behavior is stable and you understand the potential consequences.

---

## First Week: Start Here

1. Read [OSDev Getting Started](https://wiki.osdev.org/Getting_Started).
2. Begin [OSTEP](https://pages.cs.wisc.edu/~remzi/OSTEP/) and take notes on processes, memory, concurrency, and persistence.
3. If C is unfamiliar, work through [Beej's Guide to C](https://beej.us/guide/bgc/) and practice pointers and structs.
4. Choose one architecture and read its official architecture documentation as a reference, not as a cover-to-cover beginner text.
5. Install a Linux-based development environment, Git, compiler, Binutils, GDB, and QEMU using official documentation.
6. Create a repository and commit a small C program plus its tests.
7. Do not start the kernel until you can explain the build pipeline and debug a program with GDB.

**The principle of this roadmap:** understand a mechanism, implement a small version, test normal and failure cases, document the assumptions, and only then build the next layer.
