[Français](readme.md)

# Base Config

This project aims to provide a modular base for creating environment installation and configuration scripts on Linux (specifically Red Hat-based distributions). It automates the installation of repositories, packages, Flatpak applications, and system configuration via a menu interface or simple configuration files.

## Prerequisites

*   **OS**: Fedora, Red Hat, and CentOS (Only for repositories)
*   **Rights**: The main script must be run as **root** (administrator).

## Usage

1.  Make the main script executable (if not already done):
    ```bash
    chmod +x script.sh
    ```
2.  Run the script with root privileges:
    ```bash
    sudo ./script.sh
    ```
3.  An interactive menu will appear, allowing you to choose actions to perform (repo installation, applications, system config, etc.).

## File Configuration

The script relies on several files present in the same directory to know what to install or configure. Here is how to write each of them.

### 1. `install_repos.sh`
This file is a **Bash script** executed to install additional repositories (like RPM Fusion).

*   **Format**: Standard shell script.
*   **Example**:
    ```bash
    #!/bin/bash
    echo "Installing RPM Fusion repositories..."
    dnf install -y https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
    dnf install -y https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
    ```

### 2. `app.txt` & `remove.txt`
These files contain the list of DNF packages to **install** (`app.txt`) or **remove** (`remove.txt`).

*   **Format**: A list of package names, separated by spaces or newlines.
*   **Example for `app.txt`**:
    ```text
    git vim htop
    curl wget
    vlc firefox
    ```

### 3. `flatpack.txt`
This file contains the list of **Flatpak** applications to install.

*   **Format**: A list of Flatpak application identifiers (Application ID), separated by spaces or newlines.
*   **Note**: Flatpak must be installed on the system for this to work.
*   **Example**:
    ```text
    org.gimp.GIMP
    com.spotify.Client
    org.videolan.VLC
    ```

### 4. `configure_system.sh`
This file is a **Bash script** executed at the end to apply specific system configurations (firewall, services, users, etc.).

*   **Format**: Standard shell script.
*   **Example**:
    ```bash
    #!/bin/bash
    echo "Enabling Firewall..."
    systemctl enable --now firewalld
    
    echo "Configuring Git..."
    git config --system user.name "MyName"
    
    echo "Cleaning up..."
    dnf clean all
    ```
