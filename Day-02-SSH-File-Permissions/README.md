# Day 02 - SSH, SCP, Users, Groups and File Permissions

## 90 Days of DevOps Journey

This repository is part of my **90 Days of DevOps** learning journey.  
On Day 02, I focused on core Linux and DevOps fundamentals related to **SSH**, **remote server access**, **SCP**, **users and groups**, and **file permissions**.

These concepts are important because DevOps engineers regularly work with remote servers, Linux environments, permissions, secure access, and file transfers.

---

## Topics Covered

- SSH basics
- Client and server concept
- Remote server access
- Bastion host / jump server concept
- SSH key generation
- Public key and private key concept
- SCP for secure file transfer
- Linux users and groups
- Group management using `gpasswd`
- File permissions
- Permission values using binary/truth table logic
- Read, write, and execute permissions

---

## 1. SSH - Secure Shell

**SSH** stands for **Secure Shell**.

SSH is used to securely access and control a remote server from another machine.

Example real-world situation:

> I am sitting in Islamabad, and my server is running in Ireland.  
> To access and manage that server remotely, I can use SSH.

Basic SSH syntax:

```bash
ssh username@server_ip
```

With a custom port:

```bash
ssh -p 2222 username@server_ip
```

In my practice, I connected from **Git Bash** to **WSL Ubuntu** using:

```bash
ssh -p 2222 ahsan@localhost
```

---

## 2. Client and Server Concept

In SSH:

| Component | Meaning |
|---|---|
| Client | The machine from where we connect |
| Server | The machine we want to access |

In my practice setup:

| Role | Machine |
|---|---|
| Client | Git Bash |
| Server | WSL Ubuntu |

Flow:

```text
Git Bash
   |
   | SSH on port 2222
   v
WSL Ubuntu
```

This helped me understand how remote server access works in real DevOps environments.

---

## 3. SSH Service Setup

To make WSL Ubuntu accessible through SSH, I installed and started the OpenSSH server.

Commands used:

```bash
sudo apt update
sudo apt install openssh-server -y
sudo service ssh start
sudo service ssh status
```

The SSH service was configured to run on port `2222`.

To connect from Git Bash:

```bash
ssh -p 2222 ahsan@localhost
```

To stop the SSH service:

```bash
sudo service ssh stop
```

---

## 4. SSH Keys

SSH keys are used for secure authentication.

An SSH key pair contains:

| Key Type | Purpose |
|---|---|
| Private Key | Stays with the client and must never be shared |
| Public Key | Stored on the server to allow access |

SSH key generation command:

```bash
ssh-keygen
```

This created:

```text
id_ed25519
id_ed25519.pub
```

Meaning:

| File | Description |
|---|---|
| `id_ed25519` | Private key |
| `id_ed25519.pub` | Public key |

Important rule:

> Private key stays on the client.  
> Public key is stored on the server.

If the private key and public key match, access is allowed.

---

## 5. Bastion Host / Jump Server

A **Bastion Host** is a secure server used as an entry point to access private servers.

Flow:

```text
Laptop
   |
   | SSH
   v
Bastion Host
   |
   | SSH
   v
Private Server
```

Why it is used:

- Private servers are not directly exposed to the internet
- Access becomes more secure
- Server access can be controlled from one entry point
- It reduces the attack surface

Simple understanding:

> Bastion Host works like a main gate.  
> First we access the gate, then we access internal servers.

---

## 6. SCP - Secure Copy

**SCP** stands for **Secure Copy**.

SCP is used to securely copy files between machines using SSH.

Basic syntax:

```bash
scp source destination
```

In my practice, I copied a file from **Git Bash** to **WSL Ubuntu**:

```bash
scp -P 2222 text.txt ahsan@localhost:/home/ahsan/
```

Important difference:

| Command | Port Option |
|---|---|
| `ssh` | `-p` lowercase |
| `scp` | `-P` uppercase |

After copying the file, I verified it in WSL Ubuntu:

```bash
ls
cat text.txt
```

Output:

```text
Hello World From Git Bash
```

This confirmed that the file was successfully transferred.

---

## 7. Users and Groups

Linux uses users and groups for access control.

### Create a User

```bash
sudo adduser ahsan
```

This creates a new user named `ahsan`.

In Ubuntu/Linux, when a new user is created, a group with the same name may also be created automatically.

Example:

```text
User: ahsan
Group: ahsan
```

This is called a **User Private Group**.

### Create a Group

```bash
sudo addgroup devops
```

This creates a new group named `devops`.

---

## 8. Why Groups Are Created

Groups are created to manage permissions for multiple users at once.

Instead of giving permissions to every user separately, we can add users to a group and give permissions to that group.

Example:

```text
Group: devops
Users: ahsan, jaffer
```

If the `devops` group has access to a project folder, all users in that group can access it according to the assigned permissions.

One-line understanding:

> A user is an individual identity.  
> A group is a shared permission container.

---

## 9. Managing Group Members with gpasswd

The `gpasswd` command is used to manage group members.

### Add a User to a Group

```bash
sudo gpasswd -a ahsan devops
```

Meaning:

> Add user `ahsan` to the `devops` group.

### Set Full Group Member List

```bash
sudo gpasswd -M ahsan,jaffer devops
```

Meaning:

> Set `ahsan` and `jaffer` as members of the `devops` group.

Important warning:

`-M` replaces the complete group member list.  
For safer usage, `-a` is better when adding users one by one.

### Check Groups of a User

```bash
groups ahsan
```

Or:

```bash
id ahsan
```

---

## 10. File Permissions

Linux file permissions decide who can read, write, or execute a file or directory.

There are three permission types:

| Symbol | Meaning |
|---|---|
| `r` | Read |
| `w` | Write |
| `x` | Execute |

There are three permission categories:

| Category | Meaning |
|---|---|
| User/Owner | File owner |
| Group | Group assigned to the file |
| Others | Everyone else |

Example output:

```bash
-rw-r--r-- 1 ahsan ahsan 26 May 11 16:30 text.txt
```

Permission part:

```text
-rw-r--r--
```

Breakdown:

```text
-    rw-    r--    r--
|     |      |      |
|     |      |      └── Others
|     |      └───────── Group
|     └──────────────── Owner
└────────────────────── File type
```

---

## 11. File vs Directory Permissions

### For Files

| Permission | Meaning |
|---|---|
| `r` | Read file content |
| `w` | Modify file content |
| `x` | Execute/run the file |

### For Directories

| Permission | Meaning |
|---|---|
| `r` | List directory contents |
| `w` | Create/delete files inside directory |
| `x` | Enter/access the directory |

Important:

> For directories, execute permission `x` is required to enter the directory using `cd`.

---

## 12. Numeric Permission System

Linux permissions can be represented using numbers.

| Permission | Value |
|---|---:|
| Read | 4 |
| Write | 2 |
| Execute | 1 |

Values are added together.

| Number | Permission | Meaning |
|---:|---|---|
| 0 | `---` | No permission |
| 1 | `--x` | Execute only |
| 2 | `-w-` | Write only |
| 3 | `-wx` | Write + Execute |
| 4 | `r--` | Read only |
| 5 | `r-x` | Read + Execute |
| 6 | `rw-` | Read + Write |
| 7 | `rwx` | Read + Write + Execute |

---

## 13. Truth Table Logic in File Permissions

File permissions work like binary logic:

```text
1 = allowed
0 = not allowed
```

For `rwx`:

```text
r w x
4 2 1
```

Examples:

```text
rwx = 4 + 2 + 1 = 7
rw- = 4 + 2 + 0 = 6
r-x = 4 + 0 + 1 = 5
r-- = 4 + 0 + 0 = 4
--- = 0 + 0 + 0 = 0
```

This is how commands like `chmod 755` and `chmod 644` work.

---

## 14. Common chmod Examples

### chmod 755

```bash
chmod 755 script.sh
```

Meaning:

| Category | Value | Permission |
|---|---:|---|
| Owner | 7 | `rwx` |
| Group | 5 | `r-x` |
| Others | 5 | `r-x` |

Used for executable scripts.

---

### chmod 644

```bash
chmod 644 file.txt
```

Meaning:

| Category | Value | Permission |
|---|---:|---|
| Owner | 6 | `rw-` |
| Group | 4 | `r--` |
| Others | 4 | `r--` |

Used for normal text/config files.

---

### chmod 700

```bash
chmod 700 ~/.ssh
```

Meaning:

| Category | Value | Permission |
|---|---:|---|
| Owner | 7 | `rwx` |
| Group | 0 | `---` |
| Others | 0 | `---` |

Used for private directories like `.ssh`.

---

### chmod 600

```bash
chmod 600 ~/.ssh/id_ed25519
```

Meaning:

| Category | Value | Permission |
|---|---:|---|
| Owner | 6 | `rw-` |
| Group | 0 | `---` |
| Others | 0 | `---` |

Used for private SSH keys.

---

## 15. Important Commands Practiced

### SSH Commands

```bash
ssh -p 2222 ahsan@localhost
```

```bash
sudo apt install openssh-server -y
```

```bash
sudo service ssh start
```

```bash
sudo service ssh status
```

```bash
sudo service ssh stop
```

---

### SSH Key Commands

```bash
ssh-keygen
```

```bash
ls -a
```

```bash
cd ~/.ssh
```

```bash
ls
```

```bash
cat id_ed25519.pub
```

---

### SCP Commands

```bash
scp -P 2222 text.txt ahsan@localhost:/home/ahsan/
```

```bash
cat text.txt
```

---

### User and Group Commands

```bash
sudo adduser ahsan
```

```bash
sudo addgroup devops
```

```bash
sudo gpasswd -a ahsan devops
```

```bash
sudo gpasswd -M ahsan,jaffer devops
```

```bash
groups ahsan
```

```bash
id ahsan
```

---

### File Permission Commands

```bash
ls -l
```

```bash
ls -la
```

```bash
chmod 755 script.sh
```

```bash
chmod 644 file.txt
```

```bash
chmod 700 ~/.ssh
```

```bash
chmod 600 ~/.ssh/id_ed25519
```

---

## Key Learnings

Today I learned that Linux is not only about memorizing commands.

It is about understanding how:

- Machines connect securely
- Servers are accessed remotely
- Files are transferred securely
- Users are created and managed
- Groups simplify access control
- Permissions protect files and directories
- SSH and SCP are used in real DevOps environments

---

## Real DevOps Connection

These concepts are used in real DevOps work for:

- Accessing cloud servers
- Managing Linux machines
- Deploying applications
- Transferring configuration files
- Downloading logs from servers
- Managing user access
- Securing SSH keys
- Handling production server permissions

---

## Final Takeaway

Day 02 helped me understand how Linux, SSH, SCP, users, groups, and permissions connect together.

This was an important step in building strong DevOps fundamentals.

> Small daily progress builds real DevOps skills.

---

## Hashtags

#DevOps #90DaysOfDevOps #Linux #SSH #SCP #GitBash #WSL #Ubuntu #FilePermissions #LearningInPublic
