
# Legacy Launcher and Yay Installation Guide on Arch Linux

This guide provides step-by-step instructions for installing **Yay** (AUR helper) and **Legacy Launcher** (Minecraft launcher) on the Arch Linux operating system.

---

## Table of Contents

1. [Requirements](#requirements)
2. [What is Yay?](#what-is-yay)
3. [Yay Installation](#yay-installation)
4. [Legacy Launcher Installation](#legacy-launcher-installation)
5. [Launching Legacy Launcher](#launching-legacy-launcher)
6. [Troubleshooting](#troubleshooting)

---

## Requirements

Before starting the installation, make sure you have the following:

- ✅ Arch Linux installed system
- ✅ Internet connection
- ✅ User with `sudo` privileges
- ✅ Basic packages: `base-devel`, `git`

### Checking Basic Packages

```bash
# Check if base-devel group is installed
pacman -Q base-devel

# Check if git is installed
pacman -Q git
```

If not installed:

```bash
sudo pacman -S --needed base-devel git
```

---

## What is Yay?

**Yay** (Yet Another Yogurt) is an AUR (Arch User Repository) helper for Arch Linux. AUR is a repository where user-submitted packages are stored. You can install programs from AUR that are not available in the official Arch repositories.

---

## Yay Installation

### Step 1: Create a Temporary Directory

```bash
# Create a temporary working directory in /tmp
cd /tmp
mkdir -p yay-build
cd yay-build
```

### Step 2: Download Yay Source Code

```bash
# Clone from Yay's GitHub repo
git clone https://aur.archlinux.org/yay.git
```

### Step 3: Navigate to the Package Directory

```bash
# Enter the yay directory
cd yay
```

### Step 4: Build and Install the Package

```bash
# Build and install the package
makepkg -si
```

> **Note:** The `makepkg -si` command:
> - `-s` - Automatically installs dependencies
> - `-i` - Installs the package to the system

### Step 5: Verify the Installation

```bash
# Check Yay version
yay --version
```

If version information appears, you have successfully installed it!

### Complete Script (Automatic Installation)

```bash
#!/bin/bash
set -e

echo "=== Yay Installation Starting ==="

# Temporary directory
cd /tmp
rm -rf yay-build 2>/dev/null || true
mkdir -p yay-build
cd yay-build

# Download source
echo "[1/3] Downloading Yay source code..."
git clone https://aur.archlinux.org/yay.git

# Build package
echo "[2/3] Building package..."
cd yay
makepkg -si --noconfirm

# Cleanup
echo "[3/3] Cleaning up..."
cd /
rm -rf /tmp/yay-build

echo "=== Yay Successfully Installed ==="
yay --version
```

---

## Legacy Launcher Installation

### Method 1: Install with Yay (Recommended)

After installing Yay, you can easily install Legacy Launcher:

```bash
# Search in AUR
yay -Ss legacylauncher
```

```bash
# Install Legacy Launcher
yay -S legacylauncher
```

> **Note:** You may need to answer questions during installation. Usually, default answers (pressing Enter) are sufficient.

### Method 2: Manual Installation

If you don't want to use Yay:

```bash
# Create a temporary directory
cd /tmp
mkdir -p legacy-build
cd legacy-build

# Download the source
git clone https://aur.archlinux.org/legacylauncher.git

# Navigate to the package directory
cd legacylauncher

# Build and install the package
makepkg -si
```

### Verify the Installation

```bash
# Check if Legacy Launcher is installed
which legacylauncher

# Or
pacman -Q legacylauncher
```

---

## Launching Legacy Launcher

### From the Graphical Interface

1. Open the application menu
2. Search for "Legacy Launcher" or "Minecraft"
3. Click on the icon

### From Terminal

```bash
# Launch Legacy Launcher
legacylauncher
```

### Check Java Version

Legacy Launcher requires Java. Check if Java is installed:

```bash
# Check Java version
java -version
```

If not installed:

```bash
# Install OpenJDK (recommended)
sudo pacman -S jdk-openjdk

# Or JRE variant
sudo pacman -S jre-openjdk
```

---

## Troubleshooting

### Problem 1: "command not found" error

**Cause:** The package is not installed correctly or is not in the PATH.

**Solution:**

```bash
# Check if the package is installed
pacman -Q | grep -i legacy

# If not, reinstall
yay -S legacylauncher
```

### Problem 2: "permission denied" error

**Cause:** Permission issue.

**Solution:**

```bash
# Fix permissions
sudo chmod +x /usr/bin/legacylauncher
```

### Problem 3: Java errors

**Cause:** Java is not installed or the version is incorrect.

**Solution:**

```bash
# Check Java version
java -version

# Install the latest OpenJDK
sudo pacman -S jdk-openjdk

# Alternatively, Java 8 (for some older versions)
sudo pacman -S jdk8-openjdk
```

### Problem 4: AUR installation failure

**Cause:** Dependencies are missing.

**Solution:**

```bash
# Update the system
sudo pacman -Syu

# Install dependencies manually
sudo pacman -S --needed base-devel git

# Try reinstalling Yay
cd /tmp
rm -rf yay-build
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

### Problem 5: "unable to lock database" error

**Cause:** Another pacman process is running.

**Solution:**

```bash
# Delete the lock file (carefully!)
sudo rm /var/lib/pacman/db.lck
```

---

## Useful Commands

### Yay Commands

```bash
# Search for a package in AUR
yay -Ss <package-name>

# Install a package
yay -S <package-name>

# Remove a package
yay -R <package-name>

# Update the system (official + AUR)
yay -Syu

# Update only AUR packages
yay -Sua

# List installed AUR packages
yay -Qm
```

### Legacy Launcher Commands

```bash
# Launch Legacy Launcher
legacylauncher

# Open configuration directory
~/.minecraft

# Check log files
~/.minecraft/logs/latest.log
```

---

## Frequently Asked Questions

### Question 1: What is Legacy Launcher?

**Answer:** Legacy Launcher is a third-party launcher used to launch the Minecraft game. It offers more features as an alternative to the official launcher.

### Question 2: Is Yay safe?

**Answer:** Yay itself is safe, but be careful with packages installed from AUR. It is recommended to check PKGBUILD files.

### Question 3: I want to remove Legacy Launcher

**Answer:**

```bash
# Remove Legacy Launcher
sudo pacman -R legacylauncher

# If you also want to remove configuration files
rm -rf ~/.minecraft
```

### Question 4: I want to remove Yay

**Answer:**

```bash
# Remove Yay
sudo pacman -R yay

# Clear cache
rm -rf ~/.cache/yay
```

---

## Contact and Support

- **Arch Wiki:** www.youtube.com/@Doeyw
- **AUR:** https://aur.archlinux.org/
- **Yay GitHub:** https://github.com/Jguer/yay
- **Legacy Launcher:** https://www.legacylauncher.com/

---

## License

This guide was prepared for educational purposes. Please comply with the license terms of the software you use.

---

**Last Updated:** 2026-03-08

**Author:** Doeyw
