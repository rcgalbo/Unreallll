# Windows Prerequisites

Before installing any development tools, let's make sure your Windows system is ready. This guide walks you through checking system requirements and enabling necessary Windows features.

## What You'll Learn

- How to check if your computer can run Unreal Engine
- How to enable Windows features needed for development
- How to keep Windows updated for best compatibility

## Minimum System Requirements

Unreal Engine is a professional game development tool. It needs a reasonably powerful computer:

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| Operating System | Windows 10 64-bit | Windows 11 64-bit |
| Processor (CPU) | Quad-core Intel/AMD, 2.5 GHz | 6-core, 3.0 GHz or faster |
| Memory (RAM) | 8 GB | 32 GB |
| Graphics Card (GPU) | DirectX 11 compatible | NVIDIA GTX 1080 / AMD RX 580 or better |
| Storage | 50 GB free space | SSD with 100+ GB free |

### How to Check Your System Specs

1. Press `Windows Key + I` to open Settings
2. Click **System** (left sidebar)
3. Scroll down and click **About**

Here you'll see:
- **Processor** - Your CPU
- **Installed RAM** - Your memory
- **System type** - Should say "64-bit operating system"

To check your graphics card:
1. Press `Windows Key + X`
2. Click **Device Manager**
3. Expand **Display adapters**

Your graphics card will be listed there.

### Understanding These Requirements

**Why does Unreal need so much power?**

When you're making a game, you're running:
- The game engine (calculating physics, AI, etc.)
- The editor (all the tools and windows)
- Your game itself (for testing)

All of this runs simultaneously. Professional game development simply requires substantial computing power.

**What if my computer doesn't meet requirements?**

You can still learn! Start with:
- Smaller projects
- Lower quality settings in the editor
- Blueprint-only development (less resource intensive than C++)

You won't be building AAA games, but you'll learn the concepts.

## Update Windows

Outdated Windows can cause strange compatibility issues. Let's make sure you're current:

1. Press `Windows Key + I` to open Settings
2. Click **Windows Update** (left sidebar)
3. Click **Check for updates**
4. Install any available updates
5. Restart if prompted
6. Repeat until it says "You're up to date"

This can take a while. Let it finish - don't skip this step.

## Enable Developer Mode

Developer Mode gives you access to additional features needed for development tools.

1. Press `Windows Key + I` to open Settings
2. Click **System** (left sidebar)
3. Click **For developers**
4. Turn ON **Developer Mode**
5. Click **Yes** when prompted

### What Does Developer Mode Do?

It allows:
- Installing apps from any source (not just Microsoft Store)
- Using development tools that need special permissions
- Enabling WSL (which we'll set up later)

## Enable Virtualization (Required for WSL)

WSL (Windows Subsystem for Linux) needs virtualization enabled in your computer's BIOS/UEFI. Let's check if it's already on:

1. Press `Ctrl + Shift + Esc` to open Task Manager
2. Click the **Performance** tab
3. Click **CPU** on the left
4. Look at the bottom right for "Virtualization"

If it says **Enabled**, you're good! Skip to the next section.

If it says **Disabled**, you need to enable it in BIOS:

### Enabling Virtualization in BIOS

This varies by computer manufacturer. Here's the general process:

1. Restart your computer
2. Press the BIOS key repeatedly as it starts (usually `F2`, `F10`, `F12`, `Del`, or `Esc`)
3. Find a section called **Advanced**, **CPU Configuration**, or **Security**
4. Look for:
   - Intel: "Intel Virtualization Technology" or "VT-x"
   - AMD: "SVM Mode" or "AMD-V"
5. Enable it
6. Save and exit (usually `F10`)

**Can't figure it out?** Search "[your computer brand] enable virtualization" online.

## Enable Windows Features

We need to enable some Windows features that are turned off by default:

1. Press `Windows Key` and type "Turn Windows features on or off"
2. Click the result
3. Enable these features (check the boxes):
   - **Virtual Machine Platform**
   - **Windows Subsystem for Linux**
   - **Windows Hypervisor Platform** (if available)
4. Click **OK**
5. Restart when prompted

## Install Windows Terminal (Recommended)

Windows Terminal is a modern terminal application that makes command-line work much nicer.

1. Open **Microsoft Store** (press Windows Key, type "store")
2. Search for "Windows Terminal"
3. Click **Get** to install

You can also install it via command line:
```powershell
winget install Microsoft.WindowsTerminal
```

### Why Use Windows Terminal?

- Multiple tabs (like a web browser)
- Better text rendering
- Easy access to PowerShell, Command Prompt, and WSL
- Customizable colors and fonts

## Verify Everything Is Ready

Let's make sure everything is set up correctly:

### Check 1: Developer Mode
```
Settings > System > For developers > Developer Mode should be ON
```

### Check 2: Virtualization
```
Task Manager > Performance > CPU > Virtualization should say "Enabled"
```
```

### Check 3: Windows Features
```powershell
# Open PowerShell and run:
Get-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```
Should show `State : Enabled`

### Check 4: Windows Is Updated
```
Settings > Windows Update > Should say "You're up to date"
```

## Troubleshooting

### "I can't find Developer Mode"

Make sure you're on Windows 10 version 1607 or later, or Windows 11. Run Windows Update first.

### "Virtualization won't enable in BIOS"

Some laptops lock BIOS settings. Check if there's a BIOS update from your manufacturer, or contact their support.

### "Windows features won't install"

Run Windows Update, restart, then try again. Some features require updates to be installed first.

### "My computer is too slow"

Game development is resource-intensive. Options:
1. Close other applications while working
2. Use lower quality settings in Unreal
3. Consider upgrading RAM (often cheapest upgrade)
4. Use an SSD if you don't have one

## What's Next

Your Windows is now ready for development tools. Next, we'll install Unreal Engine:

**[Continue to Unreal Engine Setup](02-unreal-engine.md)**

---

## Quick Reference

| Task | How |
|------|-----|
| Open Settings | `Windows Key + I` |
| Open Task Manager | `Ctrl + Shift + Esc` |
| Open Device Manager | `Windows Key + X`, then click Device Manager |
| Search Windows | Press `Windows Key` and start typing |
| Open PowerShell | `Windows Key`, type "powershell", press Enter |
