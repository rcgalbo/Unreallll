# Setting Up WSL (Windows Subsystem for Linux)

WSL lets you run Linux inside Windows. This gives you access to powerful development tools that were traditionally only available on Mac or Linux.

## What You'll Learn

- What WSL is and why developers use it
- How to install WSL and Ubuntu
- Basic Linux commands
- How to navigate between Windows and Linux

## What Is WSL?

WSL stands for **Windows Subsystem for Linux**. It's a compatibility layer that lets you run a real Linux operating system inside Windows.

### Why Do Developers Use Linux?

Many development tools were created on Unix/Linux systems:
- Git (version control)
- Node.js (JavaScript runtime)
- Python (programming language)
- Many command-line tools

While these work on Windows, they often work better (or are easier to set up) on Linux.

### WSL 1 vs WSL 2

- **WSL 1**: Translates Linux commands to Windows
- **WSL 2**: Runs an actual Linux kernel (recommended)

WSL 2 is faster and more compatible. We'll use WSL 2.

## Step 1: Install WSL

Microsoft has made this simple. Open PowerShell as Administrator:

1. Press `Windows Key`
2. Type "PowerShell"
3. Right-click **Windows PowerShell**
4. Click **Run as administrator**
5. Click **Yes** when prompted

Now run this command:

```powershell
wsl --install
```

This command does several things:
1. Enables WSL features
2. Sets WSL 2 as default
3. Downloads and installs Ubuntu (a popular Linux distribution)

**You will need to restart your computer after this.**

### After Restart

After restarting, Ubuntu should automatically open and continue setup. If it doesn't:

1. Press `Windows Key`
2. Type "Ubuntu"
3. Click on **Ubuntu**

### Create Your Linux User

Ubuntu will ask you to create a user account:

```
Enter new UNIX username: yourname
New password: ********
Retype new password: ********
```

**Important notes about the password:**
- The cursor won't move when you type (this is normal in Linux)
- Choose something you'll remember
- This password is for your Linux user, separate from Windows

### Installation Complete!

You should see something like:

```
Welcome to Ubuntu 22.04 LTS (GNU/Linux...)
yourname@COMPUTER:~$
```

This is the Linux command prompt. The `$` means it's waiting for your input.

## Step 2: Update Ubuntu

First thing to do on any new Linux system: update everything.

```bash
sudo apt update && sudo apt upgrade -y
```

Let's break this down:
- `sudo` = "superuser do" (run as administrator)
- `apt` = Ubuntu's package manager (installs software)
- `update` = refresh the list of available software
- `&&` = "and then" (run next command if first succeeds)
- `upgrade` = install available updates
- `-y` = answer "yes" to prompts automatically

This might take a few minutes. Let it finish.

## Step 3: Essential Linux Commands

You'll use these constantly. Practice each one:

### Navigating the File System

```bash
# Print working directory (where am I?)
pwd

# List files and folders
ls

# List with details (permissions, size, date)
ls -la

# Change directory
cd foldername

# Go up one directory
cd ..

# Go to home directory
cd ~

# Go to a specific path
cd /home/yourname
```

### Working with Files

```bash
# Create an empty file
touch myfile.txt

# Create a directory
mkdir myfolder

# Copy a file
cp myfile.txt mycopy.txt

# Move (or rename) a file
mv myfile.txt newname.txt

# Delete a file (careful - no recycle bin!)
rm myfile.txt

# Delete a directory and its contents
rm -rf myfolder
```

### Viewing File Contents

```bash
# View entire file
cat myfile.txt

# View file with scrolling
less myfile.txt
# (press q to quit)

# View first 10 lines
head myfile.txt

# View last 10 lines
tail myfile.txt
```

### Getting Help

```bash
# Manual page for a command
man ls

# Brief help
ls --help
```

## Step 4: Understanding the File System

Linux and Windows have different file systems. Here's how they connect:

### Your Linux Home Directory

When you open Ubuntu, you start here:
```
/home/yourname/
```

This is YOUR Linux space. Files here are stored inside WSL.

### Accessing Windows Files from Linux

Your Windows drives are available at `/mnt/`:

```bash
# Your C: drive
cd /mnt/c/

# Your user folder
cd /mnt/c/Users/YourWindowsUsername/

# Your Documents folder
cd /mnt/c/Users/YourWindowsUsername/Documents/
```

### Accessing Linux Files from Windows

In Windows Explorer, type in the address bar:
```
\\wsl$\Ubuntu\home\yourname
```

Or click the Linux penguin icon in the sidebar of File Explorer.

### File System Best Practice

**Store your projects on Windows** (`/mnt/c/...`) rather than in the Linux file system. This way:
- Windows programs (like Unreal) can access them
- They persist if you reinstall WSL
- You can see them in File Explorer

## Step 5: Install Development Tools

Let's install some essential tools:

### Git (Version Control)

Git might already be installed, but let's make sure:

```bash
sudo apt install git -y
```

Configure it:
```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### Build Essentials

These are basic compilation tools:

```bash
sudo apt install build-essential -y
```

### Node.js (JavaScript Runtime)

Many development tools use Node.js:

```bash
# Install Node Version Manager
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

# Reload your shell configuration
source ~/.bashrc

# Install latest LTS version of Node
nvm install --lts

# Verify installation
node --version
npm --version
```

### Python

Python is often pre-installed, but let's ensure we have a good setup:

```bash
# Install Python and pip
sudo apt install python3 python3-pip -y

# Verify
python3 --version
pip3 --version
```

## Step 6: Customize Your Terminal (Optional)

The default terminal is functional but plain. Here are some improvements:

### Install Oh My Bash

This makes your terminal more informative and colorful:

```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/ohmybash/oh-my-bash/master/tools/install.sh)"
```

### Create Useful Shortcuts (Aliases)

Edit your bash configuration:

```bash
nano ~/.bashrc
```

Add these lines at the end:

```bash
# Custom aliases
alias ll='ls -la'
alias ..='cd ..'
alias ...='cd ../..'
alias projects='cd /mnt/c/UnrealProjects'
```

Save (`Ctrl+X`, then `Y`, then `Enter`) and reload:

```bash
source ~/.bashrc
```

Now typing `ll` is the same as `ls -la`.

## Step 7: Set WSL 2 as Default (If Not Already)

Verify you're using WSL 2:

```powershell
# Run this in PowerShell (not Ubuntu)
wsl --list --verbose
```

You should see:
```
  NAME      STATE           VERSION
* Ubuntu    Running         2
```

If VERSION shows 1, convert to 2:
```powershell
wsl --set-version Ubuntu 2
```

## Understanding WSL Integration

### How WSL Runs

WSL 2 runs a lightweight virtual machine. When you:
- Open Ubuntu: The VM starts
- Close all Ubuntu windows: The VM keeps running briefly
- Run `wsl --shutdown`: The VM fully stops

### Starting WSL

Multiple ways to open Linux:
1. Search for "Ubuntu" in Start menu
2. Open Windows Terminal and select Ubuntu
3. Type `wsl` in PowerShell or Command Prompt
4. Type `bash` in PowerShell or Command Prompt

### Running Linux Commands from Windows

You can run Linux commands directly from PowerShell:

```powershell
wsl ls -la
wsl pwd
```

### Running Windows Programs from Linux

You can run Windows programs from Ubuntu:

```bash
# Open current folder in Windows Explorer
explorer.exe .

# Open a file with default Windows program
notepad.exe myfile.txt

# Run VS Code (once installed)
code .
```

Note the `.exe` extension is required.

## Troubleshooting

### "WSL installation failed"

1. Make sure you enabled Windows features (see Prerequisites guide)
2. Make sure Virtualization is enabled in BIOS
3. Run Windows Update and restart

### "Ubuntu won't start"

```powershell
# In PowerShell as Admin, try:
wsl --update
wsl --shutdown
# Then try opening Ubuntu again
```

### "Permission denied"

- Did you forget `sudo`?
- Are you trying to modify Windows system files from Linux? Don't.

### "Command not found"

- Typo? Commands are case-sensitive
- Need to install the program first?
- Forgot to reload shell after changes? Run `source ~/.bashrc`

### "Very slow performance"

- Store files on Windows (`/mnt/c/`) rather than Linux file system
- Don't run heavy tasks while Unreal Engine is open
- Close unnecessary applications

### "Can't access Windows files"

Make sure you're using the right path:
- Correct: `/mnt/c/Users/Name/Documents`
- Wrong: `C:\Users\Name\Documents` (Windows syntax doesn't work in Linux)

## What's Next

You now have a Linux environment inside Windows! This gives you access to powerful command-line tools that we'll use throughout the course.

Next, we'll install Visual Studio Code, which integrates beautifully with WSL:

**[Continue to VSCode Setup](04-vscode.md)**

---

## Quick Reference

| Task | Command |
|------|---------|
| Update Ubuntu | `sudo apt update && sudo apt upgrade -y` |
| Navigate to folder | `cd foldername` |
| List files | `ls -la` |
| Current location | `pwd` |
| Go to Windows folder | `cd /mnt/c/Users/YourName/` |
| Open folder in Explorer | `explorer.exe .` |
| Install program | `sudo apt install programname -y` |
| Stop WSL | `wsl --shutdown` (in PowerShell) |
