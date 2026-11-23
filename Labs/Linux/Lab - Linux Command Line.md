# Lab: Linux Command Line

Part of my **Becoming AWS Cloud Practitioner Certified** learning journey.

## Overview

This lab focused on everyday Linux command line skills: gathering information about the current system and session, and speeding up work by reusing commands from bash history. I connected to an Amazon Linux EC2 instance over SSH and practiced command discovery, calendar and time utilities, user identity checks, and history search.

**Duration:** ~30 minutes

**Tools used:** Amazon EC2, SSH (PuTTY on Windows / OpenSSH on macOS and Linux), bash

## Objectives

In this lab, I practiced how to:

- Run commands to gain knowledge of my current system and current session
- Search and run previous bash commands

## Lab Environment

- **Amazon EC2 – Command Host:** The Amazon Linux instance used for all commands
- **Instance type:** `t3.micro` — 1 virtual CPU and 1 GiB of memory

> **Note:** Key files, public IP addresses, and other credentials are intentionally not included in this repository. Command output varies by system, time, and timezone.

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

## Task 2: Run Familiar Commands

These commands provide a quick snapshot of the system and the current session.

### Command autocomplete

Typed `who` and pressed **Tab**. Bash autocomplete expanded it to the full command:

```bash
whoami
```

Pressed **Enter** to display the current username (`ec2-user`).

### Short hostname

```bash
hostname -s
```

Displayed a shortened version of the computer's hostname, such as `ip-10-x-x-x`.

### System uptime

```bash
uptime -p
```

Displayed how long the system had been running in an easily readable format.

### Logged-in users

```bash
who -H -a
```

Displayed information about users logged into the system, including:

- `NAME` — the user
- `LINE` — the terminal line
- `TIME` — when the login occurred
- `IDLE` — idle time
- `PID` — process identifier
- `COMMENT` — additional information
- `EXIT` — exit time

### Time in other timezones

```bash
TZ=America/New_York date
TZ=America/Los_Angeles date
```

These commands displayed the current date and time for New York and Los Angeles by temporarily setting the `TZ` environment variable for the `date` command.

> **Note:** If the system clock is not set correctly, these times will be incorrect.

### Julian calendar

```bash
cal -j
```

Displayed the current month with Julian dates. Julian dates count consecutively through the year instead of restarting at 1 each month — for example, the day after January 31 is day 32, not February 1.

### Alternate calendar layouts

```bash
cal -s
cal -m
```

- `cal -s` starts the week on **Sunday**
- `cal -m` starts the week on **Monday**

The `cal` man page documents many more display options.

### User and group identity

```bash
id ec2-user
```

Displayed the user ID (UID), primary group ID (GID), and all groups the `ec2-user` belongs to.

## Task 3: Improve Workflow Through History and Search

### View command history

```bash
history
```

Displayed the list of previously entered commands, including the commands used in Task 2.

### Reverse history search

1. Pressed **Ctrl+R** to open reverse history search.
2. Typed `TZ` and pressed **Tab** to autocomplete to the previous `TZ=... date` command.
3. Used the arrow keys to edit the command inline before running it.

> **Tip:** The Tab autocomplete step is required to pull the matched command into the prompt for editing.

### Rerun the most recent command

```bash
date
!!
```

`!!` reruns the last command entered. In this case, `date` ran once, and `!!` ran it again immediately.

## Commands Used

| Command | Purpose |
|---|---|
| `who` + **Tab** | Use autocomplete to expand to `whoami` |
| `whoami` | Show the current username |
| `hostname -s` | Show the short hostname |
| `uptime -p` | Show system uptime in a readable format |
| `who -H -a` | Show logged-in users with headers and extra details |
| `TZ=America/New_York date` | Show the current time in New York |
| `TZ=America/Los_Angeles date` | Show the current time in Los Angeles |
| `cal -j` | Show the current month with Julian dates |
| `cal -s` / `cal -m` | Show the calendar starting on Sunday / Monday |
| `id ec2-user` | Show UID, GID, and group membership |
| `history` | List previously run commands |
| **Ctrl+R**, `TZ`, **Tab** | Reverse-search history and edit a matched command |
| `!!` | Rerun the most recent command |

## Key Takeaways

- **Tab autocomplete** saves typing and helps avoid command typos.
- Commands such as `whoami`, `hostname`, `uptime`, `who`, and `id` provide fast system and session context — useful for troubleshooting.
- The `TZ` environment variable makes it easy to view the time in other regions without changing the system clock.
- `cal` supports useful variations, including Julian dates and different week-start days.
- **Ctrl+R** reverse search and `!!` make it easy to find and reuse previous commands, which is a major productivity boost in the shell.

## Lab Complete

After completing the exercises, I ended the lab using **End Lab** and confirmed the environment cleanup.
