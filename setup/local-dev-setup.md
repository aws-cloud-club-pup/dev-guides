# Development Environment Setup Guide

![Windows](https://img.shields.io/badge/Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white)
![macOS](https://img.shields.io/badge/macOS-000000?style=for-the-badge&logo=apple&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)
![Python](https://img.shields.io/badge/Python_3.11-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![AWS](https://img.shields.io/badge/AWS_CLI-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white)

This guide will walk you through setting up your local development environment for the AWSCCPUP Development Team. Follow each step carefully—these tools are what we'll use for all our projects this cycle.

---

## Prerequisites

**Windows:**
- Windows 10 or later
- Administrator access on your machine
- Stable internet connection

**macOS:**
- macOS 10.15 (Catalina) or later
- Administrator access on your machine
- Stable internet connection

---

## 1. Install and Setup Git

Git is our version control system. We'll use it for all code collaboration and submissions.

### Windows

**Installation:**
1. Download Git for Windows from [git-scm.com](https://git-scm.com/download/win)
2. Run the installer
3. Use **default settings** throughout the installation—just keep clicking "Next"

**Configuration:**

After installation, open **Git Bash** and run these commands (replace with your actual name and email):

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

**Verify:**
```bash
git --version
```

You should see something like `git version 2.x.x`.

### macOS

**Installation:**

Git usually comes pre-installed on macOS. To check, open **Terminal** and run:

```bash
git --version
```

If Git is not installed, macOS will prompt you to install Command Line Developer Tools. Click **"Install"** and follow the prompts.

Alternatively, you can install Git via Homebrew (if you have it):
```bash
brew install git
```

**Configuration:**

Open **Terminal** and run these commands (replace with your actual name and email):

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

---

## 2. Install Python 3.11

We're standardizing on Python 3.11 for consistency across the team.

### Windows

**Installation:**
1. Download Python 3.11.13 from [python.org](https://www.python.org/downloads/release/python-31113/)
2. Run the installer
3. **Important:** Check the box that says **"Add Python 3.11 to PATH"** before clicking Install
4. Choose **"Install Now"**

**Verify:**

Open **Command Prompt** or **PowerShell** and run:
```bash
python --version
```

You should see `Python 3.11.13`.

### macOS

**Installation:**

1. Download Python 3.11.13 from [python.org](https://www.python.org/downloads/release/python-31113/)
2. Run the installer package
3. Follow the installation wizard using default settings

Alternatively, install via Homebrew:
```bash
brew install python@3.11
```

**Verify:**

Open **Terminal** and run:
```bash
python3 --version
```

You should see `Python 3.11.x`.

---

## 3. Install Docker Desktop

Docker lets us containerize applications and run them consistently across different environments.

### Windows

**Installation:**
1. Download Docker Desktop from [docker.com](https://www.docker.com/products/docker-desktop/)
2. Run the installer
3. Follow the installation wizard—use default settings
4. Restart your computer when prompted
5. After restart, Docker Desktop will launch automatically
6. Accept the terms and conditions

**Verify:**

Open **Command Prompt** or **PowerShell** and run:
```bash
docker --version
```

You should see `Docker version 20.x.x` or later.

### macOS

**Installation:**
1. Download Docker Desktop for Mac from [docker.com](https://docs.docker.com/desktop/setup/install/windows-install/)
2. Open the downloaded `.dmg` file
3. Drag the Docker icon to your Applications folder
4. Open Docker from Applications
5. Accept the terms and conditions
6. Docker will ask for privileged access—enter your password to proceed

**Verify:**

Open **Terminal** and run:
```bash
docker --version
```

You should see `Docker version 20.x.x` or later.

---

## 4. Install Package Manager and Development Tools

We'll use a package manager to easily install Node.js, Make, and AWS CLI.

### Windows (using Chocolatey)

**Install Chocolatey:**

Follow the official installation guide at [chocolatey.org/install](https://chocolatey.org/install)

Or run these commands:

1. Open **Windows PowerShell** as **Administrator** (right-click PowerShell → Run as Administrator)
2. Run this command to check if you can run scripts:
   ```powershell
   Get-ExecutionPolicy
   ```
3. If it returns `Restricted`, run this to allow script execution:
   ```powershell
   Set-ExecutionPolicy AllSigned
   ```
4. Now install Chocolatey by running:
   ```powershell
   Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
   ```
5. Wait for the installation to complete
6. Close and reopen PowerShell as Administrator

**Verify:**
```powershell
choco --version
```

You should see the Chocolatey version number.

**Install Node.js, Make, and AWS CLI:**

In the same PowerShell window (as Administrator), run:

```powershell
choco install nodejs -y
choco install make -y
choco install awscli -y
```

The `-y` flag automatically confirms the installation prompts.

**Wait for all installations to complete, then close and reopen PowerShell.**

### macOS (using Homebrew)

**Install Homebrew (if not already installed):**

Open **Terminal** and run:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Follow the on-screen instructions. After installation, you may need to add Homebrew to your PATH—the installer will tell you how.

**Verify:**
```bash
brew --version
```

You should see the Homebrew version number.

**Install Node.js, Make, and AWS CLI:**

In Terminal, run:

```bash
brew install node
brew install make
brew install awscli
```

---

## 5. Verify All Installations

Run all verification commands in one go to make sure everything is set up correctly.

### Windows

Open **Command Prompt** or **PowerShell** and run:

```bash
git --version
python --version
docker --version
node --version
npm --version
make --version
aws --version
```

### macOS

Open **Terminal** and run:

```bash
git --version
python3 --version
docker --version
node --version
npm --version
make --version
aws --version
```

If all commands return version numbers, you're good to go. Congratulations—your dev environment is ready!

---

## Troubleshooting

If you run into any issues during setup, don't hesitate to reach out. Chat **JP Curada** directly and we'll help you sort it out.

---

## Accept GitHub Organization Invitation

You should have received an email invitation to join the AWSCCPUP GitHub organization as a contributor. 

**Important:** Check your email inbox (and spam folder) for the invitation from GitHub. Click the link in the email and accept the invitation. This will give you access to our repositories and allow you to contribute to DevTeam projects.

If you haven't received an invitation, reach out to JP or the DevTeam leads immediately.

---

## Next Steps

Now that your environment is set up and you've joined the GitHub organization, you're ready to:
1. Clone the project repository
2. Follow project-specific setup instructions in each repo's README
3. Start contributing to DevTeam projects

