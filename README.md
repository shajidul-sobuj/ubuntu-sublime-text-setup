# Sublime Text Setup on Ubuntu

A clean, step-by-step guide to installing and managing **Sublime Text Stable** on Ubuntu using the official APT repository.

## Overview

- **Editor:** Sublime Text
- **Operating System:** Ubuntu Linux
- **Installation Method:** Official APT Repository
- **Release Channel:** Stable
- **Package Manager:** APT

## 1. Add the Official GPG Signing Key

Download and register Sublime Text's repository signing key:

```bash
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg \
  | sudo tee /etc/apt/keyrings/sublimehq-pub.asc > /dev/null
```

## 2. Configure the Stable Repository

Add the official **Stable** release channel:

```bash
echo -e 'Types: deb\nURIs: https://download.sublimetext.com/\nSuites: apt/stable/\nSigned-By: /etc/apt/keyrings/sublimehq-pub.asc' \
  | sudo tee /etc/apt/sources.list.d/sublime-text.sources
```

> **Note:** This configuration uses the Stable channel, not the Dev channel.

## 3. Update Package Lists

Refresh APT package information:

```bash
sudo apt-get update
```

## 4. Install Sublime Text

Install the editor:

```bash
sudo apt-get install sublime-text
```

## 5. Verify the Installation

Check the installed version:

```bash
subl --version
```

If the command returns a version number, the Sublime Text command-line launcher is available.

## 6. Launch Sublime Text

Open Sublime Text from the terminal:

```bash
subl
```

Open the current directory as a project:

```bash
subl .
```

Open a specific file:

```bash
subl filename.txt
```

You can also launch Sublime Text from the Ubuntu application menu.

## 7. Update Sublime Text

Since Sublime Text is installed through APT, update it using:

```bash
sudo apt-get update
sudo apt-get install --only-upgrade sublime-text
```

To upgrade all installed packages:

```bash
sudo apt-get upgrade
```

## 8. Uninstall Sublime Text

Remove the application:

```bash
sudo apt-get remove sublime-text
```

To remove the application and its system-wide package configuration:

```bash
sudo apt-get purge sublime-text
```

Optionally, remove the repository configuration and signing key:

```bash
sudo rm /etc/apt/sources.list.d/sublime-text.sources
sudo rm /etc/apt/keyrings/sublimehq-pub.asc
sudo apt-get update
```

> **Warning:** Removing the repository and signing key also removes the APT configuration used for future Sublime Text updates.

## Useful Commands

| Task | Command |
|---|---|
| Check version | `subl --version` |
| Launch editor | `subl` |
| Open current directory | `subl .` |
| Update package lists | `sudo apt-get update` |
| Upgrade Sublime Text | `sudo apt-get install --only-upgrade sublime-text` |

## Official Resources

- [Sublime Text Website](https://www.sublimetext.com/)
- [Official Linux Installation Instructions](https://www.sublimetext.com/docs/linux_repositories.html)
- [Sublime Text Documentation](https://www.sublimetext.com/docs/)

---

**Setup status:** Documented for the Sublime Text Stable installation workflow on Ubuntu.

**Maintained by:** [shajidul-sobuj](https://github.com/shajidul-sobuj)
