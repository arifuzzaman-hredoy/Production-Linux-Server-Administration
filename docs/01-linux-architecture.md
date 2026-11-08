# Linux Architecture

## 1. Overview

Linux is an open-source, Unix-like operating system widely used in production servers, cloud infrastructure, containers, and DevOps environments.

Linux architecture describes how applications interact with system resources through the Linux Kernel.

The Linux Kernel is responsible for managing hardware resources and providing essential services to user-space applications.

Understanding Linux architecture is important for system administration, server troubleshooting, performance monitoring, and production infrastructure management.

## 2. Main Components of Linux Architecture

The major components include:

1. Hardware
2. Linux Kernel
3. System Libraries
4. Shell
5. System Utilities
6. User Applications

### 2.1 Hardware

Hardware represents the physical resources of a computer system.

Examples:

- CPU
- RAM
- Storage Devices
- Network Interface Cards
- Other Peripheral Devices

### 2.2 Linux Kernel

The Linux Kernel is the core component of the Linux operating system.

It manages system resources and provides interfaces through which user-space applications request operating system services.

### 2.3 System Libraries

System libraries provide functions that applications can use to interact with operating system services.

For example, the GNU C Library (glibc) provides wrappers for many Linux system calls.

### 2.4 Shell

A shell provides a command-line interface through which users interact with the operating system.

Examples:

- Bash
- Zsh
- Fish

### 2.5 System Utilities

System utilities provide tools for managing and inspecting the Linux system.

Examples:

- `ls`
- `ps`
- `top`
- `df`
- `systemctl`
- `journalctl`

### 2.6 User Applications

User applications are programs that run in user space.

Examples:

- Nginx
- PostgreSQL
- Python Applications
- Docker CLI

## 3. Responsibilities of the Linux Kernel

The Linux Kernel manages several important system resources.

### 3.1 Process Management

The kernel manages process creation, scheduling, execution, and termination.

### 3.2 Memory Management

The kernel manages virtual memory, physical memory allocation, memory protection, and memory mapping.

### 3.3 CPU Scheduling

The kernel scheduler determines which runnable task gets CPU time.

### 3.4 Filesystem Management

The Virtual Filesystem (VFS) provides a common interface for different filesystem implementations.

### 3.5 Device Management

The kernel uses device drivers to communicate with hardware devices.

### 3.6 Network Management

The kernel provides networking functionality, including network protocols, sockets, and packet processing.

### 3.7 System Call Handling

The kernel provides system call interfaces through which user-space applications request kernel services.

## 4. User Space vs Kernel Space

Linux separates application execution from privileged kernel operations.

### 4.1 User Space

User Space is the execution environment where normal applications and processes run with restricted privileges.

Examples:

- Bash
- Nginx
- Python Applications
- PostgreSQL
- Docker CLI

Applications in user space cannot directly perform privileged kernel operations.

### 4.2 Kernel Space

Kernel Space is the privileged execution environment where the Linux Kernel performs operating system operations.

It includes:

- Process Scheduler
- Memory Management
- Filesystem Implementations
- Networking Stack
- Device Drivers

### 4.3 Comparison

| User Space | Kernel Space |
|---|---|
| Runs normal applications | Runs privileged kernel code |
| Has restricted privileges | Has kernel-level privileges |
| Includes Nginx and Bash | Includes scheduler and networking stack |
| Requests kernel services | Provides kernel services |
| Application failures are generally isolated | Kernel failures can affect the entire system |

**Important:** User Space and Kernel Space are logical execution and privilege environments, not separate disk partitions.

## 5. System Calls

A System Call is a mechanism through which a user-space application requests a service from the Linux Kernel.

System calls provide the interface between user-space applications and kernel services.

### 5.1 Why System Calls Are Required

System calls are required for operations such as:

- File access
- Process creation
- Memory management
- Network communication
- Device operations

### 5.2 Important System Calls

| System Call | Purpose |
|---|---|
| `openat()` | Open a file |
| `read()` | Read data |
| `write()` | Write data |
| `close()` | Close a file descriptor |
| `socket()` | Create a network socket |
| `bind()` | Associate a socket with an address |
| `listen()` | Listen for incoming connections |
| `accept()` | Accept a connection |
| `fork()` | Create a child process |
| `execve()` | Execute a program |

### 5.3 Example: Nginx

Nginx runs in user space.

When Nginx needs to read a website file or handle a network connection, it uses system calls to request services from the Linux Kernel.

The kernel performs the required operations and returns the result to the application.

## 6. The /proc Filesystem

The `/proc` filesystem is a virtual filesystem that exposes information about running processes and kernel-related system information.

It does not behave like a conventional filesystem storing ordinary persistent files on disk.

### 6.1 Important /proc Files

| Path | Information |
|---|---|
| `/proc/cpuinfo` | CPU information |
| `/proc/meminfo` | Detailed memory information |
| `/proc/uptime` | System uptime |
| `/proc/loadavg` | System load average |
| `/proc/mounts` | Mounted filesystems |
| `/proc/PID/` | Information about a specific process |

### 6.2 Practical Commands

Display CPU information:

```bash
cat /proc/cpuinfo
```

Display memory information:

```bash
cat /proc/meminfo
```

Display uptime:

```bash
cat /proc/uptime
```

Display load average:

```bash
cat /proc/loadavg
```

## 7. Memory Monitoring

Linux provides several tools for inspecting memory usage.

### 7.1 free -h

The `free -h` command displays a human-readable memory summary.

```bash
free -h
```

Important fields:

- Total
- Used
- Free
- Shared
- Buff/cache
- Available
- Swap

The `available` value is particularly useful when estimating how much memory can be used by new applications without significant swapping.

### 7.2 /proc/meminfo

The `/proc/meminfo` file provides detailed memory statistics.

```bash
cat /proc/meminfo
```

The `free -h` command presents a simplified summary using information provided by the kernel, primarily through `/proc/meminfo`.

## 8. Linux Load Average

Linux Load Average represents the average number of tasks that are either:

- Runnable or waiting to run on a CPU.
- In uninterruptible sleep, commonly while waiting for I/O.

Linux typically displays load averages for three periods:

| Value | Period |
|---|---|
| First | 1 minute |
| Second | 5 minutes |
| Third | 15 minutes |

### 8.1 Check Load Average

```bash
uptime
```

Or:

```bash
cat /proc/loadavg
```

### 8.2 Example

Assume a server has four logical CPUs.

A load average of:

```text
1.00, 1.20, 1.50
```

indicates an average load below the number of available logical CPUs.

A load average of:

```text
8.00, 7.50, 7.00
```

indicates a load significantly higher than the four logical CPUs.

High load average does not always mean high CPU utilization. I/O-related uninterruptible tasks can also increase load average.

Load average should be evaluated alongside CPU utilization, I/O wait, and process activity.

## 9. System Call Tracing with strace

`strace` is a diagnostic tool that traces system calls and signals made by a process.

It is useful for investigating application behavior and identifying system-call failures.

### 9.1 Check Installation

```bash
which strace
```

### 9.2 Display System Calls

```bash
strace ls
```

### 9.3 Display System Call Summary

```bash
strace -c ls
```

### 9.4 Trace File-Related System Calls

```bash
strace -e trace=%file ls
```

### 9.5 Trace Network-Related System Calls

```bash
strace -e trace=%network ls
```

### 9.6 Common System Call Results

Example:

```text
openat(AT_FDCWD, "/etc/example.conf", O_RDONLY) = 3
```

The file was successfully opened and returned file descriptor `3`.

Example:

```text
openat(AT_FDCWD, "/etc/missing.conf", O_RDONLY) = -1 ENOENT
```

The requested file does not exist.

Example:

```text
openat(AT_FDCWD, "/root/private.conf", O_RDONLY) = -1 EACCES
```

The operation failed because permission was denied.

**Important:** A failed system call does not necessarily mean the entire application has failed. Applications may perform optional file or configuration checks.

## 10. Production Troubleshooting

### 10.1 Nginx Permission Denied

If Nginx cannot read a website file, investigate:

Check service status:

```bash
sudo systemctl status nginx
```

Identify Nginx processes:

```bash
ps -eo user,pid,comm | grep nginx
```

Inspect file permissions:

```bash
ls -l /var/www/html/index.html
```

Inspect parent directory permissions:

```bash
namei -l /var/www/html/index.html
```

Test file readability as the Nginx worker user on Ubuntu:

```bash
sudo -u www-data test -r /var/www/html/index.html \
  && echo "Readable" \
  || echo "Not readable"
```

Use `strace` when further system-call investigation is necessary.

Do not apply broad permission changes without identifying the root cause.

### 10.2 High Memory Usage

Useful commands:

```bash
free -h
```

```bash
top
```

```bash
ps aux --sort=-%mem | head -10
```

```bash
cat /proc/meminfo
```

```bash
vmstat 1 5
```

These commands help identify memory pressure and resource-consuming processes.

### 10.3 Network Connection Problems

Useful investigation commands:

```bash
ip addr
```

```bash
ip route
```

```bash
sudo ss -tulpn
```

```bash
getent hosts example.com
```

```bash
curl -v https://example.com
```

For application-level system-call investigation:

```bash
strace -f -e trace=%network <application-command>
```

Network problems can originate from DNS, routing, firewall rules, application configuration, security groups, or remote services. Investigation should cover the entire network path.

## 11. Kernel Logs vs System Call Tracing

These tools serve different purposes.

| Tool | Purpose |
|---|---|
| `strace` | Trace system calls and signals |
| `dmesg` | Display kernel ring buffer messages |
| `journalctl -k` | Display kernel-related journal messages |
| `journalctl` | Query system journal logs |

Examples:

```bash
sudo dmesg
```

```bash
sudo journalctl -k
```

## 12. DevOps Relevance

Understanding Linux architecture helps DevOps Engineers:

- Investigate application failures.
- Understand process and memory behavior.
- Troubleshoot filesystem permission issues.
- Diagnose network connection problems.
- Monitor server resource utilization.
- Understand container interactions with the Linux Kernel.
- Investigate production performance issues.

Linux architecture knowledge is especially important when administering cloud servers, managing containers, and troubleshooting production workloads.

## 13. Interview Questions

1. What is the Linux Kernel?
2. What are the main responsibilities of the Linux Kernel?
3. What is the difference between User Space and Kernel Space?
4. What is a System Call in Linux?
5. How do User Space applications communicate with the Kernel?
6. Does Nginx run in User Space or Kernel Space?
7. What is the `/proc` filesystem?
8. What is the difference between `/proc/meminfo` and `free -h`?
9. What is Linux Load Average?
10. What is `strace`?
11. How would you investigate an Nginx permission issue?
12. Which commands would you use to investigate high memory usage?
13. How would you begin troubleshooting a network connection problem?

## 14. Key Takeaways

- The Linux Kernel is the core component of the Linux operating system.
- User Space runs normal applications with restricted privileges.
- Kernel Space handles privileged operating system operations.
- System calls provide an interface between applications and the kernel.
- `/proc` exposes virtual kernel and process information.
- `free -h` provides a human-readable memory summary.
- Load average measures runnable and uninterruptible tasks over time.
- `strace` helps investigate system calls and application-level failures.
- Production troubleshooting requires identifying the root cause before applying changes.

---

**Documentation Status:** Draft

**Environment Used:** WSL2 Ubuntu

**Target Production Environment:** Ubuntu Server 24.04 LTS

**Project:** Production Linux Server Administration
