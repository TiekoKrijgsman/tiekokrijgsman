# Package Management

Software for GNU/Linux systems is distributed in the form of packages. A package is an archive with software files, configuration files, and a list of required dependencies - additional packages required for the software to run.

Each GNU/Linux distribution has its own package management system.

## Debian

APT (Advanced Package Tool) is the default package manager for Debian and its derivatives (Ubuntu, Linux Mint, etc.), and is based on the `dpkg` package manager.

```sh
# Update the list of available packages
apt update

# Upgrade all installed packages
apt upgrade

# Install a package
apt install <package>

# Remove a package and its configuration files
apt purge <package>
```

## Snap

Snap is a package management system developed by Canonical.

The packages, called snaps, and the tool for using them, snapd, work across a range of GNU/Linux distributions and allow upstream software developers to distribute their applications directly to users

```sh
# Upgrade all installed packages
snap refresh

# Install a package
snap install <package>

# Remove a package
snap remove <package>
```

## Flatpak

Flatpak is a package management system developed by Red Hat, Flatpak apps can be installed on any existing and future Linux distribution.

```sh
# Install a package
flatpak install <package>

# Remove a package
flatpak remove <package>
```

## Arch Linux

Pacman is the default package manager for Arch Linux.

```sh
# Upgrade all installed packages
pacman --refresh --sync --sysupgrade

# Install a package
pacman --sync --needed <package>

# Remove a package
pacman --remove <package>
```
