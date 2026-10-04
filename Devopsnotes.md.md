##  Devops topics for junior devop
### The goal
By the end of this module you will have a working Linux terminal, Git, and VS Code — verified with version commands. Every lab in this course runs in a Linux terminal. Windows and macOS students converge on the same commands after this setup.
### Step 1 — Terminal setup
Choose the path that matches your operating system.
Linux (Ubuntu 20.04+)
You already have a terminal. Update your system and install the base tools:

>sudo apt update && sudo apt upgrade -y
>sudo apt install git curl wget -y

Windows — Install WSL2 + Ubuntu 22.04
WSL2 (Windows Subsystem for Linux) gives you a full Ubuntu environment inside Windows. All course commands run inside WSL2 — identical to native Linux.
Open PowerShell as Administrator and run:

>wsl --install

Restart your computer when prompted. Ubuntu 22.04 installs automatically and opens on first boot. Set a Unix username and password when asked (this is your Linux account, not your Windows account).
After restart, open Ubuntu from the Start menu and run:

>sudo apt update && sudo apt upgrade -y
>sudo apt install git curl wget -y

 All future terminal steps in this course run inside the Ubuntu WSL2 window, not in PowerShell or CMD.
## macOS
Install Homebrew (macOS package manager):

>/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

Follow the prompts. After install, add Homebrew to your PATH (the installer prints the exact command — copy and run it).
Install Git:

>brew install git

### Step 2 — Install Git
Linux / WSL2 (Ubuntu):

>sudo apt install git -y
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

macOS — already done via Homebrew above. Configure:

>git config --global user.name "Your Name"
 
 >git config --global email.name "your email"

Verify:

>git --version  
` git version 2.x.x`


### Step 3 — Install VS Code
Linux / WSL2 (Ubuntu):

>sudo apt install wget gpg apt-transport-https -y
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor | sudo tee /etc/apt/keyrings/packages.microsoft.gpg > /dev/null
echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" | sudo tee /etc/apt/sources.list.d/vscode.list > /dev/null
sudo apt update && sudo apt install code -y

WSL2 users: VS Code is installed on the Windows side. Download from code.visualstudio.com, then open VS Code, press Ctrl+Shift+X, search for WSL, and install the WSL extension. After that, open your Ubuntu terminal and run code . to launch VS Code connected to WSL2.

### Macos:

>brew install --cask visual-studio-code

Verify on all platforms:

`code --version`

`1.x.x`

Checkpoint — verify everything works
Run all three in your terminal before moving to Module 0.2:

>git --version
 
 >code --version 

 >echo "Terminal is working on $(uname -s)"



Expected output:

`git version 2.43.0`

 


If any command fails, do not proceed to Module 0.2. Post the error in Q&A with the exact output.
