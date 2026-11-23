# Lab: Managing Users and Groups

Part of my **Becoming AWS Cloud Practitioner Certified** learning journey.

## Overview

This lab covered Linux user and group administration on an Amazon Linux EC2 instance. I created multiple user accounts, assigned them to role-based groups, logged in as a standard user, and observed how Linux enforces file permissions and `sudo` privileges — including how unauthorized `sudo` attempts are logged.

**Duration:** ~45 minutes

**Tools used:** Amazon EC2, SSH (PuTTY on Windows / OpenSSH on macOS and Linux), bash, Linux user and group management commands

## Objectives

In this lab, I practiced how to:

- Create new users with a default password
- Create groups and assign the appropriate users
- Log in as different users

## Lab Environment

- **Amazon EC2 – Command Host:** The Amazon Linux instance used for all commands
- **Instance type:** `t3.micro` — 1 virtual CPU and 1 GiB of memory

> **Note:** Key files, public IP addresses, and credentials are intentionally not included in this repository. The lab's default starting password is redacted below; use a unique, strong password outside of disposable lab environments.

## Task 1: Use SSH to Connect to an Amazon Linux EC2 Instance

### Windows: Using PuTTY

1. Chose **Start Lab** and waited for **Lab status: ready**.
2. Opened **Details → Show** and downloaded `labsuser.ppk`.
3. Noted the **PublicIP** address.
4. Opened PuTTY and configured the session with the `.ppk` key and public IP.
5. Connected as `ec2-user`.

### macOS and Linux: Using OpenSSH

1. Downloaded `labsuser.pem` from the **Details** panel and noted the **PublicIP** address.
2. Changed to the download directory:

   ```bash
   cd ~/Downloads
   ```

3. Made the key read-only, as SSH requires:

   ```bash
   chmod 400 labsuser.pem
   ```

4. Connected to the instance, replacing `<public-ip>` with the lab's address:

   ```bash
   ssh -i labsuser.pem ec2-user@<public-ip>
   ```

5. Typed `yes` at the first-connection prompt. Key-pair authentication meant no password was needed.

## Task 2: Create Users

I created the following users based on the lab's role table. All accounts were given the lab's default starting password (redacted here).

| First Name | Last Name | User ID | Job Role |
|---|---|---|---|
| Alejandro | Rosalez | `arosalez` | Sales Manager |
| Efua | Owusu | `eowusu` | Shipping |
| Jane | Doe | `jdoe` | Shipping |
| Li | Juan | `ljuan` | HR Manager |
| Mary | Major | `mmajor` | Finance Manager |
| Mateo | Jackson | `mjackson` | CEO |
| Nikki | Wolf | `nwolf` | Sales Representative |
| Paulo | Santos | `psantos` | Shipping |
| Sofia | Martinez | `smartinez` | HR Specialist |
| Saanvi | Sarkar | `ssarkar` | Finance Specialist |

### Confirm the starting directory

```bash
pwd
```

Output confirmed the home directory:

```text
/home/ec2-user
```

### Create a user and set a password

For each user, I ran:

```bash
sudo useradd arosalez
sudo passwd arosalez
```

- `useradd` creates the account.
- `passwd` prompts twice for the new password.
- Nothing appears on screen while typing a password — this is normal.

### Verify the users

```bash
sudo cat /etc/passwd | cut -d: -f1
```

This displayed the usernames from `/etc/passwd`, confirming all ten accounts plus `ec2-user`:

```text
ec2-user
arosalez
eowusu
jdoe
ljuan
mjackson
mmajor
nwolf
psantos
smartinez
ssarkar
```

## Task 3: Create Groups

I created role-based groups and added each user to the appropriate groups. Some users belong to more than one group — for example, managers are also personnel, but not all personnel are managers.

> **Note:** The lab lists **Personnel** among the groups, but the creation and validation steps cover the six groups below; `Personnel` does not appear in the final `/etc/group` output.

### Create the groups

```bash
sudo groupadd Sales
sudo groupadd HR
sudo groupadd Finance
sudo groupadd Shipping
sudo groupadd Managers
sudo groupadd CEO
```

### Verify the groups

```bash
cat /etc/group
```

Each new user already had a matching private group, so the output included both those and the new groups:

```text
Sales:x:1014
HR:x:1015
Finance:x:1016
Shipping:x:1017
Managers:x:1018
CEO:x:1019
```

> Group IDs (GIDs) may differ between environments.

### Add users to groups

```bash
sudo usermod -a -G Sales arosalez
```

The `-a` (append) flag is important: without it, `usermod -G` would replace the user's existing supplementary groups.

I repeated the command for each membership:

```bash
# Sales
sudo usermod -a -G Sales arosalez
sudo usermod -a -G Sales nwolf

# HR
sudo usermod -a -G HR ljuan
sudo usermod -a -G HR smartinez

# Finance
sudo usermod -a -G Finance mmajor
sudo usermod -a -G Finance ssarkar

# Shipping
sudo usermod -a -G Shipping eowusu
sudo usermod -a -G Shipping jdoe
sudo usermod -a -G Shipping psantos

# Managers
sudo usermod -a -G Managers arosalez
sudo usermod -a -G Managers ljuan
sudo usermod -a -G Managers mmajor

# CEO
sudo usermod -a -G CEO mjackson
```

I also added `ec2-user` to every group:

```bash
sudo usermod -a -G Sales,HR,Finance,Shipping,Managers,CEO ec2-user
```

### Verify group membership

```bash
sudo cat /etc/group
```

Final membership:

```text
Sales:x:1014:arosalez,nwolf,ec2-user
HR:x:1015:ljuan,smartinez,ec2-user
Finance:x:1016:mmajor,ssarkar,ec2-user
Shipping:x:1017:eowusu,jdoe,psantos,ec2-user
Managers:x:1018:arosalez,ljuan,mmajor,ec2-user
CEO:x:1019:mjackson,ec2-user
```

## Task 4: Log In Using the New Users

I switched to the new `arosalez` account to see how permissions and `sudo` behave for a standard user.

### Switch users

```bash
su arosalez
```

After entering the password, the prompt changed to:

```text
[arosalez@ec2-user]$
```

The trailing `ec2-user` shows that the current working directory is still `/home/ec2-user`, because `su` without `-` keeps the current directory. Using `su - arosalez` would start a full login shell in that user's home directory.

### Try writing to another user's home directory

```bash
pwd
touch myFile.txt
```

Output:

```text
/home/ec2-user
touch: cannot touch 'myFile.txt': Permission denied
```

`arosalez` does not have write permission in `/home/ec2-user`.

### Try the same action with sudo

```bash
sudo touch myFile.txt
```

After entering the password, the command was rejected:

```text
arosalez is not in the sudoers file.  This incident will be reported.
```

Only users listed in the `sudoers` configuration may run commands with root privileges. This is a core example of **least privilege**.

### Return to the original user

```bash
exit
```

### Review the security log

```bash
sudo cat /var/log/secure
```

Scrolling to the bottom showed the failed privilege escalation:

```text
Aug  9 14:45:55 ip-10-0-10-217 sudo: arosalez : user NOT in sudoers ; TTY=pts/1 ; PWD=/home/ec2-user ; USER=root ; COMMAND=/bin/touch myFile.txt
```

The entry records the user, the fact that they are not in `sudoers`, the terminal, working directory, target user, and the exact command attempted.

## Commands Used

| Command | Purpose |
|---|---|
| `pwd` | Confirm the current working directory |
| `sudo useradd <user>` | Create a new user account |
| `sudo passwd <user>` | Set or change a user's password |
| `sudo cat /etc/passwd \| cut -d: -f1` | List usernames from `/etc/passwd` |
| `sudo groupadd <group>` | Create a new group |
| `cat /etc/group` | View groups and their members |
| `sudo usermod -a -G <group> <user>` | Append a user to a supplementary group |
| `su <user>` | Switch to another user while keeping the current directory |
| `touch <file>` | Create an empty file or update its timestamp |
| `sudo <command>` | Run a command with elevated privileges (if permitted) |
| `exit` | Return to the previous user or close the shell |

## Key Takeaways

- **`useradd` + `passwd`** is the basic workflow for creating Linux accounts; each user also gets a private group by default.
- **`/etc/passwd`** stores user account information, while **`/etc/group`** stores group definitions and memberships.
- **`usermod -a -G`** appends group membership. Omitting `-a` can silently remove a user from groups they should keep.
- **File permissions** prevent standard users from writing to another user's home directory.
- **`sudo` access is controlled by the sudoers file.** Unauthorized attempts are denied and recorded in `/var/log/secure`, which supports auditing and incident response.
- **Least privilege** matters: only users who genuinely need administrative rights should be granted `sudo`.

## Lab Complete

After completing the exercises, I ended the lab using **End Lab** and confirmed the environment cleanup.
