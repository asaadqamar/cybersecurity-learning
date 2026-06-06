# Linux Introduction

## Overview

Linux is the dominant operating system in cybersecurity, cloud infrastructure, and server environments. Understanding Linux fundamentals is essential for any security professional.

## What is Linux?

**Linux** is a free, open-source Unix-like operating system kernel created by Linus Torvalds in 1991. It powers:

- **Servers** — Web servers, databases, application servers
- **Cloud Infrastructure** — AWS, Azure, Google Cloud
- **Mobile Devices** — Android
- **IoT Devices** — Smart home, industrial systems
- **Security Tools** — Kali Linux, penetration testing platforms

## The Linux Filesystem

### Filesystem Hierarchy

Linux organizes files and directories in a hierarchical tree structure starting from root (`/`).

### Core Directories

| Directory | Purpose |
|-----------|---------|
| **/** | Root directory (everything starts here) |
| **/bin** | Essential user commands (ls, cat, grep) |
| **/sbin** | System administration commands (mount, fdisk) |
| **/etc** | Configuration files and system settings |
| **/home** | User home directories (/home/username) |
| **/root** | Root user's home directory |
| **/tmp** | Temporary files (cleaned on reboot) |
| **/var** | Variable data (logs, databases, mail) |
| **/usr** | User programs and libraries |
| **/usr/bin** | User commands |
| **/usr/sbin** | User system commands |
| **/usr/local** | Locally installed software |
| **/lib, /lib64** | System libraries |
| **/dev** | Device files (hard drives, terminals) |
| **/proc** | Virtual filesystem with process info |
| **/opt** | Optional software packages |
| **/boot** | Boot loader and kernel files |

### Absolute vs Relative Paths

**Absolute Path:** Full path from root directory
```bash
/home/username/documents/report.txt
```

**Relative Path:** Path from current directory
```bash
documents/report.txt          # If in /home/username
../other_user/file.txt        # Go up one directory, then down
```

## First Commands

### Navigation Commands

#### pwd (Print Working Directory)
Shows current directory location.

```bash
$ pwd
/home/username
```

#### cd (Change Directory)
Changes current directory.

```bash
$ cd /home/username/documents    # Change to specific directory
$ cd ..                          # Go up one directory
$ cd ~                           # Go to home directory
$ cd -                           # Go to previous directory
```

#### ls (List)
Lists directory contents.

```bash
$ ls                      # List files and directories
$ ls -l                   # Long format with details
$ ls -la                  # Include hidden files (dotfiles)
$ ls -lh                  # Human-readable file sizes
$ ls /tmp                 # List contents of /tmp
```

### File Commands

#### cat (Concatenate)
Displays file contents.

```bash
$ cat /etc/hostname        # Show system hostname
$ cat file1.txt file2.txt  # Combine two files
```

#### less / more
View file contents page-by-page.

```bash
$ less /var/log/auth.log   # View log file (q to quit)
```

#### file
Determines file type.

```bash
$ file /bin/ls             # Shows: ELF 64-bit executable
$ file /etc/passwd         # Shows: ASCII text
```

#### cp (Copy)
Copies files or directories.

```bash
$ cp file.txt file_backup.txt        # Copy file
$ cp -r directory/ directory_backup/ # Copy directory recursively
```

#### mv (Move)
Moves or renames files.

```bash
$ mv oldname.txt newname.txt              # Rename
$ mv file.txt /tmp/                       # Move to /tmp
$ mv /tmp/file.txt /home/username/        # Move between directories
```

#### rm (Remove)
Deletes files or directories.

```bash
$ rm file.txt              # Delete file
$ rm -r directory/         # Delete directory and contents (recursive)
$ rm -f file.txt           # Force delete without confirmation
```

#### touch
Creates empty file or updates modification time.

```bash
$ touch newfile.txt        # Create empty file
$ touch existing.txt       # Update timestamp if file exists
```

### Directory Commands

#### mkdir (Make Directory)
Creates directories.

```bash
$ mkdir newdirectory               # Create single directory
$ mkdir -p parent/child/grandchild # Create nested directories
```

#### rmdir (Remove Directory)
Removes empty directories.

```bash
$ rmdir emptydirectory     # Only works if directory is empty
$ rm -r directory/         # Use rm -r to delete non-empty
```

### File Permissions

#### Ownership & Permissions

Every file has owner, group, and permissions (rwx):

```bash
-rw-r--r-- 1 john developers 4096 Jan 15 10:30 document.txt
```

Breaking down: `-rw-r--r--`
- First character: `-` (file), `d` (directory), `l` (symlink)
- Next three: `rw-` (owner: read, write, no execute)
- Next three: `r--` (group: read only)
- Last three: `r--` (others: read only)

#### chmod (Change Mode)
Changes file permissions.

```bash
$ chmod 755 script.sh       # rwxr-xr-x (common for scripts)
$ chmod 644 file.txt        # rw-r--r-- (common for files)
$ chmod +x script.sh        # Add execute permission
$ chmod -w file.txt         # Remove write permission
```

Permission Values:
- `r` (read) = 4
- `w` (write) = 2
- `x` (execute) = 1

#### chown (Change Owner)
Changes file owner and group.

```bash
$ chown username file.txt              # Change owner
$ chown username:groupname file.txt    # Change owner and group
$ chown -R username directory/         # Recursive change
```

#### chgrp (Change Group)
Changes file group.

```bash
$ chgrp developers file.txt     # Change group
```

### Text Processing

#### grep (Global Regular Expression Print)
Searches for text patterns.

```bash
$ grep "error" /var/log/syslog         # Find lines with "error"
$ grep -i "error" /var/log/syslog      # Case-insensitive search
$ grep -n "error" /var/log/syslog      # Show line numbers
$ grep -c "error" /var/log/syslog      # Count matches
```

#### sed (Stream Editor)
Processes and edits text streams.

```bash
$ sed 's/old/new/' file.txt            # Replace first occurrence
$ sed 's/old/new/g' file.txt           # Replace all occurrences
$ sed -n '5,10p' file.txt              # Print lines 5-10
```

#### awk
Processes columns and fields.

```bash
$ awk '{print $1}' file.txt            # Print first column
$ awk '{print $1, $3}' file.txt        # Print 1st and 3rd columns
```

#### sort
Sorts lines in file.

```bash
$ sort file.txt                        # Sort alphabetically
$ sort -n file.txt                     # Sort numerically
$ sort -u file.txt                     # Sort and remove duplicates
```

### System Information

#### uname
Displays system information.

```bash
$ uname -a        # All system information
$ uname -r        # Kernel release
$ uname -s        # Kernel name
```

#### whoami
Shows current user.

```bash
$ whoami           # Outputs: username
```

#### id
Shows user ID and groups.

```bash
$ id               # Current user info
$ id username      # Info for specific user
```

#### df
Shows disk space usage.

```bash
$ df -h            # Human-readable format
$ df /home         # Disk usage for /home
```

#### du
Shows directory size.

```bash
$ du -h directory/ # Show directory size
$ du -sh directory # Total size (summary)
```

#### ps
Shows running processes.

```bash
$ ps              # Current user processes
$ ps aux          # All processes (verbose)
$ ps aux | grep ssh  # Find ssh processes
```

#### top
Real-time process monitor.

```bash
$ top             # Interactive process viewer
```

#### free
Shows memory usage.

```bash
$ free -h         # Human-readable memory info
```

## Users and Groups

### User Management

#### User Database
User information stored in `/etc/passwd`:

```
username:password_hash:uid:gid:comment:home_directory:login_shell
john:x:1000:1000:John Smith:/home/john:/bin/bash
```

#### Adding Users
```bash
$ useradd newuser              # Create new user
$ useradd -m -s /bin/bash newuser  # Create with home and shell
$ passwd newuser               # Set password
```

#### Removing Users
```bash
$ userdel username             # Delete user
$ userdel -r username          # Delete with home directory
```

### Group Management

#### Groups
Users can belong to multiple groups for permission management.

```bash
$ groups               # Show current user's groups
$ groups username      # Show user's groups
$ cat /etc/group       # View all groups
```

#### Adding Users to Groups
```bash
$ usermod -aG groupname username    # Add user to group
$ usermod -G group1,group2 username # Set user's groups
```

## File Permissions in Security

### Critical Security Files

| File | Purpose | Permissions |
|------|---------|------------|
| /etc/passwd | User database | 644 (world readable) |
| /etc/shadow | Password hashes | 600 (root only) |
| /etc/sudoers | Sudo configuration | 440 (root only) |
| /root/.ssh/id_rsa | Private SSH key | 600 (owner only) |
| /root/.ssh/id_rsa.pub | Public SSH key | 644 (world readable) |

### Permission Best Practices

- **Never use 777:** Excessive permissions expose files
- **Principle of Least Privilege:** Grant minimum permissions needed
- **Protect Private Keys:** SSH keys must be 600 or 400
- **Separate Sensitive Files:** Use directories with restricted permissions
- **Monitor Changes:** Use file monitoring tools (aide, tripwire)

## Shell Basics

### What is a Shell?

A **shell** is a command interpreter that translates user commands into system operations. Common shells:

- **bash** (Bourne Again Shell) — Most common
- **sh** (Bourne Shell) — Original Unix shell
- **zsh** (Z Shell) — Enhanced with features
- **fish** — User-friendly shell

### Command Basics

#### Pipes
Send output of one command as input to another.

```bash
$ cat /var/log/syslog | grep "error" | wc -l
```

#### Redirection
Redirect output to files.

```bash
$ ls > output.txt          # Redirect stdout to file
$ command 2> errors.txt    # Redirect stderr to file
$ command &> output.txt    # Redirect both stdout and stderr
$ cat input.txt | command  # Pipe input
```

#### Background Processes
Run commands without blocking terminal.

```bash
$ long_command &           # Run in background
$ jobs                     # List background jobs
$ fg %1                    # Bring job 1 to foreground
$ Ctrl+Z                   # Suspend current job
$ bg %1                    # Resume job 1 in background
```

## Security Implications

### Local Privilege Escalation
- Weak file permissions allow unauthorized file access
- Poor sudo configuration enables privilege escalation
- Defense: Regular permission audits, minimize sudo privileges

### User Enumeration
- `/etc/passwd` is world-readable, revealing all usernames
- Valid usernames aid attacker reconnaissance
- Defense: Disable unnecessary user accounts

### Setuid Binaries
- Programs with setuid bit run as owner (often root)
- Vulnerable setuid programs = privilege escalation vector
- Defense: Audit setuid binaries, apply patches

---

## Key Takeaways

✅ Understand Linux filesystem hierarchy for navigation and security
✅ Master essential commands for daily administration
✅ Permissions and ownership are fundamental to Linux security
✅ Proper file permissions prevent unauthorized access
✅ Users and groups enable granular access control

---

## Next Steps

- Practice navigation and file operations
- Set up your Linux home lab
- Learn about processes and services
- Explore networking tools (ifconfig, netstat, ss)
