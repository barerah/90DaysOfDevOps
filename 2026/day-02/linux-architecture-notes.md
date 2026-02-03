# Day 02: Linux Architecture, Processes, and systemd

## 1. The Core Components
Linux isn't one giant block of code; it’s a stack of three layers working together:

* **The Kernel (The "Boss"):** The only part that touches the hardware. If a program needs memory or CPU time, it has to ask the Kernel nicely.
* **User Space (The "Playground"):** Where my tools (`ls`, `cd`, `git`) and apps (`Nginx`, `Docker`) live. They are isolated so they can't crash the whole system.
* **systemd (The "Manager"):** The first process that starts (**PID 1**) when the server boots. It handles all other services and keeps them running.

---

## 2. Processes (Programs in Action)
A process is simply a running program. Every command you run becomes a process.
Each process has:
* **PID:** Process ID
* **Memory Usage**
* **CPU Time**

### How a Process is Created
1.  You run a command.
2.  The Kernel creates a process.
3.  The process runs its task.
4.  The process exits (or keeps running in the background).

### Process States (Very Important)
* **Running:** Actively using the CPU.
* **Sleeping:** Waiting for something (disk, network, user input). This is the normal state for most processes.
* **Stopped:** Paused manually (usually for debugging).
* **Zombie:** The process finished, but the parent didn't clean it up. They are "ghost" tasks that show bad system hygiene. 🧟‍♂️

---

## 3. Why systemd is the DevOps Superpower
Before systemd, starting services was a slow, manual mess. systemd matters because:

* **Parallelism:** It starts services at the same time, making servers boot in seconds.
* **Self-Healing:** It can be configured to automatically restart an app if it crashes at 3 AM.
* **Central Logs:** All service errors are sent to one place (`journalctl`), so I don't have to hunt for hidden log files.

---

## 4. My "Battle-Tested" Daily Commands
I use these 5 commands to see exactly what is happening inside the OS:

1.  **`top` / `htop`:** My live dashboard to see which app is "eating" the CPU.
2.  **`ps auxf`:** The "Family Tree." Shows every process and who started it.
3.  **`systemctl status <service>`:** The "Health Check." Tells me if a service is alive or failed.
4.  **`kill -9 <PID>`:** The "Force Quit." Shuts down frozen processes immediately.
5.  **`journalctl -u <service> -n 50`:** The "Time Machine." Shows the last 50 log lines to explain a crash.

---

## The DevOps Difference
A regular user sees a "Server Error." A DevOps engineer sees a **Zombie Process** or a **failed systemd unit**. Understanding these basics turns "I don't know why it's broken" into "I know exactly which process to kill."
