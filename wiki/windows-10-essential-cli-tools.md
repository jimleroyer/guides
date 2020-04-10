This is a guide for a proper Windows 10 CLI development environment. Yeah, Linux and
MacOSX are pretty awesome in there... but Microsoft has made some tremendeous improvements
in that direction. So either get on the ride with me for a comfy-slipper CLI
to work with on Windows 10.

- [The essential tools](#the-essential-tools)
  - [Your Windows Terminal](#your-windows-terminal)
  - [Scoop -- your package manager](#scoop----your-package-manager)
  - [aria2 -- your download manager](#aria2----your-download-manager)
  - [starship -- your shell prompt](#starship----your-shell-prompt)
  - [pshazz -- your shell prompt (alternative)](#pshazz----your-shell-prompt-alternative)
  - [z.lua -- your directory compass](#zlua----your-directory-compass)
  - [PSReadLine -- your line auto-completion](#psreadline----your-line-auto-completion)
  - [git -- your version control + git auto-completion](#git----your-version-control--git-auto-completion)
  - [docker -- your container tool](#docker----your-container-tool)
- [The extra tools](#the-extra-tools)
  - [GitHub CLI](#github-cli)
  - [gitignore (from [psutils](https://github.com/lukesampson/psutils))](#gitignore-from-psutils)
  - [AWS CLI](#aws-cli)
  - [touch](#touch)
  - [PsHosts](#pshosts)
  - [sudo (from psutils)](#sudo-from-psutils)
- [The Windows Subsystem for Linux](#the-windows-subsystem-for-linux)
  - [Installation](#installation)
    - [Custom Linux distribution](#custom-linux-distribution)
  - [Running the Linux shell using WSL](#running-the-linux-shell-using-wsl)
  - [Graphical applications and misc](#graphical-applications-and-misc)
  - [Caveats](#caveats)
- [Troubleshooting](#troubleshooting)
  - [unable to start ssh-agent service, error :1058](#unable-to-start-ssh-agent-service-error-1058)
  - [I see  icons on the command-line](#i-see--icons-on-the-command-line)
  - [With the `help` cmd, I keep seeing `Get-Help cannot find the Help files for this cmdlet on this computer.`](#with-the-help-cmd-i-keep-seeing-get-help-cannot-find-the-help-files-for-this-cmdlet-on-this-computer)
  - [Git ask me non-stop for my passphrase](#git-ask-me-non-stop-for-my-passphrase)
- [Credits](#credits)

# The essential tools

These are the first and esssential CLI tools to install for a productive developer's experience
on Windows 10.

## Your [Windows Terminal](https://github.com/microsoft/terminal)

> ℹ️ **A must-have** for its rich CLI experience that includes updated and modern features, bringing
the Linux/MacOSX experience to the Windows terminal world. For example, search and re-execute
any previous command by hitting the `CTRL+R` shortcut.
> Windows Terminal is a new, modern, feature-rich, productive terminal application for
command-line users. It includes many of the features most frequently requested by the Windows
command-line community including support for tabs, rich text, globalization, configurability,
theming & styling, and more.

At this time, the modern version of the Windows terminal is not yet available in Windows 10
default installation. So you need to [head to the Microsoft Store and install it from there](https://aka.ms/windowsterminal).

If you already installed  [Chocolatey](https://chocolatey.org/) on your system, you can use
the following command, but if you don't have, get it through the [Microsoft Store](https://aka.ms/windowsterminal) instead, as we'll install Scoop next anyway.

```powershell
choco install microsoft-windows-terminal
```

To learn more on how to use and configure the terminal, head to
[the documentation](https://github.com/microsoft/terminal/blob/master/doc/user-docs/index.md).
To learn about how to customize the background image or colors, [head
in that other direction](https://www.howtogeek.com/426346/how-to-customize-the-new-windows-terminal-app/).

## [Scoop](https://scoop.sh/) -- your package manager

> Scoop installs the tools you know and love

```powershell
Invoke-Expression (New-Object System.Net.WebClient).DownloadString('https://get.scoop.sh')
```

Scoop is a package manager for Windows 10, quite similar to MacOSX HomeBrew in spirit. There
are other package managers such as [Chocolatey](https://chocolatey.org/). From my observations,
Scoop is more oriented to devtools whereas Chocolatey is more to general users' tools. For
example, you can install Kotlin with Scoop but not with Chocolatey... but you can install
the Windows Terminal with the latter! I prefer Scoop so far for its Homebrew feeling.

## [aria2](https://aria2.github.io) -- your download manager

> aria2 is a lightweight multi-protocol & multi-source command-line download utility.
It supports HTTP/HTTPS, FTP, SFTP, BitTorrent and Metalink. aria2 can be manipulated
via built-in JSON-RPC and XML-RPC interfaces.

```powershell
scoop install aria2
```

This should speed up future downloads made by Scoop, as the latter automatically leverages
[aria2](https://aria2.github.io/) once installed.

> ℹ️ **A must-have** for its multi-format capabilities. For example, to download Ubuntu torrent
with secure encrypted transport and no limit on the seed ratio after the download
is made (this will continue to seed after the download until you stop the command):

```powershell
aria2c.exe --bt-force-encryption true --seed-ratio=0.0 -T .\ubuntu-19.10-desktop-amd64.iso.torrent
```

## [starship](https://starship.rs) -- your shell prompt

> ℹ️ **A must-have** for its speed, multi-modules support and high customizability.

```powershell
scoop install starship
```

Easy to use and extend, this is the prompt that will display necessary information in a
smart fashion only when it needs to.

![starship CLI example](https://github.com/jimleroyer/guides/raw/master/images/windows-10-cli-essentials/starship.gif)

## [pshazz](https://github.com/lukesampson/pshazz) -- your shell prompt (alternative)

> Pshazz extends your powershell profile to add things like
>
> - A nicer prompt, including Git and Mercurial info
> - **Git and Mercurial tab completion**
> - An SSH helper that lets you **never enter your private key password again**
> - Sensible aliases, and an easy way to add your own aliases and remove ones you don't like

```powershell
scoop install pshazz
```

> ℹ️ **A must-have** for its *git* command-line auto-completion and branch status.

![pshazz CLI example](https://github.com/jimleroyer/guides/raw/master/images/windows-10-cli-essentials/CLI-pshazz.png)

## [z.lua](https://github.com/skywind3000/z.lua) -- your directory compass

> Tracks your most used directories, based on number of previously run commands. After a short learning phase, z will take you to the most popular directory that matches all of the regular expressions given on the command line. You can use Tab-Completion / Intellisense to pick directories that are not the first choice.

```powershell
scoop install lua
scoop install z.lua

# Create the Profile file if not already existing.
if (!(Test-Path $PROFILE.CurrentUserCurrentHost)) {
  New-Item $PROFILE.CurrentUserCurrentHost
}

$ZLuaLocation = "$(scoop info z.lua | select -Index 7 | ForEach-Object { $_.Trim() })\z.lua"
$ZLuaLine = "`n# z.lua activation`n" + 'Invoke-Expression ($(lua ' + $ZLuaLocation + '  --init powershell) -join "`n")'
Add-Content $PROFILE.CurrentUserCurrentHost $ZLuaLine
```

## PSReadLine -- your line auto-completion

> ℹ️ **A must-have** to have a bash-like autocompletion that shows all choices at once
> instead of the default behavior of cycling through all choices. The latter is quite
> ineffective for long list.

First, let's make sure that the PowerShellGet module latest version is installed. In a
regular PowerShell console, install it with the following command.

```powershell
Install-Module -Name PowerShellGet -Force
```

Then, **close all PowerShell consoles**, including any opened by your favorite IDE such
as VSCode. Open an **admin-elevated regular command terminal (non-PowerShell)** to install
the latest version of PSReadLine.

```cmd
powershell -noprofile -command "Install-Module PSReadLine -Force -SkipPublisherCheck -AllowPrerelease"
```

You might know or discover that PSReadline is already installed on Windows 10. It does
not come with the latest version though. The updated version comes with important bug
fixes and has a *slicker behavior*.

![Slick PSReadLine autocompletion example](https://github.com/jimleroyer/guides/raw/master/images/windows-10-cli-essentials/psreadline.gif)

You will have to update your profile to automatically load it into your environment.
Execute the following and that will be done.

```powershell
# Create the Profile file if not already existing.
if (!(Test-Path $PROFILE.CurrentUserCurrentHost)) {
  New-Item $PROFILE.CurrentUserCurrentHost
}

$PSReadLine = "`n# PSReadline (Better tab completion)`nSet-PSReadlineKeyHandler -Key Tab -Function Complete"
Add-Content $PROFILE.CurrentUserCurrentHost $PSReadLine
```

## [git](https://git-scm.com/) -- your version control + git auto-completion

The popular distributed version control is installable via scoop.

```powershell
scoop install git
```

To enable git autocompletion, install [posh-git](https://github.com/dahlbyk/posh-git)
and enable it to your profile.

```powershell
scoop install posh-git
Add-PoshGitToProfile
```

The second command will automatically add the necessary posh-git import to your
`$profile.CurrentUserCurrentHost` file.

## [docker](https://www.docker.com/) -- your container tool

The one container solution that developers can't contain their jor for. ;)

```powershell
scoop install docker
```

# The extra tools

A set of tools that are not essentials but quite nice to have on the CLI.

## GitHub CLI

This will get the GitHub command-line in your shell environment.

```powershell
scoop install gh
```

Example:

```text
starship [🌊2412] on master [⇕] is 📦 v0.38.1 via 🦀 v1.42.0
➜ gh pr list

Showing 26 of 26 pull requests in starship/starship

#1047  New Crowdin translations                                         i18n_master
#1046  fix(python): Fix venv name not showing when using pyenv          andytom:fix/python_venv
#1035  fix: implement fallback branch_name for bare git repos           hrombach:fix-bare-git-repos
#1033  docs: Fix broken installation link anchor on README              jmfederico:patch-1
#1019  feat: Add Undistract Me Feature                                  schrieveslaach:undistract-me
```

## [gitignore](https://www.gitignore.io/) (from [psutils](https://github.com/lukesampson/psutils))

> Fetches .gitignore file templates from gitignore.io and writes them to standard output.

```powershell
scoop install gitignore
```

> ℹ️ **A must-have** to generate a gitignore file based on a combination of your favorite tools! For
example, if you are an unlikely Kotlin developer using vim to edit its files, the following would
generate gitignore entries for both tools:

```powershell
gitignore vim kotlin > .gitignore
```

## AWS CLI

This will get the AWS command-line in your shell environment.

```powershell
scoop install aws
```

## [touch](https://github.com/lukesampson/psutils)

The classic POSIX command, popularized by Gunther in a ding dong moment.

```powershell
scoop install touch
```

## [PsHosts](https://github.com/richardszalay/pshosts)

> PsHosts is a PowerShell Module that provides Cmdlets for manipulating the local hosts file on Windows, Linux, and macOS. Supports tab completion for hostnames.

```powershell
# Install with admin account with..
Install-Module PsHosts

# ..or with regular user but no creation/destruction commands will be permitted.
Install-Module PsHosts -Scope CurrentUser
```

## sudo ([from psutils](https://github.com/lukesampson/psutils))

A familiar tool to escalate permissions on the CLI.

```powershell
scoop install sudo
```

# [The Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/faq)

The Windows Subsystem for Linux deserves a section of its own, as this requires some
extra instructions. Not all developers might want it but this is an indispensable when
required (or if you just prefer one of the Linux shell :neckbeard:).

> ℹ️ **A must-have** to have a Linux shell and POSIX tools a command away. Use it for projects
and environments that heavily relies on Linux tools and has no compatibility with the
Microsoft CLI tools. The WSL is a compatibility layer for running Linux binary executables
(in ELF format) natively on Windows 10. Hence, with WSL enabled, Windows becomes a hybrid
system that can adapt to the environments built for Linux command line environments (and
potentially with POSIX compatible environments such as MacOSX), completely embracing
these to provide the ultimate developer's experience.

## Installation

The [installation guide is over here](https://docs.microsoft.com/en-us/windows/wsl/install-win10),
but... here are the steps, basically:

1. Make sure the WSL is activated in your Windows 10 installation.

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

You will have to reboot if that wasn't already activated.

You have two choices next...

2.a. Head over to the Windows store to install your favorite Linux distribution.
[Here is the link for Ubuntu.](https://www.microsoft.com/en-us/p/ubuntu-1804/9n9tngvndl3q)

2.b. Or if you're a lazy programmer at heart and don't want to leave the comfort of the
command-line (for Ubuntu 18/x64 arch):

```powershell
Add-AppxPackage .\CanonicalGroupLimited.Ubuntu18.04onWindows_1804.2018.817.0_x64__79rhkp1fndgsc.Appx
```

### Custom Linux distribution

Installing a custom distribution of your choice seems possible and some tools exist to
make the task easy to perform. [WSLInstall](https://github.com/Biswa96/WSLInstall) is
one which will require you to run the tool on your favorite distribution.

## Running the Linux shell using WSL

To start the installed distribution into your session, you can either start it from your
Start menu or by executing the (*wsl*)[https://docs.microsoft.com/en-us/windows/wsl/reference]
command on the prompt. With no parameters, the command will launch your default distribution:

```powershell
wsl
```

Once the Linux-flavored shell comes up (most likely bash), make sure to update your distribution
to its latest:

```bash
sudo apt update && sudo apt upgrade -y
```

Your distribution will keep its updates on the next WSL invocation, i.e. there
is no need to re-run it every time. Make sure to run it once in a while though
as this is no part of Windows 10 automated updates.

## Graphical applications and misc

[The Ubuntu Wiki page on WSL](https://wiki.ubuntu.com/WSL) has many goodies on proper
installation and even guidance on how to install graphical application using a *X Server*.
**I didn't try it myself.**

## Caveats

> :warning: **A word of caution**: Do not modify the Linux files using the Windows application and tools.
For more information on that topic, [head over at this link](https://devblogs.microsoft.com/commandline/do-not-change-linux-files-using-windows-apps-and-tools/#comment-85115).

# Troubleshooting

## unable to start ssh-agent service, error :1058

It's possible that the ssh-agent service is currently not running. Check the status with that command:

```powershell
Get-Service | Where {$_.name –eq 'ssh-agent'}
```

If the status is *stopped*, run that command to start it:

```powershell
Start-Service -Name ssh-agent
```

This should start the ssh-agent.

## I see  icons on the command-line

The fonts that the custom shells use are UTF-8, so you'll need UTF-8 fonts installed. You'll also need so-called "powerline" fonts. Fortunately, there's a simple script you can run to install a whole bunch of cool fonts that will work nicely on your consoles.

Here are the steps for installing the fonts. Open a PowerShell and enter the following commands:

You will need UTF-8 fonts installed that the custom themes make use of. You can get them via
some github repository and by executing a PowerShell script with the following:

```powershell
cd $projectDirectory
git clone https://github.com/powerline/fonts.git
.\install.ps1
```

## With the `help` cmd, I keep seeing `Get-Help cannot find the Help files for this cmdlet on this computer.`

The help files are probably not available on your Windows installation. Open a **PowerShell**
by **running it as an administrator**. You can't do that with the Windows Terminal: it has to
be a PowerShell terminal. Once opened, simply running the `help` alias command should trigger
the file download but you can explicitly ask for it by running this command:

```powershell
Update-Help
```

All help files should be downloaded without any error. You can then have a help description
on the next execution of the `help` alias command, even for non-admin users and in the Windows
Terminal.

## Git ask me non-stop for my passphrase

Follow steps as detailed [in this StackOverflow answer](https://stackoverflow.com/a/58784438/647641).
This is the simplest solution for Windows 10 that does not involve extra installation of
tools whatsoever.

# Credits

- Mattie Behrens for pointing out PSReadLine in his article titled [Making Development with PowerShell More Hospitable](https://spin.atomicobject.com/2019/05/09/powershell-tools/)
