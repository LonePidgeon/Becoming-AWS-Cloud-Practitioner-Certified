# Lab: Editing Files

Part of my **Becoming AWS Cloud Practitioner Certified** learning journey.

## Overview

This lab introduced command-line text editing on Linux using **Vim** and **nano**. I worked through the built-in `vimtutor` lessons, created and edited files in Vim using normal and insert modes, and used nano to create and save a file without modal editing.

**Duration:** ~1 hour

**Tools used:** Amazon EC2, SSH (PuTTY on Windows / OpenSSH on macOS and Linux), bash, Vim, `vimtutor`, nano

## Objectives

After completing this lab, I was able to:

- Use the `vimtutor` executable to conduct the tutorial exercises
- Copy content from the `/var/log/secure` file and edit it with nano

> **Note:** The detailed steps in this lab version focus on creating and editing files with Vim and nano. The `/var/log/secure` objective is part of the broader lab goals.

## Lab Environment

- **Amazon EC2 – Command Host:** The Amazon Linux instance used for all commands
- **Instance type:** `t3.micro` — 1 virtual CPU and 1 GiB of memory

> **Note:** Key files, public IP addresses, and other credentials are intentionally not included in this repository.

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

## Task 2: Exercise – Run the Vim Tutorial

`vimtutor` is an interactive application that teaches the basics of Vim, one of the standard Linux text editors.

```bash
vimtutor
```

If Vim is not installed:

```bash
sudo yum install vim
```

I completed the tutorial lessons, which covered:

- Moving the cursor with `h`, `j`, `k`, and `l`
- Entering and leaving insert mode
- Basic editing, deleting, and saving

> The lab text references lessons 1–3 in the step-by-step instructions and tasks 1–4 in the introduction; I completed the tutorial lessons as directed in the terminal.

To exit `vimtutor` without saving:

```text
:q!
```

## Task 3: Exercise – Edit a File in Vim

Vim is a **modal** editor: keystrokes mean different things depending on the current mode.

### Create and open a file

```bash
vim helloworld
```

### Insert text

Pressed `i` to enter **insert mode**, then typed:

```text
Hello World!
This is my first file in Linux and I am editing it in Vim!
```

The bottom-left of the terminal indicates when insert mode is active. Pressed **Esc** to return to normal mode.

### Save and quit

```text
:wq
```

- `:` enters command-line mode
- `w` writes (saves) the file
- `q` quits

### Reopen the file

```bash
vim helloworld
```

Because this was the most recent command, the **up arrow** could also recall it.

### Add another line

Pressed `i`, added:

```text
I learned how to create a file, edit and save them too!
```

Pressed **Esc**, then quit **without saving**:

```text
:q!
```

### Analyze the difference

Reopening `helloworld` showed only the first two lines. The third line was **not** there because `:q!` discards changes, while `:wq` writes them to disk.

### Additional Vim commands

| Command | Action |
|---|---|
| `dd` | Delete the entire current line |
| `u` | Undo the last change |
| `:w` | Save changes without quitting |
| `:wq` | Save changes and quit |
| `:q!` | Quit without saving |
| `i` | Enter insert mode before the cursor |
| `Esc` | Return to normal mode |

## Task 4: Exercise – Edit a File in nano

nano is a simpler, non-modal editor: you can start typing immediately.

### Create and open a file

```bash
nano cloudworld
```

### Enter text

```text
We are using nano this time! We can simply start typing! No insert mode needed.
```

### Save the file

Pressed **Ctrl+O**, then **Enter** to confirm the filename.

### Exit nano

Pressed **Ctrl+X**.

### Verify the file

```bash
nano cloudworld
```

The text was present as expected. Exited again with **Ctrl+X**.

### nano shortcuts used

| Shortcut | Action |
|---|---|
| `Ctrl+O` | Write the file (save) |
| `Enter` | Confirm the filename when saving |
| `Ctrl+X` | Exit nano |

## Commands Used

| Command | Purpose |
|---|---|
| `vimtutor` | Launch the interactive Vim tutorial |
| `sudo yum install vim` | Install Vim if it is not already available |
| `vim helloworld` | Create or open a file in Vim |
| `i` | Enter insert mode |
| `Esc` | Return to normal mode |
| `:wq` | Save and quit Vim |
| `:q!` | Quit Vim without saving |
| `dd` | Delete a line in Vim |
| `u` | Undo the last change in Vim |
| `:w` | Save without quitting Vim |
| `nano cloudworld` | Create or open a file in nano |
| `Ctrl+O`, `Enter` | Save the file in nano |
| `Ctrl+X` | Exit nano |

## Key Takeaways

- **Vim is modal.** Editing requires entering insert mode (`i`), and saving/quitting happens from normal mode using `:` commands.
- **`:wq` saves and quits; `:q!` discards changes.** This distinction explains why the third line disappeared when the file was reopened.
- **`dd`, `u`, and `:w`** are essential Vim workflow commands for deleting, undoing, and saving incrementally.
- **nano is non-modal and beginner-friendly.** `Ctrl+O` saves and `Ctrl+X` exits.
- Knowing both editors is useful: nano is quick for small edits, while Vim is powerful and available on nearly every Linux system.

## Lab Complete

After completing the exercises, I ended the lab using **End Lab** and confirmed the environment cleanup.
