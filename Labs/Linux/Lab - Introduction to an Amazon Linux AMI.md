# Lab: Introduction to an Amazon Linux AMI

Part of my **Becoming AWS Cloud Practitioner Certified** learning journey.

## Overview

This lab introduced the Linux command line by connecting to an Amazon Linux EC2 instance over Secure Shell (SSH) and exploring the Linux manual pages (`man`). The goal was to build a solid foundation in basic CLI functionality and learn how to find help for commands directly from the shell.

**Duration:** ~30 minutes

**Tools used:** Amazon EC2, SSH (PuTTY on Windows / OpenSSH on macOS and Linux), bash, Linux `man` pages

## Objectives

After completing this lab, I was able to:

- Use SSH to access an Amazon Linux AMI within Vocareum labs
- Understand the purpose of the `man` command
- Demonstrate the search feature of the man pages
- Examine man page headers

## Lab Environment

The following components were created for me as part of the lab environment:

- **Amazon EC2 – Command Host (public subnet):** The instance I logged in to in order to run the lab commands
- **Public subnet**
- **Amazon Virtual Private Cloud (Amazon VPC)**

The lab used a **t3.micro** instance type, which has 1 virtual CPU and 1 GiB of memory.

> **Note:** Key files, public IP addresses, and other credentials are intentionally not included in this repository.

## Task 1: Use SSH to Connect to an Amazon Linux EC2 Instance

In this task, I connected to the Amazon Linux EC2 instance using an SSH utility.

### Windows: Using PuTTY

1. Chose **Start Lab** at the top of the instructions and waited for **Lab status: ready**.
2. Opened the **Details** drop-down, chose **Show**, and downloaded the `labsuser.ppk` key file.
3. Noted the **PublicIP** address for the instance.
4. Opened PuTTY and configured the session with the downloaded `.ppk` key and the public IP.
5. Connected to the instance as `ec2-user`.

### macOS and Linux: Using OpenSSH

1. Downloaded the `labsuser.pem` key file from the **Details** panel and noted the **PublicIP** address.
2. Changed to the directory containing the key:

   ```bash
   cd ~/Downloads
   ```

3. Made the key read-only, which SSH requires for private keys:

   ```bash
   chmod 400 labsuser.pem
   ```

4. Connected to the instance, replacing `<public-ip>` with the address from the lab details:

   ```bash
   ssh -i labsuser.pem ec2-user@<public-ip>
   ```

5. Typed `yes` when prompted to allow the first connection to the remote SSH server. Because the connection used key-pair authentication, no password was required.

## Task 2: Exercise – Explore the Linux Man Pages

The Linux standard help system is generally referred to as the **manual pages** (or **man pages**). I used a bash terminal to view it.

### Open the man page for `man`

```bash
man man
```

The terminal displayed the manual page for the `man` command itself. I used the up and down arrow keys to move through the page.

### Identify the major sections

Important man page headers include:

- `NAME`
- `SYNOPSIS`
- `DESCRIPTION`
- `OVERVIEW`
- `EXAMPLES`
- `FILES`
- `OPTIONS`
- `SEE ALSO`

The `DESCRIPTION` header in `man man` also explains the numbered manual sections:

| Section | Contents |
|---|---|
| 1 | Executable programs or shell commands |
| 2 | System calls |
| 3 | Library calls |
| 4 | Special files (usually devices in `/dev`) |
| 5 | File formats and conventions |
| 6 | Games |
| 7 | Miscellaneous |
| 8 | System administration commands |
| 9 | Kernel routines |

### Search within a man page

Man pages can be searched interactively:

- Press `/`, type a search term, and press **Enter** to search forward.
- Press `n` to jump to the next match and `N` for the previous match.

### Exit the man pages

```bash
q
```

## Commands Used

| Command | Purpose |
|---|---|
| `cd ~/Downloads` | Change to the directory containing the downloaded SSH key |
| `chmod 400 labsuser.pem` | Make the private key read-only so SSH will accept it |
| `ssh -i labsuser.pem ec2-user@<public-ip>` | Connect to the Amazon Linux instance using key-pair authentication |
| `man man` | Open the manual page for the `man` command |
| `/term`, `n`, `N` | Search within a man page and move between matches |
| `q` | Quit the man page viewer |

## Key Takeaways

- **SSH key authentication** is the standard way to access Linux EC2 instances; private keys must have restrictive permissions (`chmod 400`).
- The **`man` command** provides offline documentation for nearly every Linux command and utility.
- Man pages follow a consistent structure (`NAME`, `SYNOPSIS`, `DESCRIPTION`, `OPTIONS`, `EXAMPLES`, and more), which makes it easy to find the information you need.
- The **`man man`** page is a great starting point because it explains how the manual itself is organized.
- Interactive search (`/`, `n`, `N`) makes long man pages much faster to navigate.

## Lab Complete

After finishing the exercises, I ended the lab using **End Lab** and confirmed the environment cleanup.
