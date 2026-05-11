# Day 02 - SSH, SCP, Users, Groups and File Permissions

## Overview

Today I continued my DevOps learning journey and explored some important Linux and server administration concepts.

The main focus was understanding how servers communicate securely, how users and groups are managed in Linux, and how file permissions work.

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
- File permission basics
- Permission values using binary/truth table logic
- `r`, `w`, and `x` permissions

---

## What I Practiced

### SSH Connection

I practiced connecting from Git Bash to WSL Ubuntu using SSH.

```bash
ssh -p 2222 ahsan@localhost