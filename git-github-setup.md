# Git & GitHub Setup Guide

_Saving and sharing your code_

---

## Welcome to Version Control! 👋

Git helps you save your work and track changes over time. GitHub is where you can store your code online and share it with others. Think of Git as "track changes" for your code, and GitHub as Google Drive for your projects.

**Don't worry if this seems confusing at first** – everyone struggles with Git when they're learning. Take it slow, and it will click eventually.

**Time needed**: About 20-30 minutes

---

## What We're Setting Up

1. **Git** - Tracks changes in your code
2. **GitHub Account** - Stores your code online
3. **Connection between them** - So you can easily save your work

## Step 1: Create Your GitHub Account

**Sign up for GitHub:**

1. Go to [github.com](https://github.com)
2. Click the "Sign up" button
3. Enter:
   - **Username** - Choose carefully! This becomes part of your web address (like github.com/yourname)
   - **Email** - Use the same email you just configured with Git
   - **Password** - Make it strong and unique
4. Verify your account through the puzzle/captcha
5. Check your email and click the verification link

**✅ Success!** You now have a GitHub account.

**Free vs Paid:** The free account is perfect for learning and most projects. You don't need to pay for anything.

---

## Step 2: Install Git

<details>
<summary><strong>📱 For macOS Users (Click to expand)</strong></summary>

**Check if Git is already installed:**

Open Terminal and type:

```bash
git --version
```

If you see a version number, Git is already installed! Skip to Step 2.

If not, macOS will prompt you to install it. Click "Install" and wait for it to finish.

**Alternative: If the prompt doesn't appear:**

- Download Git from [git-scm.com](https://git-scm.com/download/mac)
- Open the downloaded file and follow the installer

</details>

<details>
<summary><strong>🐧 For Linux Users (Click to expand)</strong></summary>

**Install Git using your terminal:**

For Ubuntu/Debian:

```bash
sudo apt update
sudo apt install git
```

For Fedora:

```bash
sudo dnf install git
```

**Check it worked:**

```bash
git --version
```

You should see a version number like `git version 2.34.1`

</details>

<details>
<summary><strong>🪟 For Windows Users with WSL (Click to expand)</strong></summary>

Since you're using WSL, open your Ubuntu terminal and check if Git is installed:

```bash
git --version
```

If you see a version number, you're good! If not, install it:

```bash
sudo apt update
sudo apt install git
```

**Important:** Always use Git from within your WSL terminal, not Windows Command Prompt.

</details>

---

## Step 3: Tell Git Who You Are

Git needs to know your name and email for tracking who makes changes.

Open your terminal and run these commands (replace with YOUR information):

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

**Example:**

```bash
git config --global user.name "Jane Smith"
git config --global user.email "jane@email.com"
```

**Check it worked:**

```bash
git config --global user.name
git config --global user.email
```

---

## Step 4: Create Your First Repository

A repository (or "repo") is like a folder for your project. Let's create one to test everything works.

**On GitHub.com:**

1. **Click the green "New" button** (or the "+" icon → "New repository")

2. **Fill in the details:**

   - Repository name: `my-first-repo`
   - Description: "Learning Git and GitHub" (optional)
   - Keep it **Public** (so others can see your work)
   - ✅ Check "Add a README file"
   - Leave other options as default

3. **Click "Create repository"**

**You did it!** You now have a repository on GitHub. You should see a page with your README file.

---

## Step 5: Download Your Repository to Your Computer

Now let's get this repository onto your computer so you can work with it.

<details>
<summary><strong>📱 For macOS Users (Click to expand)</strong></summary>

1. **Copy your repository's web address:**

   - On your repository page, click the green "Code" button
   - Make sure "HTTPS" is selected (not SSH)
   - Click the copy button next to the URL

2. **Clone it to your computer:**
   - Open Terminal
   - Navigate to where you want to save it: `cd ~/Documents`
   - Type: `git clone ` (with a space after clone)
   - Paste the URL you copied
   - Press Enter

**Example:**

```bash
cd ~/Documents
git clone https://github.com/yourname/my-first-repo.git
```

3. **Open the folder:**

```bash
cd my-first-repo
```

**✅ Success!** Your repository is now on your computer.

</details>

<details>
<summary><strong>🐧 For Linux Users (Click to expand)</strong></summary>

1. **Copy your repository's web address:**

   - On your repository page, click the green "Code" button
   - Make sure "HTTPS" is selected (not SSH)
   - Click the copy button next to the URL

2. **Clone it to your computer:**
   - Open Terminal
   - Navigate to where you want to save it: `cd ~/Documents`
   - Type: `git clone ` (with a space after clone)
   - Paste the URL you copied
   - Press Enter

**Example:**

```bash
cd ~/Documents
git clone https://github.com/yourname/my-first-repo.git
```

3. **Open the folder:**

```bash
cd my-first-repo
```

**✅ Success!** Your repository is now on your computer.

</details>

<details>
<summary><strong>🪟 For Windows Users with WSL (Click to expand)</strong></summary>

1. **Copy your repository's web address:**

   - On your repository page, click the green "Code" button
   - Make sure "HTTPS" is selected (not SSH)
   - Click the copy button next to the URL

2. **Clone it to your computer:**
   - Open your Ubuntu terminal
   - Navigate to your home folder: `cd ~`
   - Create a projects folder: `mkdir projects`
   - Go into it: `cd projects`
   - Type: `git clone ` (with a space after clone)
   - Paste the URL you copied (right-click to paste in terminal)
   - Press Enter

**Example:**

```bash
cd ~
mkdir projects
cd projects
git clone https://github.com/yourname/my-first-repo.git
```

3. **Open the folder:**

```bash
cd my-first-repo
```

**✅ Success!** Your repository is now in your WSL environment.

**Important:** Keep your projects in WSL (like `~/projects/`), not in Windows folders.

</details>

---

## Step 6: Make Your First Change

Let's make a change and save it to GitHub!

1. **Open the project in VS Code:**

```bash
code .
```

(The dot means "current folder")

2. **Edit the README file:**

   - Find `README.md` in VS Code
   - Add a new line: "This is my first Git commit!"
   - Save the file (Ctrl+S or Cmd+S)

3. **Check what changed:**
   - In VS Code, open the terminal (Terminal → New Terminal)
   - Type:

```bash
git status
```

- You should see README.md in red (meaning it changed)

4. **Stage your changes** (prepare them to be saved):

```bash
git add README.md
```

Or to add everything:

```bash
git add .
```

5. **Commit your changes** (save them with a message):

```bash
git commit -m "Add my first message to README"
```

6. **Push to GitHub** (upload to the internet):

```bash
git push
```

You might be asked for your GitHub username and password the first time.

7. **Check GitHub:**
   - Go back to your repository page on github.com
   - Refresh the page
   - You should see your change!

**🎉 Congratulations!** You just made your first Git commit and pushed it to GitHub!

---

## Essential Git Commands

Here are the commands you'll use most often:

### The Basic Workflow

```bash
git status              # See what's changed
git add .               # Stage all changes
git commit -m "message" # Save changes with a description
git push                # Upload to GitHub
git pull                # Download latest changes
```

### What Each Command Does

**`git status`** - Shows you what files have changed

**`git add .`** - Prepares ALL changed files to be saved (the dot means "everything")

**`git commit -m "your message"`** - Saves your changes with a description

**`git push`** - Uploads your commits to GitHub

**`git pull`** - Downloads changes from GitHub to your computer

---

## 💡 Tips for Beginners

### Commit Often

Make small commits frequently rather than huge commits rarely. It's like saving your work regularly.

### Write Clear Messages

Instead of: `git commit -m "stuff"`
Write: `git commit -m "Add navigation menu to homepage"`

### When You're Stuck

- `git status` tells you what's happening
- Google the error message - someone else had the same problem
- Don't be afraid to ask for help

### GitHub Desktop

If the command line feels overwhelming, you can use [GitHub Desktop](https://desktop.github.com/) - it's a visual app that does the same things with buttons instead of commands.

---

## What's Next?

Now that Git and GitHub are set up:

1. **Use Git for all your projects** - Even small practice ones
2. **Make commits as you work** - Don't wait until everything is "perfect"
3. **Build your GitHub profile** - It's like your coding portfolio

**Remember:** Git feels confusing at first for EVERYONE. The more you use it, the more natural it becomes. Start with the basic commands and gradually learn more as you need them.

---

## Need More Help?

- **GitHub's Hello World Guide**: [guides.github.com/activities/hello-world](https://guides.github.com/activities/hello-world/)
- **Interactive Git Tutorial**: [learngitbranching.js.org](https://learngitbranching.js.org/)
- **Ask for help**: Don't struggle alone - ask in forums or Discord

**The important thing is to start using Git, even if you don't understand everything yet. Learning by doing is the best way!**
