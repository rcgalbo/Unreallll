# Installing Unreal Engine

Unreal Engine is a professional game engine created by Epic Games. It's used to make everything from indie games to blockbuster titles. Best of all, it's free to use.

## What You'll Learn

- How to create an Epic Games account
- How to install the Epic Games Launcher
- How to download and install Unreal Engine
- How to create your first project

## Understanding the Licensing

Unreal Engine is free with these terms:
- **Free to use** for learning and development
- **Free to publish** games until you earn $1,000,000 in revenue
- **5% royalty** on revenue above $1,000,000

For learning purposes, you'll never pay anything.

## Step 1: Create an Epic Games Account

1. Go to [epicgames.com](https://www.epicgames.com)
2. Click **Sign In** (top right)
3. Click **Sign Up**
4. Choose how to register:
   - **Email** (recommended) - Use your own email address
   - Or connect with Google, Facebook, Xbox, PlayStation, etc.
5. Fill in your details:
   - Display Name (your username)
   - First and Last Name
   - Email address
   - Password (make it strong!)
6. Accept the terms of service
7. Click **Continue**
8. Verify your email by clicking the link Epic sends you

### Tips for Your Account

- Use a password manager to create a strong, unique password
- Enable two-factor authentication (2FA) after creating your account
- This account also works for Fortnite, the Epic Games Store, etc.

## Step 2: Download Epic Games Launcher

The Epic Games Launcher is the application that manages all Epic software including Unreal Engine.

1. Go to [unrealengine.com](https://www.unrealengine.com)
2. Click **Download** (usually top right)
3. Select your platform (Windows)
4. Run the installer when it downloads

### Installing the Launcher

1. Double-click the downloaded `EpicInstaller-xx.x.x.msi` file
2. If Windows asks "Do you want to allow this app to make changes?", click **Yes**
3. Choose installation location (default is fine)
4. Click **Install**
5. Wait for installation to complete
6. Click **Finish**

### First Launch

1. The Epic Games Launcher should open automatically
2. Sign in with the account you created
3. You might see the Epic Games Store - that's normal

## Step 3: Install Unreal Engine

Now we'll install the actual game engine.

1. In the Epic Games Launcher, look at the left sidebar
2. Click **Unreal Engine** (not the store, not your library)
3. Click the **Library** tab
4. Click the **+** button to add an engine version
5. Click the dropdown arrow next to the version number
6. Select the latest version (e.g., "5.4.x")
7. Click **Install**

### Choosing Installation Location

A dialog will appear asking where to install:

- **Default location**: `C:\Program Files\Epic Games\UE_5.x`
- **Custom location**: Click **Browse** to choose

**Important considerations:**
- Unreal Engine is LARGE (40-60 GB per version)
- Install on an SSD if possible (much faster loading)
- Make sure the drive has at least 100 GB free

### Selecting Components

You'll see checkboxes for optional components:

| Component | Size | Do You Need It? |
|-----------|------|-----------------|
| Engine | ~30 GB | Yes (required) |
| Starter Content | ~2 GB | Yes (useful for learning) |
| Templates | ~2 GB | Yes (provides project starting points) |
| Target Platforms | Varies | Only what you need |

For learning, install:
- Engine (required)
- Starter Content
- Templates
- Desktop Development (Windows)

You can skip mobile and console platforms for now.

### Installation Time

This will take a while depending on your internet speed:
- 50 Mbps connection: ~1-2 hours
- 100 Mbps connection: ~30-60 minutes
- 1 Gbps connection: ~10-15 minutes

Let it run. Don't pause and resume repeatedly as this can cause issues.

## Step 4: First Launch

Once installation completes:

1. Click **Launch** in the Epic Games Launcher
2. Wait for Unreal Engine to load (first launch takes longer)
3. You'll see the **Unreal Project Browser**

### Creating Your First Project

Let's create a simple project to verify everything works:

1. In the Project Browser, select **Games** category
2. Choose **Third Person** template
3. Click **Next**
4. Configure your project:
   - **Blueprint** (for now - we'll learn C++ later)
   - **Maximum Quality**
   - **Raytracing**: Off (unless you have RTX GPU)
   - **Starter Content**: Checked
5. Set the location and name:
   - Location: Choose a folder (create a dedicated folder like `C:\UnrealProjects`)
   - Name: `MyFirstProject` (no spaces, no special characters)
6. Click **Create**

### Understanding What Happens

When you click Create, Unreal:
1. Copies the template files to your project location
2. Generates project files
3. Opens the Unreal Editor

First project creation can take 5-10 minutes. Be patient.

## Step 5: Verify It Works

Once the editor opens:

1. You'll see a 3D scene with a mannequin character
2. Click the **Play** button (green arrow in the toolbar)
3. Use WASD to move, mouse to look around
4. Press **Escape** to stop playing

**Congratulations!** You've successfully installed Unreal Engine and created your first project.

## Understanding the Installation

### What Got Installed Where

```
C:\Program Files\Epic Games\
└── UE_5.x\                    # Unreal Engine installation
    ├── Engine\                # The engine itself
    │   ├── Binaries\          # Executable files
    │   ├── Content\           # Engine content
    │   ├── Source\            # Engine source code
    │   └── ...
    ├── Templates\             # Project templates
    └── Samples\               # Sample projects

C:\Users\[You]\AppData\Local\
└── UnrealEngine\              # Your personal engine settings
```

### What's in a Project

```
MyFirstProject\
├── Config\                    # Project settings
├── Content\                   # All your game content (models, textures, etc.)
├── Intermediate\              # Build files (can be deleted to reclaim space)
├── Saved\                     # Logs, autosaves, local settings
├── MyFirstProject.uproject    # The project file (double-click to open)
└── ...
```

## Installing Visual Studio (For C++ Later)

When you're ready to write C++ code (not required for Blueprints), you'll need Visual Studio:

1. Go to [visualstudio.microsoft.com](https://visualstudio.microsoft.com)
2. Download **Visual Studio Community** (free)
3. Run the installer
4. Select these workloads:
   - **Desktop development with C++**
   - **Game development with C++**
5. Install (this is also large, ~10 GB)

We'll cover Visual Studio in detail when we get to C++ development.

## Managing Engine Versions

You can have multiple Unreal versions installed:

- Each version is independent
- Projects are tied to specific versions
- You can upgrade projects, but it's one-way

### Viewing Installed Versions

In Epic Games Launcher:
1. Click **Unreal Engine** in sidebar
2. Click **Library** tab
3. See all installed versions

### Installing Additional Versions

1. Click the **+** button in Library
2. Select version
3. Install

### Removing Old Versions

1. Click the dropdown on the version
2. Select **Remove**
3. Confirm

This frees up 40-60 GB per version.

## Troubleshooting

### "Download keeps failing"

- Check your internet connection
- Try pausing and resuming
- Try at a different time (servers might be busy)
- Disable VPN if using one

### "Not enough disk space"

- Unreal needs ~60 GB for the engine + project space
- Clear your downloads folder
- Uninstall unused programs
- Consider installing on a different drive

### "Editor won't launch"

1. Update graphics drivers (NVIDIA/AMD website)
2. Run as administrator
3. Verify installation (in Launcher, click dropdown > Verify)
4. Check Windows Event Viewer for errors

### "Performance is terrible"

1. Close other applications
2. In Editor: Edit > Editor Preferences > Performance > Use Less CPU when in Background
3. Lower scalability settings: Settings (in viewport) > Scalability > Medium or Low

### "Can't create project"

- Don't use spaces or special characters in project names
- Make sure project location has write permissions
- Don't put projects in Program Files or system folders

## Keeping Unreal Updated

Epic releases regular updates:

1. **Major versions** (5.0 → 6.0): Big changes, new features
2. **Minor versions** (5.3 → 5.4): Features and improvements
3. **Hotfixes** (5.4.1 → 5.4.2): Bug fixes

### Update Strategy

- **Don't update mid-project** unless needed
- **Test updates on a copy** of your project first
- **Read release notes** before updating

## What's Next

You now have Unreal Engine installed and verified working! Next, we'll set up WSL (Windows Subsystem for Linux) for additional development tools:

**[Continue to WSL Setup](03-wsl.md)**

---

## Quick Reference

| Task | How |
|------|-----|
| Open Unreal | Epic Games Launcher > Unreal Engine > Library > Launch |
| Create project | Project Browser > Select template > Create |
| Open existing project | Double-click the `.uproject` file |
| Verify installation | Epic Launcher > Version dropdown > Verify |
| Update engine | Epic Launcher > Version dropdown > Update |
