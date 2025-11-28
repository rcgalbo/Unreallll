# Setting Up Google Antigravity IDE

Google Antigravity is an AI-powered development environment that uses autonomous agents to help you build software. Instead of just autocompleting code, it can plan, write, and debug entire features.

## What You'll Learn

- What Antigravity is and how it differs from traditional editors
- How to download and install Antigravity on Windows
- How to configure it for your first use
- How to connect it with WSL for Linux development

## What Is Antigravity?

Antigravity is Google's "agent-first" IDE. Traditional code editors help you write code faster. Antigravity goes further - it can:

- **Plan features** - Describe what you want, and it creates a plan
- **Write code** - Autonomous agents write code based on your instructions
- **Debug problems** - It can investigate and fix issues
- **Browse documentation** - Agents can look up information they need

Think of it as having a junior developer assistant who can handle routine tasks while you focus on the big picture.

### Why Use an AI-Powered Editor?

For learning, Antigravity offers unique benefits:

1. **Ask questions** - Don't understand code? Ask the AI to explain it
2. **Get unstuck** - When you hit a wall, describe your problem and get suggestions
3. **Learn patterns** - See how the AI approaches problems
4. **Move faster** - Spend less time on boilerplate, more time on interesting problems

However, **don't let the AI do all the thinking**. Use it as a learning tool, not a replacement for understanding.

## System Requirements

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| Operating System | Windows 10 64-bit | Windows 11 64-bit |
| RAM | 8 GB | 16 GB |
| Disk Space | 2 GB free | 5 GB free |
| Processor | 64-bit Intel or AMD | Modern multi-core |
| Internet | Required | Fast connection recommended |

Antigravity requires an internet connection because the AI models run on Google's servers.

## Step 1: Download Antigravity

1. Go to [antigravity.google](https://antigravity.google/)
2. Click **Download** or go directly to [antigravity.google/download](https://antigravity.google/download)
3. Select **Windows**
4. The installer will download (approximately 200-300 MB)

## Step 2: Install Antigravity

1. Find the downloaded installer (usually in your Downloads folder)
2. Double-click `AntigravitySetup.exe`
3. If Windows asks "Do you want to allow this app to make changes?", click **Yes**
4. Follow the installation wizard:
   - Accept the license agreement
   - Choose installation location (default is fine)
   - Optionally create a desktop shortcut
5. Click **Install**
6. Wait for installation to complete (5-10 minutes)
7. Click **Finish**

### Installation Location

Default: `C:\Users\YourName\AppData\Local\Programs\Antigravity\`

You can change this, but the default location is recommended.

## Step 3: First Launch and Sign In

1. Launch Antigravity:
   - Double-click the desktop shortcut, OR
   - Press `Windows Key`, type "Antigravity", press Enter

2. **Sign in with Google**:
   - Click **Sign in with Google**
   - A browser window opens
   - Sign in with your Gmail account (personal accounts work - it's free)
   - Grant the requested permissions
   - Return to Antigravity

3. **Choose your AI model**:
   Antigravity offers multiple AI models. For beginners:
   - **Gemini 3 Pro** - Google's model, well-integrated (recommended to start)
   - **Claude Sonnet 4.5** - Anthropic's model, excellent for explanations
   - **GPT-OSS** - OpenAI-based option

   You can change this later. Pick one and continue.

4. **Welcome tour**:
   Antigravity will show you around. Pay attention to:
   - The **Mission Control** panel (where you interact with agents)
   - The **file explorer** (left side)
   - The **editor area** (center)
   - The **terminal** (bottom)

## Step 4: Configure WSL Integration

Since you have WSL set up, let's connect Antigravity to your Linux environment:

1. Open Antigravity
2. Press `Ctrl + Shift + P` to open the Command Palette
3. Type "WSL" and select **Connect to WSL**
4. Choose **Ubuntu** (or your installed distribution)
5. Antigravity will install some components in WSL (first time only)

Now when you open a terminal in Antigravity, you can choose between:
- **PowerShell** (Windows)
- **Command Prompt** (Windows)
- **Ubuntu (WSL)** (Linux)

### Opening a Folder in WSL

1. Press `Ctrl + Shift + P`
2. Type "WSL: Open Folder in WSL"
3. Navigate to your project folder

Or from Ubuntu terminal:
```bash
# Navigate to your project
cd /mnt/c/UnrealProjects/MyProject

# Open in Antigravity
antigravity .
```

## Step 5: Essential Settings

Let's configure Antigravity for a good development experience:

### Open Settings

Press `Ctrl + ,` (comma) to open Settings.

### Recommended Settings

**Auto Save**:
- Search for "auto save"
- Set `Files: Auto Save` to `afterDelay`
- This prevents losing work

**Font Size**:
- Search for "font size"
- Set `Editor: Font Size` to something comfortable (14-16 is common)

**Word Wrap**:
- Search for "word wrap"
- Set `Editor: Word Wrap` to `on`
- Long lines will wrap instead of requiring horizontal scrolling

**Theme** (Optional):
- Search for "color theme"
- Click to browse and select one you like
- Dark themes are easier on the eyes for long sessions

## Step 6: Understanding Mission Control

Mission Control is what makes Antigravity different from other editors. Here's how to use it:

### Opening Mission Control

- Click the rocket icon in the sidebar, OR
- Press `Ctrl + Shift + M`

### Types of Interactions

**Ask** - Get answers to questions:
```
What does this function do?
How do I create a new class in Unreal Engine?
Explain this error message.
```

**Plan** - Create a plan for a feature:
```
Plan how to add a health system to my game character.
```

**Build** - Have agents write code:
```
Create a simple inventory system with add, remove, and display functions.
```

**Fix** - Debug issues:
```
This code throws a null pointer exception. Find and fix the bug.
```

### Best Practices for AI Interaction

1. **Be specific** - "Add a jump feature" is vague. "Add a double-jump that consumes stamina" is specific.

2. **Provide context** - Tell it what files are relevant, what you've already tried.

3. **Review everything** - Don't blindly accept AI suggestions. Read and understand them.

4. **Ask "why"** - When the AI does something, ask it to explain why.

5. **Learn from it** - The goal is to eventually not need the AI for basic tasks.

## Step 7: Install Useful Extensions

Antigravity supports extensions to add functionality. Here are some useful ones for our work:

### How to Install Extensions

1. Click the Extensions icon in the sidebar (looks like four squares)
2. Search for the extension name
3. Click **Install**

### Recommended Extensions

**For Unreal Engine:**
- **Unreal Engine Tools** - Syntax highlighting for Unreal files
- **C/C++ Extension Pack** - Essential for C++ development

**For General Development:**
- **GitLens** - Enhanced Git integration
- **Error Lens** - Shows errors inline in your code
- **Bracket Pair Colorizer** - Colors matching brackets

**For Blueprints (if available):**
- Search for "Blueprint" to see if there are any Unreal Blueprint extensions

### Installing from Command Line

You can also install extensions via command:
```
Ctrl + Shift + P > "Install Extension" > paste extension ID
```

## Step 8: Test Your Setup

Let's verify everything works:

### Test 1: Create a File

1. Press `Ctrl + N` to create a new file
2. Type some text
3. Press `Ctrl + S` to save
4. Choose a location and filename (e.g., `test.txt`)

### Test 2: Open Terminal

1. Press `` Ctrl + ` `` (backtick) to open terminal
2. Click the dropdown to select "Ubuntu (WSL)"
3. Type `pwd` and press Enter
4. You should see a Linux path

### Test 3: Try Mission Control

1. Open Mission Control (`Ctrl + Shift + M`)
2. Type: "Explain what a variable is in programming"
3. Press Enter
4. You should get an explanation

### Test 4: Open a Folder

1. Press `Ctrl + K` then `Ctrl + O`
2. Navigate to a folder (like `C:\UnrealProjects`)
3. Click **Select Folder**
4. The folder should appear in the sidebar

## Keyboard Shortcuts

Learn these - they'll save you hours:

| Action | Shortcut |
|--------|----------|
| Open Command Palette | `Ctrl + Shift + P` |
| Open Settings | `Ctrl + ,` |
| Open Terminal | `` Ctrl + ` `` |
| Open Mission Control | `Ctrl + Shift + M` |
| New File | `Ctrl + N` |
| Save | `Ctrl + S` |
| Save All | `Ctrl + K S` |
| Find in File | `Ctrl + F` |
| Find in All Files | `Ctrl + Shift + F` |
| Go to File | `Ctrl + P` |
| Toggle Sidebar | `Ctrl + B` |
| Split Editor | `Ctrl + \` |
| Close Tab | `Ctrl + W` |

## Troubleshooting

### "Can't sign in with Google"

- Check your internet connection
- Try a different browser for the sign-in popup
- Clear browser cookies and try again
- Make sure you're using a personal Gmail (not a school/work account that might have restrictions)

### "WSL integration not working"

1. Make sure WSL is installed and working (test by opening Ubuntu directly)
2. Restart Antigravity
3. Try: `Ctrl + Shift + P` > "WSL: Reinstall Server"

### "AI responses are slow"

- This depends on server load and your internet
- Try switching to a different AI model
- Check your internet connection speed

### "Extensions won't install"

- Check internet connection
- Try: `Ctrl + Shift + P` > "Clear Extension Cache"
- Restart Antigravity

### "Antigravity is using too much memory"

- Close unused tabs
- Disable extensions you're not using
- Restart Antigravity periodically during long sessions

## Using Antigravity Responsibly for Learning

Antigravity's AI is powerful, but remember:

### Do:
- Use it to explain concepts you don't understand
- Ask it to review your code and suggest improvements
- Use it when you're truly stuck
- Have it explain WHY it wrote code a certain way

### Don't:
- Let it write everything without understanding
- Accept suggestions without reading them
- Skip learning fundamentals because "AI can do it"
- Copy AI code without testing it

The goal is to become a better programmer, not to become dependent on AI.

## What's Next

You now have a powerful AI-assisted code editor set up! Next, we'll install Claude Code for terminal-based AI assistance:

**[Continue to Claude Code Setup](05-claude-code.md)**

---

## Quick Reference

| Task | How |
|------|-----|
| Open Antigravity | Windows Key > type "Antigravity" |
| Open folder | `Ctrl + K`, `Ctrl + O` |
| Open terminal | `` Ctrl + ` `` |
| Mission Control | `Ctrl + Shift + M` |
| Command Palette | `Ctrl + Shift + P` |
| Settings | `Ctrl + ,` |
| Connect to WSL | Command Palette > "Connect to WSL" |

## Sources

- [Google Antigravity Official Site](https://antigravity.google/)
- [Google Developers Blog - Antigravity Announcement](https://developers.googleblog.com/build-with-google-antigravity-our-new-agentic-development-platform/)
- [Antigravity Download Page](https://antigravityai.org/download)
