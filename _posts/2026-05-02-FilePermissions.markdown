---
layout: post
title:  "File Permissions and Privilege Control Explained for Beginners"
date:   2025-05-02 07:15:00 -0500
author: Ravi Shankar Gurram
categories: ['Linux']
background: '/images/package management/package management.png'
---

# Linux Security Essentials: File Permissions and Privilege Control

**What You'll Learn:** This guide demystifies Linux security for complete beginners. We'll explain file permissions, access control, and how Linux protects important files—without assuming any IT knowledge. By the end, you'll understand how Linux keeps your system secure.

---

## Part 1: The Big Picture—Why Linux Security Matters

Imagine a filing cabinet in an office. The owner of the cabinet (the boss) decides who can open it, who can read the documents, and who can make changes. Some employees get read-only access, others can modify documents, and some shouldn't open it at all.

**Linux works exactly the same way.** Every file and directory on a Linux system has an owner and permissions that control who can read, modify, or execute it. This system is called **access control**, and it's the foundation of Linux security.

There are two main approaches to access control in Linux:

### Discretionary Access Control (DAC)
**The owner decides the rules.** Just like a file cabinet owner, the person who owns a Linux file decides who gets access. This is flexible but requires trust in the owner.

*Example: You own a document, so you decide if your colleague can read or edit it.*

**Pros:** Flexible, easy to manage  
**Cons:** Requires trust in owners, less secure

### Mandatory Access Control (MAC)
**The system enforces the rules.** A system administrator sets strict policies that even the owner can't override. This is more secure but less flexible.

*Example: A bank's security system doesn't allow anyone to access customer data, even the data owner.*

**Pros:** Very secure, enforced consistency  
**Cons:** Complex, less flexible

Most home and business Linux systems use DAC (easier to manage). Secure servers often use MAC (stronger protection). We'll focus on DAC first, since that's what most people use.

---

## Part 2: Understanding File Permissions

### The Three Types of Permissions

In Linux, every file has three basic permissions:

#### 📖 **Read (r)**
Allows viewing the contents of a file or listing what's inside a folder. Think of it as looking at a document without changing it.

#### ✏️ **Write (w)**
Allows creating, editing, or deleting the file. It's like having a pen to mark up a document.

#### ⚙️ **Execute (x)**
Allows running a file (if it's a program) or entering a folder. For files, it's like having permission to turn a recipe into a meal.

### Three Categories of Users

Permissions are assigned to three different groups:

| Category | Who It Means | Real-World Example |
|----------|-------------|-------------------|
| **Owner (u)** | The person who created or owns the file | You created the document, so you're the owner |
| **Group (g)** | A named collection of users (like a department) | All members of the "Sales Team" group |
| **Others (o)** | Everyone else on the system | Random users who aren't the owner or in the group |

**💡 Tip:** Think of permissions like a theater. The owner is the theater manager (has all access), the group is the staff (backstage access), and others are the public (only front-of-house access).

### Reading Permission Symbols

When you list files in Linux, you might see something like this:

```bash
$ ls -l
-rw-r--r-- user group file.txt
```

Let's decode this:

```
-rw-r--r--

- = Regular file (not a folder)
rw- = Owner can read & write (not execute)
r-- = Group can read (nothing else)
r-- = Others can read (nothing else)
```

---

## Part 3: Managing Permissions with Commands

### The chmod Command (Change Mode)

**chmod** changes file permissions. There are two ways to use it: symbolic mode (using letters) and numeric mode (using numbers).

#### Method 1: Symbolic Mode (Easier for Beginners)

This method uses letters to add, remove, or set permissions:

| Who | What | How |
|-----|------|-----|
| u (owner)<br>g (group)<br>o (others)<br>a (all) | r (read)<br>w (write)<br>x (execute) | + (add)<br>- (remove)<br>= (set exactly) |

**Examples:**

```bash
# Give the owner write permission
chmod u+w file.txt

# Remove read permission for others
chmod o-r file.txt

# Set exact permissions: owner reads/writes, group reads, others nothing
chmod u=rw,g=r,o= file.txt
```

#### Method 2: Numeric Mode (More Efficient)

This uses numbers where each permission has a value:

| Permission | Value |
|-----------|-------|
| Read (r) | 4 |
| Write (w) | 2 |
| Execute (x) | 1 |

You add the numbers together and create a three-digit code:

- **7** = 4+2+1 = read+write+execute (rwx)
- **6** = 4+2 = read+write (rw-)
- **5** = 4+1 = read+execute (r-x)
- **4** = read only (r--)
- **0** = no permissions (---)

**Examples:**

```bash
# 755 = owner gets 7 (rwx), group gets 5 (r-x), others get 5 (r-x)
chmod 755 myprogram

# 644 = owner gets 6 (rw-), group gets 4 (r--), others get 4 (r--)
chmod 644 myfile.txt
```

### Quick Reference: The Most Common Permissions

- **755:** Owner can do everything; others can read and execute. Great for programs.
- **644:** Owner can read/write; others can only read. Great for documents.
- **700:** Only the owner can access. Perfect for private files.

### The chown Command (Change Owner)

**chown** changes who owns a file. Usually only the administrator (root) can use this.

```bash
# Change the owner to 'alice'
sudo chown alice myfile.txt

# Change owner AND group at the same time
sudo chown alice:developers myfile.txt
```

### The chgrp Command (Change Group)

**chgrp** changes which group owns a file.

```bash
# Change the group to 'developers'
chgrp developers myfile.txt
```

### Special Permissions: setuid, setgid, and Sticky Bit

Beyond basic permissions, Linux has three special modes that control execution and deletion:

| Permission | Symbol | Numeric | What It Does |
|-----------|--------|---------|-------------|
| **setuid** | s (on owner) | 4000 | Program runs with owner's privileges (not the runner's) |
| **setgid** | s (on group) | 2000 | Program runs with group privileges; folders inherit group |
| **Sticky Bit** | t (on others) | 1000 | Only the owner can delete the file (even on shared folders) |

**Examples:**

```bash
# Add sticky bit to a shared directory
chmod +t /shared/folder

# Set setuid on a program (users run it as if they're the owner)
chmod u+s myprogram
```

**⚠️ Security Warning:** Special permissions are powerful but dangerous. Never use setuid on scripts—only on compiled programs. Setuid + write access = privilege escalation risk!

### Access Control Lists (ACLs) – Fine-Grained Control

Basic permissions (owner, group, others) are simple but sometimes too restrictive. What if you want to give one specific person access to a file without adding them to a group?

**ACLs (Access Control Lists)** let you grant permissions to individual users or groups without changing ownership.

```bash
# Give user 'ravi' read+write access to a file
setfacl -m u:ravi:rw myfile.txt

# See who has ACL permissions on a file
getfacl myfile.txt
```

**Common ACL options:**
- `-m` = add or modify an ACL entry
- `-x` = remove an ACL entry
- `-b` = remove all ACLs

### File Attributes – Extra Protection

Linux also supports **file attributes** that add extra security beyond normal permissions:

| Attribute | Command | What It Does |
|-----------|---------|-------------|
| **Immutable** | `chattr +i file.txt` | Prevents ANY changes or deletion (even by root!) |
| **Append Only** | `chattr +a file.txt` | Only allows adding data; prevents deletion/modification |

**Use cases:**
- Immutable: Protect critical config files from accidental changes
- Append Only: Perfect for log files (prevent tampering)

```bash
# View file attributes
lsattr myfile.txt
```

---

## Part 4: Mandatory Access Control (MAC) – The Higher Security Level

Discretionary Access Control (DAC) gives owners control, but a system administrator might want more enforcement. That's where **Mandatory Access Control (MAC)** comes in.

In MAC, the system administrator (not the file owner) sets strict policies that no one can override. It's like a bank's security system—even the customer can't change access rules.

### SELinux (Security-Enhanced Linux)

**SELinux** is the most popular MAC system on Linux. It labels files and processes with security contexts and enforces strict policies.

```bash
# Check if SELinux is enabled
sestatus

# Put SELinux into enforcing mode (blocks violations)
sudo setenforce 1

# Put SELinux into permissive mode (logs but allows violations)
sudo setenforce 0

# List SELinux booleans
sudo getsebool -a

# Modify a SELinux boolean
sudo setsebool httpd_enable_homedirs on

# Manage file contexts
sudo semanage fcontext -a -t httpd_sys_content_t "/web(/.*)?"
sudo restorecon -Rv /web
```

**Note:** SELinux is powerful but complex. Most beginner-friendly systems use AppArmor instead.

### AppArmor

**AppArmor** is a simpler MAC system that restricts what individual programs can do. Each program has a "profile" that defines allowed actions.

```bash
# Check AppArmor status
sudo aa-status

# Put Firefox in enforcing mode
sudo aa-enforce /etc/apparmor.d/usr.bin.firefox

# Put Firefox in complain mode (logs but allows)
sudo aa-complain /etc/apparmor.d/usr.bin.firefox

# Reload a profile
sudo apparmor_parser -r /etc/apparmor.d/usr.bin.firefox
```

**Bottom Line:** DAC = Owner decides (flexible, user-controlled). MAC = System enforces (strict, admin-controlled). Use DAC for home systems. Use MAC for secure servers.

---

## Part 5: Privilege Escalation – Doing Admin Tasks Safely

On Linux, the most powerful user is **root** (the administrator). Root can do anything—install software, change system files, even delete the entire system.

Normal users have limited permissions for safety. But sometimes you need to do admin tasks as a regular user. How?

Enter **privilege escalation**—the safe way to temporarily run commands as root.

### The sudo Command (SuperUser Do)

**sudo** lets authorized users run commands as root (or as another user) without logging in as root permanently.

```bash
# Run a command as root
sudo apt update

# Open an interactive root shell
sudo -i
```

#### Why Use sudo Instead of Logging In as Root?

**Logging in as root:** All actions run with full root power. If you make a mistake, you could destroy the system. Worse: no audit trail of who did what.

**Using sudo:** Only the specific command runs as root. Mistakes are limited. Every sudo action is logged with username, timestamp, and command—perfect for accountability.

### Understanding the sudoers File

The administrator controls who can use sudo by editing the **/etc/sudoers** file. Each line defines who can run what commands.

**Example sudoers entry:**

```
ravi ALL=(root) /usr/bin/systemctl restart apache2
```

**Breaking this down:**
- **ravi** = the username
- **ALL** = works on all computers
- **(root)** = runs as the root user
- **/usr/bin/systemctl** = the allowed command

This means: "Ravi can run the systemctl command as root to restart Apache."

**Full sudoers entry syntax:**

```
user ALL=(ALL:ALL) ALL
```

Explanation:
- **User:** The rule applies to this user
- **Hosts:** ALL — applies on all hosts
- **Run-as specification:** (ALL:ALL)
  - First ALL: may run as any user
  - Second ALL: may run as any group
- **Commands:** ALL — full command access

**⚠️ Critical Rule:** Never edit /etc/sudoers with a regular text editor. Always use `sudo visudo`, which checks for errors before saving. A broken sudoers file can lock you out!

```bash
# The RIGHT way to edit sudoers
sudo visudo

# Edit a specific sudoers.d file
sudo visudo -f /etc/sudoers.d/myfile
```

### sudo Modifiers: NOPASSWD and NOEXEC

#### NOPASSWD – Run Without a Password

By default, sudo asks for your password every time. NOPASSWD skips this step:

```
ravi ALL=(root) NOPASSWD: /usr/bin/systemctl restart apache2
```

**Use cases:**
- Automation scripts that run without user input
- Cron jobs that need root privileges

**⚠️ Security Warning:** Never use `NOPASSWD: ALL`—this gives unrestricted root access to anyone with an account. Only allow specific commands without passwords.

#### NOEXEC – Prevent Shell Escapes

Some programs (like vim, python, or less) let you run shell commands from inside them. NOEXEC prevents this:

```
ravi ALL=(root) NOEXEC: /usr/bin/vim
```

With NOEXEC, ravi can edit files in vim as root, but can't open a shell (`:!sh`) to escalate privileges.

**Why Use NOEXEC?** Some tools allow embedded shell commands, such as:
- vim
- less
- man
- python
- awk
- tar

NOEXEC adds a layer of protection against privilege escalation through these utilities.

### sudo vs. wheel Groups

Different Linux systems use different groups for sudo access:

| Linux Distribution | Group Name |
|-------------------|-----------|
| Debian, Ubuntu | **sudo** |
| Red Hat, CentOS, Fedora | **wheel** |

To add a user to the sudo/wheel group:

```bash
# On Ubuntu
sudo usermod -aG sudo username

# On Fedora
sudo usermod -aG wheel username
```

### The su Command (Substitute User)

**su** (not to be confused with sudo) switches to another user account, like logging in as them:

```bash
# Switch to root (you need root's password)
su -

# Switch to another user
su - alice
```

**Comparing sudo vs su:**

| Feature | sudo | su |
|---------|------|-----|
| **Purpose** | Run one command as another user | Switch to another user account |
| **Password** | Uses YOUR password | Uses THEIR password |
| **Session** | Single command | Full interactive shell |
| **Logging** | Detailed logs | Minimal logs |

**Bottom line:** Modern systems prefer sudo. Use su only when you need a full interactive session as another user.

### Benefits of Using sudo

- **Least Privilege:** Users gain only the required elevated capabilities
- **Auditability:** All sudo actions are logged
- **Temporary Access:** Privileges apply only to the executed command

---

## Part 6: Practical Security Checklist

Now that you understand Linux security, here's a checklist for keeping your system safe:

### ✅ File Permissions Best Practices

- Keep sensitive files at **600** (owner read+write only)
- Keep programs at **755** (executable by everyone, but only owner can modify)
- Keep documents at **644** (owner can edit, others can read)
- Make log files **immutable** (`chattr +i`) to prevent tampering
- Use sticky bit on shared directories to prevent deletion by others
- Review file permissions after system updates

### ✅ Privilege Escalation Best Practices

- Never log in as root for daily tasks—use sudo
- Use **visudo** (never a text editor) to edit sudoers
- Grant only the minimum permissions needed (least privilege principle)
- Use **NOEXEC** on dangerous programs like vim or python
- Log sudo activity and review it regularly
- Never use `NOPASSWD: ALL` unless you want a security breach
- Only allow specific commands without passwords if needed

### ✅ Access Control Best Practices

- Understand the difference between DAC (flexible) and MAC (strict)
- Use DAC for home/office systems
- Use MAC (SELinux/AppArmor) for high-security servers
- Review file permissions after system updates
- Use ACLs for fine-grained access when needed
- Protect critical config files with immutable attribute
- Use append-only attribute on log files

---

## Summary: You Now Understand Linux Security

You've learned how Linux protects files through a combination of:

- **File Permissions:** Who can read, write, and execute files
- **Ownership:** Who owns files and what that means
- **Groups:** Collecting users for easier permission management
- **Special Modes:** setuid, setgid, sticky bit for advanced control
- **Privilege Escalation:** Safely running admin commands without logging in as root
- **Access Control:** Choosing between flexible (DAC) and strict (MAC) approaches

### 🎓 The Golden Rule of Linux Security

Give each user the **minimum permission** needed to do their job. This principle—called the "principle of least privilege"—prevents accidents, limits damage from compromised accounts, and keeps systems secure.

Start by applying these concepts to your own system. Check your file permissions with `ls -l`. Use `sudo visudo` to understand who has admin access. Try setting different permission levels and observe how they affect what you can do.

With this foundation, you're well-equipped to understand—and manage—Linux security like a pro.

### 📚 Next Steps

Now that you understand files and privileges, explore advanced topics like firewalls, user accounts, encryption, and secure remote access. Each builds on these fundamentals.

---

## Quick Command Reference

### File Permissions

```bash
ls -l                          # View file permissions
chmod 755 file.txt            # Set permissions (numeric)
chmod u+x file.txt            # Add execute for owner (symbolic)
chown alice file.txt          # Change owner
chgrp developers file.txt     # Change group
chown alice:developers file.txt # Change owner and group
```

### Special Permissions

```bash
chmod u+s file.txt            # Add setuid
chmod g+s directory           # Add setgid on directory
chmod +t directory            # Add sticky bit
chmod 4755 file.txt           # Set with setuid (numeric)
```

### ACLs

```bash
setfacl -m u:username:rw file.txt  # Grant ACL permission
getfacl file.txt                    # View ACLs
setfacl -x u:username file.txt      # Remove ACL permission
```

### File Attributes

```bash
chattr +i file.txt            # Make immutable
chattr +a file.txt            # Make append-only
lsattr file.txt               # View attributes
chattr -i file.txt            # Remove immutable
```

### Privilege Escalation

```bash
sudo command                   # Run command as root
sudo -i                        # Open root shell
sudo visudo                    # Edit sudoers (safely!)
sudo visudo -f /etc/sudoers.d/file  # Edit sudoers.d file
su -                           # Switch to root
su - username                  # Switch to user
```

### MAC (SELinux)

```bash
sestatus                       # Check SELinux status
sudo setenforce 1              # Enable SELinux
sudo setenforce 0              # Set to permissive mode
sudo getsebool -a              # List SELinux booleans
sudo setsebool bool_name on    # Enable a boolean
```

### MAC (AppArmor)

```bash
sudo aa-status                 # Check AppArmor status
sudo aa-enforce profile        # Enable a profile
sudo aa-complain profile       # Set complain mode
sudo apparmor_parser -r profile # Reload a profile
```

---