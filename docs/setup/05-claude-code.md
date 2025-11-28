# Setting Up Claude Code

Claude Code is a command-line AI assistant made by Anthropic. Unlike GUI-based AI tools, Claude Code lives in your terminal and can directly read, write, and modify files in your projects.

## What You'll Learn

- What Claude Code is and when to use it
- How to install Claude Code in WSL
- How to authenticate and configure it
- Basic usage patterns for learning and development

## What Is Claude Code?

Claude Code is an AI assistant that:

- **Lives in your terminal** - Works alongside your other command-line tools
- **Reads your code** - Can examine files to understand your project
- **Writes and edits files** - Can make changes (with your approval)
- **Runs commands** - Can execute terminal commands to test things
- **Explains concepts** - Great for learning and understanding code

### When to Use Claude Code vs. Antigravity

| Use Claude Code When... | Use Antigravity When... |
|------------------------|-------------------------|
| Working in terminal | Working in GUI editor |
| Quick questions | Long coding sessions |
| File system operations | Visual code editing |
| Shell scripting | Extension-heavy workflows |
| Command-line workflows | Multi-file visual navigation |

They complement each other - you don't have to choose one.

## System Requirements

| Component | Requirement |
|-----------|-------------|
| Operating System | Ubuntu 20.04+ (via WSL) |
| RAM | 4 GB minimum |
| Internet | Required (AI runs on Anthropic's servers) |
| Shell | Bash or Zsh (default in Ubuntu) |

## Step 1: Install Claude Code in WSL

Open your Ubuntu terminal (via WSL) and run:

### Option A: Installation Script (Recommended)

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

This downloads and runs the official installer.

### Option B: Using npm (If You Have Node.js)

If you already have Node.js 18+ installed:

```bash
npm install -g @anthropic-ai/claude-code
```

### Option C: Using Homebrew (If Installed in WSL)

If you have Homebrew set up in your WSL:

```bash
brew install --cask claude-code
```

### Verify Installation

After installation, verify it works:

```bash
claude --version
```

You should see a version number.

## Step 2: First Run and Authentication

Launch Claude Code:

```bash
claude
```

### Authentication Options

Claude Code will ask you to authenticate. You have several options:

#### Option 1: Claude Console (Recommended)

1. Create an account at [console.anthropic.com](https://console.anthropic.com)
2. Set up billing (pay-per-use)
3. When Claude Code prompts, sign in with your Console account

#### Option 2: Claude Pro/Max Subscription

If you have a Claude.ai Pro or Max subscription:
1. When prompted, choose to sign in with your Claude.ai account
2. Your subscription covers Claude Code usage

### Complete Authentication

When Claude Code opens:

1. It will display an authentication URL
2. Open the URL in your browser
3. Sign in with your chosen account
4. Authorize Claude Code
5. Return to your terminal - it should now be authenticated

### Verify Setup

Run the diagnostic command:

```bash
claude doctor
```

This checks that everything is configured correctly. Fix any issues it reports.

## Step 3: Understanding the Interface

When Claude Code starts, you'll see a prompt waiting for your input:

```
claude>
```

### Basic Interaction

Just type naturally:

```
claude> What does the ls command do?
```

Claude will respond with an explanation.

### Working with Your Project

Navigate to a project folder first:

```bash
cd /mnt/c/UnrealProjects/MyProject
claude
```

Now Claude can see your project files:

```
claude> What files are in this project?
claude> Explain what main.cpp does
claude> Find all places where "health" is mentioned
```

### Exiting Claude Code

Type `exit` or press `Ctrl + C`:

```
claude> exit
```

## Step 4: Essential Commands

### Getting Help

```
claude> /help
```

Shows all available commands.

### Asking Questions

```
claude> What is a class in C++?
claude> Explain how Unreal Engine handles player input
claude> What does this error mean: [paste error]
```

### Reading Files

Claude can read files in your current directory:

```
claude> Read the file Config/DefaultGame.ini and explain it
claude> What's in the Source folder?
```

### Making Changes

Claude asks permission before modifying files:

```
claude> Add a comment explaining what this function does
```

Claude will show you the proposed change and ask for approval.

### Running Commands

Claude can run terminal commands:

```
claude> Run git status
claude> List all .cpp files in the project
```

## Step 5: Configuration

Claude Code uses JSON configuration files for settings.

### User Settings

Your personal settings go in `~/.claude/settings.json`:

```bash
# Create the directory if it doesn't exist
mkdir -p ~/.claude

# Edit settings
nano ~/.claude/settings.json
```

Basic settings file:

```json
{
  "model": "claude-sonnet-4-5-20250929"
}
```

### Project Settings

For project-specific settings, create `.claude/settings.json` in your project folder:

```json
{
  "model": "claude-sonnet-4-5-20250929",
  "env": {
    "PROJECT_TYPE": "unreal"
  }
}
```

### Settings Priority

Settings are applied in this order (later overrides earlier):
1. User settings (`~/.claude/settings.json`)
2. Project settings (`.claude/settings.json`)
3. Local project settings (`.claude/settings.local.json`)
4. Command-line arguments

## Step 6: Practical Usage Patterns

### Pattern 1: Learning Mode

Use Claude to explain code you don't understand:

```
claude> I'm looking at this Blueprint node in Unreal. What does "Branch" do?
claude> Explain what a pointer is like I'm 10 years old
claude> Why would I use a struct instead of a class?
```

### Pattern 2: Problem Solving

When you hit a wall:

```
claude> My character won't move. I've set up input but nothing happens. Here's my code...
claude> I get this error when I compile: [error message]. What does it mean?
claude> My game crashes when I pick up an item. How do I debug this?
```

### Pattern 3: Code Review

Have Claude review your code:

```
claude> Review the function in PlayerController.cpp and suggest improvements
claude> Is there anything wrong with this code? [paste code]
claude> Are there any bugs in CharacterMovement.cpp?
```

### Pattern 4: Task Execution

One-shot commands without entering interactive mode:

```bash
# Quick question
claude "What is the actor lifecycle in Unreal Engine?"

# File operation
claude "Add a copyright header to all .h files"

# Explanation
claude "Explain the contents of this folder" --cwd /mnt/c/UnrealProjects/MyGame
```

## Step 7: Best Practices

### Be Specific

Instead of:
```
claude> Fix my code
```

Try:
```
claude> The health bar doesn't update when the player takes damage. The health variable changes but the UI stays the same. Look at HealthWidget.cpp
```

### Provide Context

```
claude> I'm building a simple platformer game in Unreal Engine 5. I want to add double-jump. The player can already single-jump using the built-in character movement component.
```

### Verify Changes

Always review what Claude proposes before accepting:

1. Read the diff Claude shows you
2. Make sure you understand what it's changing
3. Test after accepting changes

### Learn, Don't Just Accept

When Claude writes code for you:

```
claude> Explain why you wrote it this way
claude> What would happen if I changed X to Y?
claude> What alternatives did you consider?
```

## Troubleshooting

### "Command not found"

The installation didn't add Claude to your PATH. Try:

```bash
# Reload shell configuration
source ~/.bashrc

# Or restart your terminal
```

### "Authentication failed"

1. Check internet connection
2. Try `claude doctor` to diagnose
3. Remove credentials and re-authenticate:
   ```bash
   rm -rf ~/.claude/credentials
   claude
   ```

### "Claude doesn't see my files"

Make sure you're in the right directory:

```bash
pwd                    # Check current directory
cd /path/to/project    # Navigate to your project
claude                 # Start Claude Code
```

### "Responses are slow"

This depends on server load and your internet. Claude Code requires internet for every interaction.

### "Changes weren't saved"

Make sure you approved the changes when Claude asked. If you typed `n` or just pressed Enter, changes are discarded.

## Security Notes

- Claude Code can read files in your current directory
- It asks permission before making changes
- It can run commands (with your approval)
- Your code is sent to Anthropic's servers for processing
- Don't use it with sensitive/proprietary code you can't share

## What's Next

You now have Claude Code set up as a command-line AI assistant! Combined with Antigravity, you have powerful AI tools for both terminal and GUI workflows.

Next, let's dive into programming fundamentals:

**[Continue to Programming Fundamentals](../fundamentals/01-thinking-like-a-programmer.md)**

---

## Quick Reference

| Task | Command |
|------|---------|
| Start Claude Code | `claude` |
| Start with a task | `claude "your task"` |
| Get help | `/help` |
| Exit | `exit` or `Ctrl + C` |
| Check setup | `claude doctor` |
| View version | `claude --version` |

### Common Prompts

| Goal | Prompt |
|------|--------|
| Explain code | "Explain what [filename] does" |
| Find bugs | "Are there any bugs in [filename]?" |
| Ask concept | "What is [concept] in [context]?" |
| Review code | "Review [filename] and suggest improvements" |
| Debug error | "I get this error: [error]. What's wrong?" |
