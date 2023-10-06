# Windows Web Developer Setup - The complete Guide

## Prerequisites

- Windows 10 version 2004 and higher or Windows 11
- A GitHub account

## 🐧 WSL

The first and most important part of setting up your dev environment.

### Installation

Enter the following command into PowerShell or Command Prompt:

```shell
wsl --install
```

This command does the following:

- Enables the optional WSL and Virtual Machine Platform components
- Downloads and installs the latest Linux kernel
- Sets WSL 2 as the default
- Downloads and installs the Ubuntu Linux distribution (a reboot may be required)

### User Config

Once the process of installing your Linux distribution with WSL is complete, open the distribution using the Start menu. You will be asked to create a User Name and Password for your Linux distribution.

- Once you create a User Name and Password, the account will be your default user for the distribution and automatically sign-in on launch.
- This account will be considered the Linux administrator, with the ability to run sudo (Super User Do) administrative commands.

### Updating Linux

It is recommended that you regularly update and upgrade your packages.

```bash
sudo apt update && sudo apt upgrade
```

### Mapping your Linux Drive

We are going to add our Ubuntu virtual drive as a network location for easy access.

1. Open the ```\\wsl$\``` location from file explorer
2. Right-click on the Ubuntu folder, and select ```Connecter un lecteur réseau```
3. Select the drive letter you would like to use, leave ```Se reconnecter lors de la connectionn``` checked and ```Se connecter à l'aide d'informations d'identifications différentes``` unchecked, and then click finish.

### Pin Your Code Directory

Another quick tip I have is to create a code directory inside of Ubuntu, and then pin it to the quick access menu found on the left side of the file explorer.

1. Open file explorer and click on the Ubuntu network drive we created
2. Select the home dir, and then your user directory
3. Right-click and create a new folder, name it code, or whatever name you like
4. Drag that new folder to the left, underneath the star icon that says ```Quick access```

### Restarting WSL

If for some reason WSL stops working, you can restart it with these two commands from PowerShell:

```shell
wsl.exe --shutdown
wsl.exe
```

If you go back to your Linux shell everything should be back to normal.

## 👨‍💻 Windows Terminal

To launch a Linux terminal we currently need to use the Ubuntu icon from the Start menu or enter the wsl or bash commands into PowerShell. Another option that will give us more features is the Windows Terminal.

### Installation

Download and install Windows Terminal through the [Microsoft Store](https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701?hl=en-us&gl=us&rtc=1&activetab=pivot%3Aoverviewtab).
If for some reason (like me) Windows Store is blocked on your machine, download the ```Microsoft.WindowsTerminal_1.17.11461.0_8wekyb3d8bbwe.msixbundle``` file from the **Assets** section of the [official release page](https://github.com/microsoft/terminal/releases/tag/v1.17.11461.0). To install the app, you can simply double-click on the ```.msixbundle``` file.
If that fails for any reason, you can try the following command at a PowerShell prompt:

```shell
Add-AppxPackage <PATH TO THE FILE>/Microsoft.WindowsTerminal_1.17.11461.0_8wekyb3d8bbwe.msixbundle
```

#### Terminal Settings

##### Default profile

Windows Terminal will open a PowerShell or Command Prompt shell when launched by default, here is how to switch it to WSL:

1. Select the ```˅``` icon from Windows Terminal and go to the Settings menu
2. In the Startup section you will find the Default profile dropdown, select Ubuntu. Below it, select Windows Terminal as the Default terminal application

##### Starting directory

A default Ubuntu terminal will open to the root directory. To make finding your files a little quicker we can have it open into your home directory instead:

1. Under the Profiles section in the settings menu click on Ubuntu
2. At the General tab, you will find a Starting directory input
3. Enter the following replacing "username" with your Ubuntu username: ```\\wsl$\Ubuntu\home\username```

## 📝 Git

Git should come pre-installed on most, if not all of the WSL Linux distributions. To ensure you have the latest version, use the following command:

```bash
sudo apt install git
```

### Config

The name to display in the commits:

```bash
git config --global user.name "John Doe"
```

The email linked to the commits (should be the one that you use on GitHub):

```bash
git config --global user.email "johndoe@email.com"
```

Turn on the colors in the display of the Git commands:

```bash
git config --global color.ui true
```

## 😺 GitHub Credentials (optional)

To avoid having to type in your credentials every time you push or pull soéething from your machine to GitHub you can access and write data on your repositories using SSH protocol.

### Generating a new SSH key

You can generate a new SSH key on your local machine. After you generate the key, you can add the public key to your account on GitHub.com to enable authentication for Git operations over SSH.

1. Open Terminal
2. Paste the text below, substituting in your GitHub email address.

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

3. When you're prompted to "Enter a file in which to save the key", you can press Enter
4. At the prompt, type a secure passphrase.

```bash
> Enter passphrase (empty for no passphrase): [Type a passphrase]
> Enter same passphrase again: [Type passphrase again]
```

5. Start the ssh-agent in the background.

```bash
eval "$(ssh-agent -s)"
```

6. Start the ssh-agent in the background.

```bash
ssh-add ~/.ssh/id_ed25519
```

### Adding a new SSH key to your GitHub account

1. Copy the SSH public key to your clipboard

```bash
cat ~/.ssh/id_ed25519.pub
# Then select and copy the contents of the id_ed25519.pub file
# displayed in the terminal to your clipboard
```

2. On GitHub.com go to ```settings > SSH and GPG keys```
3. Click "New SSH key or Add SSH key"
4. In the "Title" field, add a descriptive label for the new key.
5. Select "authentication" for the type of key
6. In the "Key" field, paste your public key.
7. Click Add SSH key.

## 📦 Node.js and NPM

Node and NPM should be already installed in Ubuntu but the version is really old (older than patek V6) and we may need to be able to switch easily between Node versions. That's why we'll use [NVM](https://github.com/nvm-sh/nvm).

### cURL

First off, we need to make sure we have cURL installed.

```bash
sudo apt install curl
```

### Installation (the easy way ✌)

1. Install NVM with the following command:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
```

2. That's it ! Just verify the installation with:

```bash
nvm -v
> 0.39.5
```

### Installation (the less easy way 🙃)

The easy download may be blocked by our security policy but there's a work around:

1. clone this repo in the root of your user profile

```bash
# Go to root
cd ~/
# Clone the repo
git clone https://github.com/nvm-sh/nvm.git .nvm
```

2. ```cd ~/.nvm``` and check out the latest version with ```git checkout v0.39.5```
3. Add these lines to your ```~/.bashrc``` file to have it automatically sourced upon login:

```bash
# Load nvm
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

### Usage

You can install the desired version of node with the folling command:

```bash
nvm install --lts # for latest stable version
# or
nvm install <version> # for a specific version
```

The corresponding version of NPM will be installed automaticaly alongside Node.
You can test that everything work with these two commands:

```bash
node -v
npm -v
```
