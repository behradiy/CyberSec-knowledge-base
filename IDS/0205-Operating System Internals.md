## Inside the Castle Walls: An OS Internals Roadmap for Host-Based IDS Developers

*Part 5 of the [IDS developer roadmap](02-what%20should%20you%20learn.md) · ← [0204-Intrusion Detection Techniques](0204-Intrusion%20Detection%20Techniques.md) · [0206-Network Security Tools](0206-Network%20Security%20Tools.md) → · Resources last checked: November 2026*

A network IDS watches the roads; a host-based IDS (HIDS) stands guard *inside* each castle tower. To build one, you need to know how the operating system really works: how programs start, how they ask the kernel for help (system calls), and where the OS leaves footprints that a detector can read. This roadmap covers Linux and Windows, the two systems you'll defend most.

**Phase 1: Linux Internals (~2 Months):**

* **System calls:** the doorway between programs and the kernel. Almost everything a program does (opening files, starting processes, connecting to the network) passes through one.
* **Processes and threads:** how they are created (`fork`, `execve`), and what `/proc` tells you about each one.
* **Namespaces and cgroups:** the building blocks of containers, and why container escapes look the way they do.
* **auditd:** Linux's built-in audit system, and how to write rules that log suspicious system calls.

**Phase 2: eBPF, the Modern Way to Watch Linux (~2 Months):**

eBPF lets you run small, safe programs *inside* the kernel to observe events with very little overhead. It has become the foundation of modern Linux runtime security.

* **Hook points:** kprobes, tracepoints and LSM (Linux Security Module) hooks.
* **Tooling:** libbpf and CO-RE ("compile once, run everywhere"), and bpftrace for quick one-liners.
* **Real-world tools:** study how **Falco** (a CNCF graduated project), **Tetragon** (from the Cilium project) and **Tracee** use eBPF to detect threats.

**Phase 3: Windows Internals for Detection (2-3 Months):**

* **Processes, threads and access tokens:** who is running what, with which privileges.
* **Event Tracing for Windows (ETW):** the high-speed event stream that most Windows security tools rely on.
* **Sysmon:** Microsoft's free system monitor, which logs process creation, network connections, file changes and more. It is now also available as a built-in optional feature in Windows 11 and Windows Server 2025, and there is a Linux version too.
* **Windows event logs and audit policy:** which Security log events matter (logons, privilege use, process creation).
* **Kernel callbacks and minifilters:** how security products get notified when processes start or files change.
* **The post-CrowdStrike shift:** after the July 2024 outage, Microsoft began building the Windows Endpoint Security Platform so security vendors can run outside the kernel. As of November 2026 it is still in preview for security partners, with no public SDK yet. See [03-AI meets network detection](03-AI%20meets%20network%20detection.md) for the story.

**Phase 4: Putting It Together with Real HIDS Tools (1-2 Months):**

* **Wazuh and OSSEC:** file-integrity monitoring, log analysis and rule writing.
* **osquery:** query your operating system as if it were a database (for example, "show me all processes listening on a port").
* **Validation:** run safe attack simulations and check what each tool caught and what it missed.

**Resources:**

* **Free Online Courses and Documentation:**
    * [OpenSecurityTraining2](https://p.ost2.fyi/) (free, in-depth classes such as x86-64 OS internals, Windows kernel internals and WinDbg)
    * [ebpf.io – What is eBPF?](https://ebpf.io/what-is-ebpf/) and [Get Started](https://ebpf.io/get-started/) (free introduction, labs and tutorials)
    * [Brendan Gregg's eBPF tracing tools](https://www.brendangregg.com/ebpf.html) (free)
    * [Sysmon documentation](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon) and [About Event Tracing (ETW)](https://learn.microsoft.com/en-us/windows/win32/etw/about-event-tracing) on Microsoft Learn (free)
    * Linux man pages: [syscalls(2)](https://man7.org/linux/man-pages/man2/syscalls.2.html), [proc(5)](https://man7.org/linux/man-pages/man5/proc.5.html), [namespaces(7)](https://man7.org/linux/man-pages/man7/namespaces.7.html), [auditd(8)](https://man7.org/linux/man-pages/man8/auditd.8.html)
* **Books:**
    * **Windows Internals, Part 1, 7th Edition by Pavel Yosifovich, Mark Russinovich, Alex Ionescu and David Solomon** (Microsoft Press, 2017): processes, threads, memory and security. [Publisher page](https://www.microsoftpressstore.com/store/windows-internals-part-1-system-architecture-processes-9780735684188)
    * **Windows Internals, Part 2, 7th Edition by Andrea Allievi, Mark Russinovich, Alex Ionescu and David Solomon** (Microsoft Press, 2021): including ETW and system mechanisms. [Publisher page](https://www.microsoftpressstore.com/store/windows-internals-part-2-9780135462409)
    * **The Linux Programming Interface by Michael Kerrisk** (No Starch Press, 2010): system calls, processes and everything in between. [Publisher page](https://nostarch.com/tlpi)
    * **Learning eBPF by Liz Rice** (O'Reilly, 2023): the friendliest introduction to eBPF programming.
    * **BPF Performance Tools by Brendan Gregg** (Addison-Wesley, 2019): the deep reference on eBPF tracing. [Publisher page](https://www.informit.com/store/bpf-performance-tools-9780136554820)
    * **Evading EDR by Matt Hand** (No Starch Press, 2023): how endpoint detection really works, explained by showing how attackers bypass it. [Publisher page](https://nostarch.com/evading-edr)
* **Practice:**
    * [Wazuh quickstart](https://documentation.wazuh.com/current/quickstart.html) (free, open source: deploy a HIDS in your lab)
    * [Write your first Falco rule](https://falco.org/docs/concepts/rules/) and try [Tetragon tracing policies](https://tetragon.io/docs/)
    * **Configure Sysmon** with the actively maintained [sysmon-modular](https://github.com/olafhartong/sysmon-modular) configuration
    * **Query your system with [osquery](https://osquery.readthedocs.io/en/stable/)**
    * **Run [Atomic Red Team](https://github.com/redcanaryco/atomic-red-team) tests in a lab VM** and compare what Sysmon, auditd, osquery and Falco each recorded

**Beyond the Roadmap:**

* **Learn a debugger:** WinDbg on Windows and gdb on Linux will teach you the OS faster than any book.
* **Read the attacker's side:** understanding EDR evasion (see *Evading EDR*) makes you a much better detection developer.
* **Watch the kernel debate:** the move from kernel drivers to eBPF on Linux and to user-mode APIs on Windows will shape host-based detection for years.

**Wishing you the best of luck!** With the host covered, it's time to master the tools that watch the network in [0206-Network Security Tools](0206-Network%20Security%20Tools.md).
