# Linux Directory Structure

Below is an overview of the most important directories you'll find in a typical Linux system

/ is the root directory. Every other directory is part of /

![alt text](linux-structure.jpg)

**/bin:**

The /bin directory in Linux stands for "binary" and contains essential command-line programs (executables) that are required for basic system functionality, especially during boot or recovery.
Example: ls, cp, rm, mkdir, etc.

We can see the symlink /bin → /usr/bin.

**/dev:**

The /dev directory in Linux is a special directory that contains device files, also called device nodes. These files are interfaces to hardware devices, allowing programs and users to interact with devices using standard file operations like read/write.

* Linux treats everything as a file, including hardware devices.
* The files in /dev are not regular files; they are special device files created and managed by the kernel or the udev device manager.

Device File	Purpose
/dev/sda	First SATA hard disk (entire disk)
/dev/sda1	First partition on the first disk
/dev/sr0	First CD/DVD-ROM drive
/dev/null	Discards all data written to it (bit bucket)
/dev/tty	Terminal device (current session)
/dev/usb	USB device interface (usually managed by subdirs)
/dev/input/eventX Keyboard
/dev/input/mice Mouse

**/boot:**

The /boot directory contains all the files needed to boot the Linux operating system. These files are used very early in the startup process, even before the main filesystem is mounted.

**/etc:**

The /etc directory in Linux contains system-wide configuration files and directories. These files control the behavior of the system, services, and installed applications.
For example
* /etc/passwd
* /etc/group
* /etc/systemd/system

**/home:**

The /home directory is where all regular users’ personal files and settings are stored. Each user gets a subdirectory inside /home, named after their username.

User settings	Hidden "dotfiles" like .bashrc, .profile, .config/ store per-user settings.

**lib:**

The /lib directory contains essential shared libraries needed by the core system binaries in /bin and /sbin. These libraries are similar to .dll files in Windows or .dylib in macOS.

For example When you run a basic command like ls, the system:
1. Finds /bin/ls
2. Loads necessary libraries from /lib
3. Executes the binary with support from those libraries

**/lib64:**

The /lib64 directory contains 64-bit shared libraries that are essential for running 64-bit binaries, especially those located in /bin and /sbin.

On 64-bit Linux systems, libraries are separated by architecture to avoid conflicts:

Directory	    Purpose
/lib	        Contains 32-bit libraries (on multilib systems)
/lib64	        Contains 64-bit libraries (default on 64-bit systems)

This separation helps the OS load the correct library version depending on whether a program is 32-bit or 64-bit.

**/media:**

The /media directory is used to automatically mount external storage devices such as:

* USB drives
* CD/DVDs
* SD cards
* External hard drives

When a user named alice inserts a USB named MyUSB, the system might mount it as:

```shell
/media/alice/MyUSB/
```
Inside that directory, you’ll see the contents of the USB.
**NOTE:** This media is mostly for desktop auto mount environments. It is like plug and play as we see in windows.

**Flow When You Plug in a USB:**

* Linux detects hardware and assigns it a device file like /dev/sdb1
* An auto-mount tool mounts it under /media/username/MyUSB/
* You access your files through /media, not /dev

**/mnt:**

The /mnt directory is a standard mount point used for temporarily mounting filesystems, such as:

* External hard drives
* USB drives (manually mounted)
* Network filesystems (NFS, SMB)
* ISO images

It’s primarily intended for manual or admin use.

Manual Mounts	    Used by system administrators to mount filesystems temporarily
No Auto-mounting	Unlike /media, this is not used by desktop auto-mount tools
Testing or recovery	Often used during system recovery or file system testing

**/opt:**

The /opt directory stands for “optional” and is used to install third-party software that is not part of the default Linux distribution.

Feature	            Description
💾 Third-party apps	Used for software not managed by the package manager
🗂️ Self-contained	Each application has its own subdirectory, keeping things isolated
📥 Manual installs	Ideal for software downloaded directly (e.g., Chrome, MATLAB)

```bash
/opt/
├── google/
│   └── chrome/
├── myapp/
│   ├── bin/
│   ├── lib/
│   └── config/
```

**When to Use /opt**

* Installing commercial software (e.g., Oracle, VMware, Zoom)
* Managing applications outside the OS package manager (apt, yum, dnf)
* Deploying self-contained apps or custom-built binaries

**/proc:**

The /proc directory is a virtual filesystem (not a real one on disk) that provides a real-time interface to kernel and process information.

Think of it as a window into the running Linux kernel.

| Path            | Description                          |
| --------------- | ------------------------------------ |
| `/proc/cpuinfo` | Info about CPU(s)                    |
| `/proc/meminfo` | Memory usage                         |
| `/proc/uptime`  | System uptime                        |
| `/proc/version` | Kernel version                       |
| `/proc/[PID]/`  | Info about a running process by PID  |

**/root:**

The /root directory is the home directory for the root user, who is the superuser (administrator) on a Linux system.

Do not confuse /root with the root (/) directory — / is the root of the entire filesystem, while /root is the root user’s home folder.

**/run:**

The /run directory is a temporary filesystem used to store runtime data for processes and services after boot, such as:

* Process ID (PID) files
* Lock files
* Socket files

| Feature                   | Description                                                       |
| ------------------------- | ----------------------------------------------------------------- |
| ⚡ **Volatile**            | Stored in **RAM** (`tmpfs`) — **cleared at every reboot**         |
| 🧰 **Used by services**   | Systemd, udev, network manager, and other daemons store data here |
| 🕒 **Early availability** | Available **very early in boot**, even before `/var` is mounted   |

* /proc/<pid>/: Shows live process data (real-time, kernel-managed).
* /run/*.pid: Text files created by services to store their own PID for tracking or control.

**/sbin:**

The /sbin directory contains essential system binaries that are primarily intended for the system administrator (root). These binaries are crucial for system boot, repair, and maintenance.

Example: fsck, iptables, reboot, iptables, init, etc.

**/srv:**

The /srv directory stands for "service" and is used to store data served by system services such as:

* Web servers (httpd, nginx)
* FTP servers
* File sharing services

Often structured like /srv/ftp, /srv/www, /srv/nfs

Not Always Used
* Optional: Not all Linux distributions or setups use /srv.
* Customizable: Some admins prefer /var/www, /usr/share/nginx/html, /home/ftp, or mount points elsewhere.

**/swap:**

In Linux, /swap is not a directory, but rather a swap space used as virtual memory to help manage the system's memory resources.

Swap space can be located either in a swap partition (a dedicated disk partition) or a swap file (a file on the filesystem). It allows the system to offload less frequently used memory pages when the physical RAM is full.

**/tmp:**
The /tmp directory is used for temporary file storage. It's a standard location where programs and users can store temporary files that are required only for short-term use.


🗑️ Temporary Storage	Used to store temporary files created by programs or users
🛠️ Program usage	    Programs often create files here that they don’t need permanently
📂 Writable by all	   It’s a world-writable directory, meaning any user or process can create files

Many systems are configured to clear the contents of /tmp on reboot, or periodically through cron jobs.

**/usr:**

The /usr directory is one of the core system directories in Linux. It contains user-related programs and data that are not required for the basic system to run, but are essential for most applications.

📂 Common Subdirectories of /usr
Directory	Description
/usr/bin	User binaries - contains most executable programs for general users
/usr/lib	Libraries - stores shared libraries required by programs
/usr/include	Contains C header files for development
/usr/share	Contains shared data, such as documentation and icons
/usr/local	Locally installed software (software manually installed by the admin)