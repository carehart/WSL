---
title: Frequently Asked Questions (FAQ)
description: Frequently Asked Questions about the Windows Subsystem for Linux.
keywords: BashOnWindows, bash, wsl, windows, windowssubsystem, faq
ms.date: 9/4/2018
ms.topic: article
ms.assetid: 129101ed-b88a-43c2-b6a2-cd2c4ff6fee1
ms.localizationpriority: high
---

# Frequently Asked Questions about Windows Subsystem for Linux

## What is Windows Subsystem for Linux (WSL)?

The Windows Subsystem for Linux (WSL) is a new Windows 10 feature that enables you to run native Linux command-line tools directly on Windows, alongside your traditional Windows desktop and modern store apps.

See the [about page](./about.md) for more details.

## Who is WSL for?

This is primarily a tool for developers -- especially web developers and those who work on or with open source projects. This allows those who want/need to use Bash, common Linux tools (`sed`, `awk`, etc.) and many Linux-first tools (Ruby, Python, etc.) to use their toolchain on Windows.

## What can I do with WSL?

WSL provides an application called Bash.exe that, when started, opens a Windows console running the Bash shell. Using Bash, you can run command-line Linux tools and apps. For example, type `lsb_release -a` and hit enter; you’ll see details of the Linux distro currently running:

![Screenshot of distro details](media/distro.png)

You can also access your local machine’s filesystem from within the Linux Bash shell – you’ll find your local drives mounted under the `/mnt` folder. For example, your `C:` drive is mounted under `/mnt/c`:  

![Screenshot of mounted C drive](media/ls.png)

## What is Bash?

[Bash](https://en.wikipedia.org/wiki/Bash_%28Unix_shell%29) is a popular text-based shell and command-language. It is the default shell included within Ubuntu and other Linux distros, and in macOS. Users type commands into a shell to execute scripts and/or run commands and tools to accomplish many tasks.

## How does this work?

Check out our [blog](https://blogs.msdn.microsoft.com/wsl/) where we go into detail about the underlying technology.

## Why would I use WSL rather than Linux in a VM?

WSL requires fewer resources (CPU, memory, and storage) than a full virtual machine. WSL also allows you to run Linux command-line tools and apps alongside your Windows command-line, desktop and store apps, and to access your Windows files from within Linux. This enables you to use Windows apps and Linux command-line tools on the same set of files if you wish.

## Why would I use, for example, Ruby on Linux instead of on Windows?

Some cross-platform tools were built assuming that the environment in which they run behaves like Linux. For example, some tools assume that they are able to access very long file paths or that specific files/folders exist. This often causes problems on Windows which often behaves differently from Linux.

Many languages like Ruby and node are often ported to, and run great, on Windows. However, not all of the Ruby Gem or node/NPM library owners port their libraries to support Windows, and many have Linux-specific dependencies. This can often result in systems built using such tools and libraries suffering from build and sometimes runtime errors or unwanted behaviors on Windows.

These are just some of issues that caused many people to ask Microsoft to improve Windows’ command-line tools and what drove us to partner with Canonical to enable native Bash and Linux command-line tools to run on Windows.

## What does this mean for PowerShell?

While working with OSS projects, there are numerous scenarios where it’s immensely useful to drop into Bash from a PowerShell prompt. Bash support is complementary and strengthens the value of the command-line on Windows, allowing PowerShell and the PowerShell community to leverage other popular technologies.

Read more on the PowerShell team blog -- [Bash for Windows: Why it’s awesome and what it means for PowerShell](https://blogs.msdn.microsoft.com/powershell/2016/04/01/bash-for-windows-why-its-awesome-and-what-it-means-for-powershell/)

## Can I run ALL Linux apps in WSL?

No! WSL is a tool aimed at enabling users who need them to run Bash and core Linux command-line tools on Windows.

WSL does **not** aim to support GUI desktops or applications (e.g. Gnome, KDE, etc.)  

Also, even though you will be able to run many popular server applications (e.g. Redis), we do not recommend WSL for hosting production services – Microsoft offers a variety of solutions for running production Linux workloads in Azure, Hyper-V, and Docker. 

## What Windows SKUs is WSL included in?

Windows Subsystem for Linux is available on the desktop version of Windows for Windows 10 Anniversary and Creators update or later.

Beginning in the Fall Creators update WSL will be available on both the desktop and server SKUs of Windows.

## What processors does WSL support?

WSL supports x64 and ARM CPUs.

## How do I access my C: drive?

Mount points for hard drives on the local machine are automatically created and provide easy access to the Windows filesystem.

**/mnt/\<drive letter>/**

Example usage would be `cd /mnt/c` to access c:\

## How do I set up Git Credential Manager? (How do I use my Windows Git permissions in WSL?) 

Git Credential Manager enables you to authenticate a remote Git server, even if you have a complex authentication pattern like Azure Active Directory or two-factor authentication. Git Credential Manager integrates into the authentication flow for services like GitHub and, once you're authenticated to your hosting provider, requests a new authentication token. It then stores the token securely in the Windows Credential Manager. After the first time, you can use git to talk to your hosting provider without needing to re-authenticate. It will just access the token in the Windows Credential Manager.

To set up Git Credential Manager for use with a WSL distribution, open your distribution and enter this command:

```Bash
git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/libexec/git-core/git-credential-manager.exe"
```

Now any git operation you perform within your WSL distribution will use the credential manager. If you already have credentials cached for a host, it will access them from the credential manager. If not, you'll receive a dialog response requesting your credentials, even if you're in a Linux console.

This support relies on the [interoperability between Windows Subsystem for Linux and Windows itself](https://docs.microsoft.com/windows/wsl/interop).

## How do I use a Windows file with a Linux app?

One of the benefits of WSL is being able to access your files via both Windows and Linux apps or tools. 

WSL mounts your machine's fixed drives under the `/mnt/<drive>` folder in your Linux distros. For example, your `C:` drive is mounted under `/mnt/c/`

Using your mounted drives, you can edit code in, for example, `C:\dev\myproj\` using [Visual Studio](https://visualstudio.microsoft.com/vs/) / or [VS Code](https://code.visualstudio.com/), and build/test that code in Linux by accessing the same files via `/mnt/c/dev/myproj`.

> **IMPORTANT NOTE**: One of the key limitations of using WSL is that directly accessing/changing files in your Linux distros' filesystem using Windows apps or tools is not supported. See: [Do not change Linux files using Windows apps and tools](https://blogs.msdn.microsoft.com/commandline/2016/11/17/do-not-change-linux-files-using-windows-apps-and-tools/)

## Are files in the Linux drive different from the mounted Windows drive?

1. Files under the Linux root (i.e. `/`) are controlled by WSL which mimics Linux specific behavior, including but not limited to:
   * Files which contain invalid Windows filename characters
   * Symlinks created for non-admin users
   * Changing file attributes through chmod and chown
   * File/folder case sensitivity

2. Files in mounted drives are controlled by Windows and have the following behaviors:
   * Support case sensitivity
   * All permissions are set to best reflect the Windows permissions

## Why are there so many errors when I run apt-get upgrade?

Some packages use features that we haven't implemented yet. `udev`, for example, isn't supported yet and causes several `apt-get upgrade` errors.

To fix issues related to `udev`, follow the following steps:

1. Write the following to `/usr/sbin/policy-rc.d` and save your changes.

    ```bash
    #!/bin/sh
    exit 101
    ```

2. Add execute permissions to `/usr/sbin/policy-rc.d`

    ```bash
    chmod +x /usr/sbin/policy-rc.d
    ```
  
3. Run the following commands

    ```bash
    dpkg-divert --local --rename --add /sbin/initctl
    ln -s /bin/true /sbin/initctl
    ```

## How do I uninstall a WSL Distribution?

On builds prior to 1709 (16299) open a command prompt and run:

  ```batchfile
  lxrun /uninstall /full
  ```
  
WSL distributions installed from the store can be uninstalled like any other Windows app, by right-clicking on the app tile and clicking Uninstall, or via PowerShell using the [`Remove-AppxPackage` cmdlet](https://technet.microsoft.com/library/hh856038.aspx).

## Why does ping generate permission denied errors?

In WSL builds < 14926, ping required WSL to run via an elevated Console. This issue was fixed in Build 14926 and later.

## How do I run an OpenSSH server?

Administrator privileges in Windows are required to run OpenSSH in WSL. To run an OpenSSH server, run Bash on Ubuntu on Windows as an administrator, or run bash.exe from a CMD/PowerShell prompt with administrator privileges.

## Why do I get "Error: 0x80040306" when I try to install?

WSL does not support running in a legacy console. To turn off legacy console:

1. Open WSL, PowerShell, or Cmd
1. Right click title bar -> Properties -> Uncheck "Use legacy console"
1. Click OK

## Why do I get "Error: 0x80040154" when I run bash.exe after upgrading Windows?

The "Windows Subsystem for Linux" feature may be disabled during a Windows update. If this happens the Windows feature must be re-enabled. Instructions for enabling the "Windows Subsystem for Linux" feature can be found in the [Installation Guide](https://docs.microsoft.com/windows/wsl/install-win10#install-the-windows-subsystem-for-linux).

## How do I change the display language of WSL?

WSL install will try to automatically change the Ubuntu locale to match the locale of your Windows install. If you do not want this behavior you can run this command to change the Ubuntu locale after install completes. You will have to relaunch bash.exe for this change to take effect.

The below example changes to locale to en-US:

```bash
sudo update-locale LANG=en_US.UTF8
```

## Why do I not have internet access from WSL?

Some users have reported issues with specific firewall applications blocking internet access in WSL. The firewalls reported are:

1. Kaspersky
1. AVG
1. Avast

In some cases turning off the firewall allows for access. In some cases simply having the firewall installed looks to block access.

## How do I access a port from WSL in Windows?
WSL shares the IP address of Windows, as it is running on Windows. As such you can access any ports on localhost e.g. if you had web content on port 1234 you could https://localhost:1234 into your Windows browser.

## How can I back up my WSL distros, or move them from one drive to another?

The best way to backup or move your distros is via the export/import commands available in Windows Version 1809 and later. You can export your entire distribution to a tarball using the `wsl --export` command. You can then import this distro back into WSL using the `wsl --import` command, which can name a new drive location for the import, allowing you to backup and save states of or move your WSL distributions. 

Please note that traditional backup services that backup files in your Appdata folders (like Windows Backup) will not corrupt your Linux files.

## Where can I provide feedback?

You can share feedback and ask questions through multiple channels.

If you have technical issues, or want to request new features please go to our Github issue tracker:

* [GitHub issue tracker](https://github.com/Microsoft/BashOnWindows/issues)

If you'd like to stay up to date with the latest WSL news you can do so with:

* Our [command-line team blog](https://blogs.msdn.microsoft.com/commandline/)
* Twitter. Please follow [@craigaloewen](https://twitter.com/craigaloewen) on Twitter to learn of news, updates, etc.
