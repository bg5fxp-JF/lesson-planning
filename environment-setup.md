# Setting Up Your Development Environment

_Getting your computer ready for React development_

---

## Welcome to Setup! 👋

Setting up your development environment is often one of the more challenging parts of learning new technologies. **Don't be discouraged if you encounter hurdles or if it takes some time – that's perfectly normal.**

If you get stuck at any point, remember to:

- Use the course discussion forums
- Ask AI assistants for help
- Search Google for specific error messages
- Take breaks when frustrated

Whether you solve setup issues quickly or it takes a bit longer, **the key is to get your environment ready to build amazing things.**

---

## What We're Installing

We need to install a few essential tools to start building React applications:

1. **WSL** (Windows only) - Linux environment for better development experience
2. **Visual Studio Code** - Your code editor (where you'll write code)
3. **Node.js with pnpm** - JavaScript runtime and package manager (lets you run JavaScript on your computer and manage code libraries)

**Time needed**: About 45-60 minutes

---

## Step 1: Install WSL (Windows Users Only)

<details>
<summary><strong>🪟 For Windows Users - Install WSL First (Click to expand)</strong></summary>

### Why Install WSL First?

We recommend installing WSL before VS Code because it provides a Linux environment that makes React development much smoother on Windows. VS Code will automatically detect and integrate with WSL once both are installed.

### What is WSL and Why Use It?

Setting up a development environment directly on Windows can be tricky when dealing with development tools commonly used in web development. To create a more consistent and manageable environment, we recommend using the Windows Subsystem for Linux (WSL).

WSL allows you to install and run a genuine Linux distribution (like Ubuntu) directly on your Windows machine. It provides a full Unix-like command-line experience seamlessly integrated with Windows, without needing a virtual machine or dual-boot setup.

**Using WSL offers several advantages:**

- **Access to Linux Tooling**: Native access to Linux command-line tools essential for development
- **Environment Consistency**: Develop in an environment closer to typical deployment servers
- **Simplified Workflow**: Manage development tools within Linux while using Windows as your primary OS

### Installing WSL: Step-by-Step Guide

**Step 1: Open Windows Terminal as Administrator**

WSL installation requires administrator privileges.

- **Windows 11**: Press the Windows key, type "terminal", right-click on "Windows Terminal", and select "Run as administrator"
- **Windows 10**: If you don't have Windows Terminal:
  - Open Microsoft Store, search for "Windows Terminal"
  - Install the app by Microsoft Corporation
  - Then right-click and "Run as administrator"

You'll see a User Account Control (UAC) prompt - click "Yes". The terminal opens with a PowerShell prompt.

**Step 2: Run the WSL Installation Command**

In the administrator Windows Terminal, type this command and press Enter:

```bash
wsl --install
```

This downloads and installs WSL components and the latest Ubuntu Linux distribution by default.

**Step 3: Approve UAC Prompt (if needed)**

Another UAC prompt might appear for "Windows Subsystem for Linux" installer. Click "Yes" if it appears.

**Step 4: Reboot Your Computer**

Once the command finishes, Windows requires a reboot to complete setup. Restart your computer when prompted.

**Step 5: Complete Linux Setup (After Reboot)**

After restart, an "Ubuntu" terminal window should open automatically. If not, search for "Ubuntu" in the Start menu and run it.

You'll be prompted to set up your Linux user account:

1. **Create Username**: Enter a username for your Linux environment (doesn't need to match Windows username)

2. **Set Password**:
   - Type a secure password (you won't see characters appear - this is normal Linux behavior)
   - Press Enter, then retype the same password for confirmation

**Step 6: Installation Complete**

You'll see "Installation successful!" and be at the Linux command prompt: `your_username@computer_name:~$`

**✅ Congratulations!** WSL and Ubuntu are now installed and ready for development tools.

**Note**: We'll install VS Code next, which will automatically integrate with your new WSL environment.

</details>

---

## Step 2: Install Visual Studio Code

<details>
<summary><strong>📱 For macOS Users (Click to expand)</strong></summary>

Visual Studio Code is a powerful and popular code editor that makes writing code enjoyable.

**To install VS Code:**

1. **Open your web browser** and go to: [code.visualstudio.com](https://code.visualstudio.com)

2. **Look for the main download button** - the site usually detects you're on macOS

3. **Download the 'Mac Universal' stable build**

4. **Once downloaded:**

   - Find the VS Code zip file in your Downloads folder
   - Double-click to unzip it
   - Drag the Visual Studio Code app to your Applications folder

5. **Launch VS Code** from your Applications folder

6. **First time opening:** You'll see a 'Get Started' welcome page with helpful guides - take a few minutes to explore these if you're new to VS Code

**✅ Success!** VS Code is now installed and ready to use.

</details>

<details>
<summary><strong>🐧 For Linux Users (Click to expand)</strong></summary>

Visual Studio Code is a powerful and popular code editor that makes writing code enjoyable.

**To install VS Code:**

1. **Open your web browser** and go to: [code.visualstudio.com](https://code.visualstudio.com)

2. **Download the appropriate package for your system:**

   - For Ubuntu/Debian: Download the `.deb` package
   - For Fedora/CentOS: Download the `.rpm` package

3. **Install the package:**

   **For Ubuntu/Debian:**

   - Open terminal
   - Navigate to your Downloads folder: `cd ~/Downloads`
   - Install: `sudo dpkg -i code_*.deb`
   - If there are dependency issues: `sudo apt-get install -f`

   **For Fedora/CentOS:**

   - Open terminal
   - Navigate to your Downloads folder: `cd ~/Downloads`
   - Install: `sudo rpm -i code-*.rpm`

4. **Launch VS Code:**

   - You can now launch VS Code from your applications menu
   - Or run `code` in your terminal

5. **First time opening:** You'll see a 'Get Started' welcome page with helpful guides - take a few minutes to explore these if you're new to VS Code

**✅ Success!** VS Code is now installed and ready to use.

</details>

<details>
<summary><strong>🪟 For Windows Users (Click to expand)</strong></summary>

Visual Studio Code is a powerful and popular code editor that makes writing code enjoyable.

**To install VS Code:**

1. **Open your web browser** and go to: [code.visualstudio.com](https://code.visualstudio.com)

2. **Look for the main download button** - the site usually detects you're on Windows

3. **Download the Windows installer**

4. **Run the installer:**

   - Find the downloaded file (usually in Downloads)
   - Double-click to run it
   - Follow the installation wizard
   - **Important:** Make sure to check "Add to PATH" when given the option

5. **Launch VS Code** from your Start menu or desktop shortcut

6. **First time opening:** You'll see a 'Get Started' welcome page with helpful guides - take a few minutes to explore these if you're new to VS Code

**✅ Success!** VS Code is now installed and ready to use.

### How to Know You're Using WSL in VS Code

Since you installed WSL first, VS Code will integrate with it automatically. Here's how to verify you're working in the WSL environment:

**Opening VS Code with WSL:**

1. Open your Ubuntu terminal (from Start menu)
2. Navigate to your project folder: `cd ~/my-projects`
3. Type: `code .` to open VS Code in that folder

**Visual Indicators in VS Code:**

- Look at the **bottom-left corner** of VS Code - you should see a green indicator that says **"WSL: Ubuntu"**
- If you see this, you're correctly using VS Code with WSL
- If you don't see it, click the green button (or empty corner) and select "Connect to WSL"

**Important**: Always work in your WSL environment for React development. Your files should be stored in the Linux filesystem (like `/home/username/projects/`) not in Windows paths (like `/mnt/c/Users/`).

</details>

---

## Step 3: Install Node.js

### What is Node.js?

We will heavily rely on a technology called Node.js, which you can find at [nodejs.org](https://nodejs.org). Node.js is formally defined as a "JavaScript runtime environment." While this terminology might seem a bit confusing initially, the core concept is straightforward.

The JavaScript code that runs directly within your web browser (like Chrome, Firefox, etc.) operates in the browser's built-in environment. It "just works" when embedded in a webpage. However, if you want to run JavaScript code directly on your computer – perhaps as a standalone script or to power a server – you need a different environment capable of executing that code outside the browser context.

This is where Node.js comes in. It provides the necessary "runtime environment" to execute JavaScript files locally or on a server. Therefore, installing Node.js is a mandatory prerequisite for proceeding with React development that involves running JavaScript code outside of a web browser.

### Setting Up Node.js with NVM and pnpm

Setting up development tools might feel overwhelming at first. Don't worry if the installation process takes longer than expected – even experienced developers sometimes spend hours troubleshooting setup issues. What matters is persistence, not speed.

We'll use VS Code's built-in terminal for the installation process, making everything happen in one place.

<details>
<summary><strong>📱 For macOS Users (Click to expand)</strong></summary>

**Step 1: Visit the Node.js Download Page**
Head over to [nodejs.org/en/download](https://nodejs.org/en/download) in your browser.

**Step 2: Pick Your Platform**
Select macOS when prompted for your operating system.

**Step 3: Choose NVM Installation**
You'll see several installation options. Pick NVM (Node Version Manager) – it's the best choice because it lets you easily switch Node.js versions when different projects require different versions.

**Step 4: Switch to pnpm Package Manager**
The page defaults to npm as the package manager. Change this selection to pnpm – it's faster and more efficient with disk space than npm.

**Step 5: Copy the Generated Commands**
The website creates a custom installation script based on your choices. Copy all of this code carefully.

**Step 6: Execute in VS Code**

- Launch VS Code and open a new terminal (Terminal → New Terminal)
- Paste the script you copied and hit Enter
- The script handles everything: installing NVM, setting up Node.js, and configuring pnpm

Watch for any additional prompts – you might need to run a follow-up command or two.

</details>

<details>
<summary><strong>🐧 For Linux Users (Click to expand)</strong></summary>

**Step 1: Visit the Node.js Download Page**
Head over to [nodejs.org/en/download](https://nodejs.org/en/download) in your browser.

**Step 2: Pick Your Platform**
Select Linux when prompted for your operating system.

**Step 3: Choose NVM Installation**
You'll see several installation options. Pick NVM (Node Version Manager) – it's the best choice because it lets you easily switch Node.js versions when different projects require different versions.

**Step 4: Switch to pnpm Package Manager**
The page defaults to npm as the package manager. Change this selection to pnpm – it's faster and more efficient with disk space than npm.

**Step 5: Copy the Generated Commands**
The website creates a custom installation script based on your choices. Copy all of this code carefully.

**Step 6: Execute in VS Code**

- Launch VS Code and open a new terminal (Terminal → New Terminal)
- Paste the script you copied and hit Enter
- The script handles everything: installing NVM, setting up Node.js, and configuring pnpm

Watch for any additional prompts – you might need to run a follow-up command or two.

</details>

<details>
<summary><strong>🪟 For Windows Users (Click to expand)</strong></summary>

**Quick Note for Windows Users:** Since you've already set up WSL, you'll want to use the Linux instructions on the Node.js website for the smoothest experience.

**Step 1: Visit the Node.js Download Page**
Head over to [nodejs.org/en/download](https://nodejs.org/en/download) in your browser.

**Step 2: Pick Your Platform**

- If you're using WSL (recommended): Select Linux
- For native Windows installation: Select Windows

**Step 3: Choose NVM Installation**
You'll see several installation options. Pick NVM (Node Version Manager) – it's the best choice because it lets you easily switch Node.js versions when different projects require different versions.

**Step 4: Switch to pnpm Package Manager**
The page defaults to npm as the package manager. Change this selection to pnpm – it's faster and more efficient with disk space than npm.

**Step 5: Copy the Generated Commands**
The website creates a custom installation script based on your choices. Copy all of this code carefully.

**Step 6: Execute in VS Code**

- For WSL users: Make sure VS Code is connected to WSL (check for "WSL: Ubuntu" in the bottom-left)
- Launch a new terminal (Terminal → New Terminal)
- Paste the script you copied and hit Enter
- The script handles everything: installing NVM, setting up Node.js, and configuring pnpm

Watch for any additional prompts – you might need to run a follow-up command or two.

</details>

### Confirming Everything Works

Once the installation completes, you need to verify everything is set up correctly:

**First, refresh your terminal:**

- Close the current terminal tab (click the trash icon)
- Open a fresh terminal (Terminal → New Terminal)
- This refresh ensures your system recognizes all the new tools

**Test Node.js installation:**
Run this command:

```bash
node --version
```

You should see a version number like `v22.x.x` (the exact number depends on the current LTS release).

**Test pnpm installation:**
Run this command:

```bash
pnpm --version
```

You should see a version number like `10.x.x`.

**Success!** If both commands show version numbers, you've successfully set up Node.js and pnpm with NVM.

### Troubleshooting Help

Having issues with pnpm? Check out their documentation at [pnpm.io/installation](https://pnpm.io/installation) for alternative installation methods and troubleshooting tips.

Your development environment is now ready for React development!

---

## 🧪 Final Test: Make Sure Everything Works

Let's create a quick test to make sure all your tools are working together:

### Test Your Setup

1. **Open your terminal/command prompt**

2. **Check all your tools:**

   ```bash
   node --version
   npm --version
   pnpm --version
   ```

   You should see three version numbers. If any command says "not found," go back and reinstall that tool.

3. **Open VS Code:**

   - **Mac/Linux:** Type `code` in your terminal
   - **Windows:** Type `code` in Command Prompt, or find VS Code in your Start menu

   VS Code should open up.

### 🎉 Congratulations!

If all the version numbers showed up and VS Code opened, **you're ready to start building React applications!**

---

## What's Next?

With your development environment set up, you're ready to:

1. **Set up Git and GitHub** (for saving and sharing your code)

**Remember:** Every professional developer has gone through this setup process. Some have it easy, others face challenges, but everyone gets through it. **You've got this!** 💪
