# Unreal Engine 5 WSL Setup Guide

Running Unreal Engine 5 inside WSL (Windows Subsystem for Linux) is possible but requires specific configuration.

> [!WARNING]
> **Performance Warning**: Running UE5 inside WSL can be significantly slower than running it natively on Windows due to virtualization overhead.
> **Recommendation**: If you have access to the Windows host, it is **strongly recommended** to install Unreal Engine 5 natively on Windows via the Epic Games Launcher.
>
> If you **must** use WSL, follow the steps below.

## Prerequisites

1.  **Windows 11** (Recommended) or Windows 10 Build 19044+.
2.  **WSL 2** (Not WSL 1). Check with `wsl --list --verbose` in PowerShell.
3.  **WSLg**: This allows running GUI apps from Linux. It is included by default in updated Windows 11 WSL installations.
4.  **GPU Drivers**: Install the latest **NVIDIA GeForce** or **AMD Adrenalin** drivers **on Windows**. Do *not* install Linux GPU drivers inside WSL. WSL inherits the GPU from Windows.

## Step 1: Install Dependencies in WSL

Open your WSL terminal (Ubuntu recommended) and run:

```bash
sudo apt update
sudo apt upgrade -y
sudo apt install -y build-essential clang lld gdb cmake git
sudo apt install -y libvulkan1 mesa-vulkan-drivers vulkan-tools
sudo apt install -y libx11-dev libxcursor-dev libxrandr-dev libxinerama-dev libxi-dev libgl1-mesa-dev libglu1-mesa-dev
```

## Step 2: Download Unreal Engine

1.  Go to the [Unreal Engine Website](https://www.unrealengine.com/).
2.  Log in and go to **Download** -> **Linux**.
3.  Download the **Linux Precompiled Binary**.
4.  Move the downloaded file into your WSL file system (e.g., `\\wsl.localhost\Ubuntu\home\youruser\Downloads`).

## Step 3: Extract and Install

```bash
mkdir -p ~/UnrealEngine
# Replace with your actual filename
tar -xvf ~/Downloads/UnrealEngine-*-Linux.tar.gz -C ~/UnrealEngine
```

## Step 4: Run the Engine

1.  Navigate to the binary directory:

```bash
cd ~/UnrealEngine/Linux_Unreal_Engine_*/Engine/Binaries/Linux
```

2.  Run the editor:

```bash
./UnrealEditor
```

*Note: The first launch will take a long time to compile shaders. If it crashes or hangs, you might be running out of RAM. WSL by default caps RAM usage.*

## Step 5: Create Your Project

1.  When the **Unreal Project Browser** opens:
    -   Select **Games** -> **Blank**.
    -   Select **C++**.
    -   Name: `SimpleRunner`.
    -   Location: `/home/rcgalbo/unreallll`.
2.  Click **Create**.

## Troubleshooting WSL Issues

-   **"Vulkan not found"**: Ensure your Windows GPU drivers are up to date. Run `vulkaninfo` in WSL to check if it sees your GPU.
-   **Slow Performance**: Create a `.wslconfig` file in your Windows User folder (`C:\Users\YourUser\.wslconfig`) to give WSL more resources:
    ```ini
    [wsl2]
    memory=16GB  # Adjust based on your system RAM
    processors=8
    ```
-   **Black Screen**: Try launching with `./UnrealEditor -opengl4` (though Vulkan is preferred).

Once the project is created, let me know!
