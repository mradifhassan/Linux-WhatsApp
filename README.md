<p align="center">
  <img src="whatsapp.svg" alt="WhatsApp">
</p>

# How to install native WhatsApp software alternative desktop client in Ubuntu Linux

Unlike Windows and macOS, WhatsApp does not provide a native desktop client for Linux systems. So we use WhatsApp in our browser. But there is an alternative using WhatsApp from your device without using browser. To do that so, we need to get introduced with an open source desktop client named `zapzap` which is built specifically for Linux users.
## Features
* Provides a native-like experience by wrapping the web version in a system-integrated application.
* Adding features like multi-account support.
* Native system notifications.
* Dark/Light mode theming.

## Table of Contents
1. [Prerequisites & Repository Fix](#1-prerequisites--repository-fix)
2. [Installing ZapZap](#2-installing-zapzap)
3. [Customizing the Application Icon](#3-customizing-the-application-icon)
4. [Validating and Applying Changes](#4-validating-and-applying-changes)

## 1. Prerequisites & Repository Fix

### Step 1.1: Enable the Ubuntu Universe Repository
Ensure that the Ubuntu `universe` repository is available and system package lists are up to date:
```bash
sudo add-apt-repository universe
sudo apt update
```

### Step 1.2: Fix Flatpak GPG Verification Error

If you encounter a `GPG verification enabled, but no summary found` error while attempting to interact with Flathub, the remote configuration needs to be re-added cleanly to fetch the correct cryptographic keyrings.
```bash
# Install Flatpak
sudo apt install flatpak

# Install GNOME Software Flatpak plugin
sudo apt install gnome-software-plugin-flatpak

# Delete the corrupted or misconfigured flathub remote
sudo flatpak remote-delete flathub

# Re-add flathub cleanly
sudo flatpak remote-add --if-not-exists flathub [https://dl.flathub.org/repo/flathub.flatpakrepo](https://dl.flathub.org/repo/flathub.flatpakrepo)

# Refresh Flatpak metadata
flatpak update
```

## 2. Installing ZapZap
With the Flathub repository successfully fixed, go to the official site of zapzap (https://rtosta.com/zapzap/) and then install flathub and install the ZapZap application package and its dependencies (such as the KDE Platform runtime):
```bash
sudo flatpak install flathub com.rtosta.zapzap
```
Type `y` when prompted to confirm the installation of required runtimes and permissions.
## 3. Customizing the Application Icon
Flatpak applications maintain their configurations in global directories that get overwritten during software updates. To ensure customizations persist, create a local, user-specific desktop override.

### Step 3.1: Copy the Desktop Entry
Copy the global application shortcut launcher to your local user applications directory:
```bash
cp /var/lib/flatpak/exports/share/applications/com.rtosta.zapzap.desktop ~/.local/share/applications/
```
### Step 3.2: Configure the Icon Asset
Ensure your custom icon asset (e.g., `whatsapp.svg`) is safely placed in your local applications folder (If you don't find any suitable format of `.svg`, then download the given `.svg`):

```bash
cd ~/.local/share/applications/
# Verify your icon is an actual scalable graphic image asset
file whatsapp.svg
```
The ultimate directory path and structure would be something like this way- 
```bash
/home/username/
├── ~/.local/share/applications/
|   └── com.rtosta.zapzap.desktop
|   └── whatsapp.svg
```
### Step 3.3: Edit the Desktop Entry Paths
Open the copied launcher file using a text editor:
```bash
nano com.rtosta.zapzap.desktop
```
Modify the entry according to the FreeDesktop Icon Theme Specification. Since we are targeting an asset inside a non-standard icon directory, an absolute path must be supplied instead of just a raw filename with an extension:
```bash
[Desktop Entry]
Version=1.0
Name=WhatsApp
Comment[pt_BR]=WhatsApp Desktop para Linux
Comment=WhatsApp Desktop for Linux
Exec=/usr/bin/flatpak run --branch=stable --arch=x86_64 --command=zapzap --file-forwarding com.rtosta.zapzap @@u %u @@
Icon=whatsapp.svg
Type=Application
Categories=Chat;Network;InstantMessaging;Qt;
Keywords=WhatsApp;Chat;ZapZap;
StartupWMClass=zapzap
MimeType=x-scheme-handler/whatsapp
Terminal=false
SingleMainWindow=true
X-GNOME-UsesNotifications=true
X-GNOME-SingleWindow=true
X-Flatpak=com.rtosta.zapzap

```
Save and exit the text editor (in `nano`, press `Ctrl+O`,`Enter`, then `Ctrl+X`. For `vim`, `esc` and then `:wq`. For `gnome-text-editor`, `Ctrl+S` and then `Ctrl+Q`).

### Step 3.4: Set Executable Permissions
Give the custom local launcher permission to execute properly:
```bash
chmod +x com.rtosta.zapzap.desktop
```
## 4. Validating and Applying Changes
To ensure the file conforms to FreeDesktop standards and to force the desktop environment (GNOME) to pick up the changes immediately, validate the layout and rebuild the local menu database:
```bash
# Validate that no formatting errors remain
desktop-file-validate ~/.local/share/applications/com.rtosta.zapzap.desktop

# Update the local desktop application registry
update-desktop-database ~/.local/share/applications/
```

Give the desktop file proper execution permissions by running:
```bash
chmod +x ~/.local/share/applications/com.rtosta.zapzap.desktop
```
