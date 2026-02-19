<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/img/dotfiles_logo.png">
    <source media="(prefers-color-scheme: light)" srcset="assets/img/dotfiles_logo.png">
    <img alt="Dotfiles logo" src="assets/img/dotfiles_logo.png" width="50%">
  </picture>
</div>

<h3 align="center">
  Ubuntu Development Setup for daily use.
  <br/>
</h3>

---

## Features

- **OS**: Ubuntu 25.10
- **Shell**: zsh + starship prompt
- **Editors**: Zed / VS Code + extensions
- **DE**: Gnome 49
- **Terminal**: Ptyxis
- **Theme**: Yaru dark purple
- **Optional**: SNAPS FREE!
- **Tech Stack**: Python, Golang & Typescript
- **My cool wallpapers folder! 21:9 and 16:9**

#### ***Note:***
*I’m aware I’m on a non-LTS Ubuntu release right now.
Once 26.04 LTS drops and proves to be stable, I’ll update the dotfiles to match.
I went with Ubuntu for its balance of stability, simplicity, and great gaming support on the same machine.*

---

## First Steps After Installation

#### 1. Update the system and reboot

```bash
sudo apt update && sudo apt upgrade && sudo reboot now
```

#### 2. Curl install

```bash
sudo apt install curl
```

#### 3. Brave browser install and setting up

```bash
curl -fsS https://dl.brave.com/install.sh | sh
```

###### *Note: Now config the browser as default, sync key, etc*

---

## Optional: Clean snaps and disable it

#### Optional. Create a timeshift backup

#### 1. Remove base packages from snap and apt

```bash
sudo snap remove --purge firefox && sudo apt remove *firefox* -y && sudo apt remove *libreoffice* -y && sudo apt remove *remmina* -y && sudo apt remove *transmission* -y && sudo snap remove --purge thunderbird && sudo apt remove *thunderbird* -y
```

#### 2. Remove all extra packages:

```bash
sudo snap remove gtk-common-themes && sudo snap remove gnome-42-2204 && sudo snap remove snapd-desktop-integration && sudo snap remove desktop-security-center && sudo snap remove prompting-client && sudo snap remove snap-store && sudo snap remove firmware-updater && sudo snap remove bare && sudo snap remove core22 && sudo snap remove snapd
```

#### 3. Check list and remove if still have packages:

```bash
sudo snap list
```

#### 4. Disable Snaps:

```bash
sudo systemctl stop snapd && sudo systemctl disable snapd && sudo systemctl mask snapd && sudo apt purge snapd -y && sudo apt-mark hold snapd && sudo rm -rf ~/snap && sudo rm -rf /snap && sudo rm -rf /var/snap && sudo rm -rf /var/lib/snapd
```
```bash
sudo nano /etc/apt/preferences.d/nosnap.pref
```
```bash
Package: snapd
Pin: release a=*
Pin-Priority: -10
```

#### 5. Enable Firefox repositories:

```bash
sudo apt update && sudo install -d -m 0755 /etc/apt/keyrings
```
```bash
wget -q https://packages.mozilla.org/apt/repo-signing-key.gpg -O- | sudo tee /etc/apt/keyrings/packages.mozilla.org.asc > /dev/null
```
```bash
echo "deb [signed-by=/etc/apt/keyrings/packages.mozilla.org.asc] https://packages.mozilla.org/apt mozilla main" | sudo tee -a /etc/apt/sources.list.d/mozilla.list > /dev/null
```
```bash
echo '
Package: *
Pin: origin packages.mozilla.org
Pin-Priority: 1000
' | sudo tee /etc/apt/preferences.d/mozilla
```
```bash
sudo add-apt-repository ppa:mozillateam/ppa && sudo gnome-text-editor /etc/apt/preferences.d/mozillateamppa
```
```bash
Package: thunderbird*
Pin: release o=LP-PPA-mozillateam
Pin-Priority: 1000

Package: thunderbird*
Pin: release o=Ubuntu
Pin-Priority: -10
```
```bash
echo 'Unattended-Upgrade::Allowed-Origins:: "LP-PPA-mozillateam:${distro_codename}";' | sudo tee /etc/apt/apt.conf.d/51unattended-upgrades-thunderbird
```

#### 6. Enable flatpak and flathub:
```bash
sudo apt install --install-suggests gnome-software -y
```

- Reboot and run:

```bash
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

###### *Note: this steps are extracted from this links https://kskroyal.com/remove-snap-packages-from-ubuntu/ & https://ubuntuhandbook.org/index.php/2024/03/install-thunderbird-deb-ubuntu-2404/*

---

## Base System Setup & GNOME

#### 1. Install nvidia drivers with Additional Drivers
- Install your drivers with the tool provided by canonical. My drivers actually are 590 from nvidia. You can do it with a command:

```bash
sudo ubuntu-drivers autoinstall
```

- Reboot and enjoy your drivers

#### 2. Config Gnome Settings
- Set up your monitor(s) or multi-monitor layout as needed (via Settings → Displays).
- Disable mouse acceleration for precise control (especially useful for gaming/productivity)
- Tweak other settings

#### 3. Install my apps

```bash
sudo apt install gdebi dconf-editor gnome-tweaks pavucontrol steam vlc ratbagd ufw gufw
```
```bash
sudo systemctl enable --now ratbagd
```

- Install Discord ***.deb*** from their official website using GDEBI https://discord.com/.

- Install LACT ***.deb*** with GDEBI from their releases: https://github.com/ilya-zlobintsev/LACT/releases. After install enable it:
```bash
sudo systemctl enable --now lactd
```

- Now I configure TLP (or auto-cpufreq/lact) with my preferred options. This section can vary depending on your hardware and system. I adapt it to my NVIDIA RTX 3080 setup for optimal power management and performance.

<div align="center">
  <picture>
    <source srcset="assets/img/undervolt.png">
    <img alt="Undervolt settings" src="assets/img/undervolt.png" width="50%">
  </picture>
</div>

- Open gnome software and install manually:

```bash
- Refine
- Gnome Extensions Manager
- Bitwarden
- ProtonPlus
- Protontricks
- AdwSteamGtk
- Zen Browser (My new replace for brave)
- Spotify
- Obsidian
- Piper
- Flatseal
```

- Finally open refine and enable VRR in shell settings.

#### 4. Firewall & Tweaks
```bash
sudo systemctl enable ufw && sudo systemctl start ufw && sudo ufw deny 22/tcp
```
```bash
sudo apt install ubuntu-restricted-extras build-essential checkinstall libfuse2t64 unzip p7zip p7zip-full unrar make curl ca-certificates libssl-dev libxmlsec1-dev libmagic-dev libmagickwand-dev fzf git-lfs starship bleachbit ffmpeg
```
```bash
sudo mkdir -p '/etc/systemd/resolved.conf.d' && sudo -e '/etc/systemd/resolved.conf.d/99-dns-over-tls.conf'
```
```
[Resolve]
DNS=1.1.1.2#security.cloudflare-dns.com 1.0.0.2#security.cloudflare-dns.com 2606:4700:4700::1112#security.cloudflare-dns.com 2606:4700:4700::1002#security.cloudflare-dns.com
DNSOverTLS=yes
Domains=~.
```
```bash
sudo timedatectl set-local-rtc '0'
```
```bash
gsettings set org.gnome.shell.extensions.dash-to-dock click-action 'minimize-or-previews'
```
```bash
sudo apt autoremove && sudo apt autoclean && sudo apt clean
```
```bash
sudo mv /etc/apt/apt.conf.d/20apt-esm-hook.conf /etc/apt/apt.conf.d/20apt-esm-hook.bak
```



#### 5. Optional. Setup my headphones JBL dual channel.
###### I have JBL Quantum 810 Wireless headphones that support two separate audio channels: one for system audio and another for voice chats. By default, they are not configured this way.

- Open pavucontrol, find soundcard headphones option and change its profile to Pro Audio. Now got the dual channel enabled, only need rename it.

```bash
pw-cli ls Node | less
```

- Copy alsa node name for System channel and the Chat

```bash
mkdir -p ~/.config/wireplumber/wireplumber.conf.d
```
```bash
nano ~/.config/wireplumber/wireplumber.conf.d/55-audio-rename.conf
```

- Paste this config into the nano editor:
```json
monitor.alsa.rules = [
  {
    matches = [
      {
        node.name = "alsa_output.usb-Harman_International_Inc_JBL_Quantum810_Wireless-00.pro-output-0"
      }
    ]
    actions = {
      update-props = {
        node.description = "JBL Quantum 810 Wireless Game"
      }
    }
  }
  {
    matches = [
      {
        node.name = "alsa_output.usb-Harman_International_Inc_JBL_Quantum810_Wireless-00.pro-output-1"
      }
    ]
    actions = {
      update-props = {
        node.description = "JBL Quantum 810 Wireless Chat"
      }
    }
  }
]
```

```bash
systemctl --user restart wireplumber.service
```

---

## Development Setup

#### 1. Install ZSH, enable as default and install Oh My Zsh

```bash
sudo apt install zsh -y
```
```bash
chsh -s $(which zsh)
```
```bash
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
- Reboot the system.

#### 2 Install my essential plugin stack.

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```
```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```
```bash
git clone https://github.com/MichaelAquilina/zsh-you-should-use.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/you-should-use
```
```bash
git clone https://github.com/Aloxaf/fzf-tab ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/fzf-tab
```

#### 3. UV, AWS Cli, Golang & Node Installation
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```
```bash
source $HOME/.local/bin/env
```
```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
```
```bash
unzip awscliv2.zip && sudo ./aws/install && rm -rf aws awscliv2.zip
```
```bash
sudo add-apt-repository ppa:longsleep/golang-backports && sudo apt update && sudo apt install golang-go
```
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.4/install.sh | bash
```

#### 4. Install VS Code & Zed
```bash
curl -f https://zed.dev/install.sh | sh
```
```bash
sudo apt-get install wget gpg && wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg && sudo install -D -o root -g root -m 644 microsoft.gpg /usr/share/keyrings/microsoft.gpg && rm -f microsoft.gpg
```
```bash
sudo nano /etc/apt/sources.list.d/vscode.sources
```
```
Types: deb
URIs: https://packages.microsoft.com/repos/code
Suites: stable
Components: main
Architectures: amd64,arm64,armhf
Signed-By: /usr/share/keyrings/microsoft.gpg
```
```bash
sudo apt install apt-transport-https && sudo apt update && sudo apt install code
```

###### ***Note:*** If you need check vscode installation docs, go here: https://code.visualstudio.com/docs/setup/linux

- Create an script called test.sh and copy that inside:
```
#!/bin/bash

extensions=(
    aaron-bond.better-comments
    alefragnani.project-manager
    batisteo.vscode-django
    charliermarsh.ruff
    cweijan.dbclient-jdbc
    cweijan.vscode-mysql-client2
    esbenp.prettier-vscode
    formulahendry.auto-close-tag
    formulahendry.auto-rename-tag
    github.remotehub
    graphql.vscode-graphql
    graphql.vscode-graphql-syntax
    gruntfuggly.todo-tree
    hbenl.vscode-test-explorer
    ibm.output-colorizer
    idleberg.icon-fonts
    kamikillerto.vscode-colorize
    kevinrose.vsc-python-indent
    littlefoxteam.vscode-python-test-adapter
    mhutchie.git-graph
    miguelsolorio.fluent-icons
    miguelsolorio.min-theme
    miguelsolorio.symbols
    mikestead.dotenv
    mkxml.vscode-filesize
    mrmlnc.vscode-autoprefixer
    ms-azuretools.vscode-containers
    ms-azuretools.vscode-docker
    ms-kubernetes-tools.vscode-kubernetes-tools
    ms-python.debugpy
    ms-python.isort
    ms-python.python
    ms-python.vscode-pylance
    ms-python.vscode-python-envs
    ms-vscode-remote.remote-containers
    ms-vscode-remote.remote-ssh
    ms-vscode-remote.remote-ssh-edit
    ms-vscode-remote.remote-wsl
    ms-vscode-remote.vscode-remote-extensionpack
    ms-vscode.azure-repos
    ms-vscode.remote-explorer
    ms-vscode.remote-repositories
    ms-vscode.remote-server
    ms-vscode.test-adapter-converter
    njpwerner.autodocstring
    pranaygp.vscode-css-peek
    redhat.vscode-yaml
    spywhere.guides
    wholroyd.jinja
    yzhang.markdown-all-in-one
    zhuangtongfa.material-theme
)

for ext in "${extensions[@]}"; do
  code --install-extension "$ext"
done
```

#### 5. System Tweaks
```bash
sudo nano /etc/sysctl.conf
```
```
fs.inotify.max_user_watches = 10000000
fs.inotify.max_user_instances = 256
```
```bash
sudo sysctl -p
```
```bash
sudo mkdir -p /etc/systemd/system/user@.service.d/
```
```bash
sudo nano /etc/systemd/system/user@.service.d/delegate.conf
```
```
[Service]
Delegate=yes
```
```bash
sudo systemctl daemon-reload
```
```bash
sudo nano /etc/environment
```
```
GDK_GL=gles
```
```bash
sudo apt install nvidia-cuda-toolkit
```

#### 6. Podman setup
```bash
sudo apt install podman
```
```bash
flatpak install flathub io.podman_desktop.PodmanDesktop
```

- Start Podman Desktop and follow the launch wizard. Install kubernetes & compose. After that create kind container and update it.

```bash
systemctl --user restart podman
```

#### 7. Git, SSH and GPG sign
- We can configure Git in the .gitconfig file that I will leave in the dotfiles. Everything will be done in the last step when we install the dotfiles in our folder.
- For ssh and gpg, Im using noreply mail provided by github.
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
```bash
ssh-add ~/.ssh/id_ed25519
```
```bash
cat ~/.ssh/id_ed25519.pub
```
```bash
ssh -T git@github.com
```
```bash
gpg --full-generate-key
```
- Take RSA and RSA, 4096 bits and fill all the fields in the prompt.

```bash
gpg --list-secret-keys --keyid-format=long
```
- Copy the long form of the GPG key ID you'd like to use. E.g. 8FF7C99772967AA3
```bash
gpg --armor --export 8FF7C99772967AA3
```

- Add the GPG key to your git account.

#### 8. Others settings
- Currently im using Ollama as model provider to run local IA. I prefer over claude or cloud solutions.
```bash
curl -fsSL https://ollama.com/install.sh | sh
```
```bash
curl -fsSL https://opencode.ai/install | bash
```

- Download some nerd fonts to install it https://github.com/ryanoasis/nerd-fonts/releases. After download, extract and copy into the fonts folder one by one.

```bash
cp -r *.ttf ~/.local/share/fonts/
```
```bash
sudo fc-cache -f -v
```

- I've got another HDD to storage some games and development. Simply partition your disk as you like, copy the UUID of each partition, and edit the fstab file by adding a line similar to the following snippet for each partition you want to mount in a folder:
```bash
sudo nano /etc/fstab
```
```bash
UUID=your_UUID /mnt/Games ext4 defaults,user,exec,rw 0 2
```
```bash
sudo mount -a
```

- If you dont use gnome disk, you should probably give permissions to the folder with:
```bash
sudo chown user:user /mnt/Games
```

- Now I use dconf-editor for change system keybinds like Alt+Space, Workspace switcher or move window to X workspace, etc.

- Install ruff formatter from astral team. Blazing fast formatter... Im in love with them, just only use:
```bash
uv tool install ruff@latest
or
curl -LsSf https://astral.sh/ruff/install.sh | sh
```

#### 9. Fix flatpak and gtk3 themes
```bash
mkdir -p ~/.themes
```
```bash
cp -r /usr/share/themes/Yaru-purple* ~/.themes/
```
```bash
flatpak override --user --env=GTK_THEME=Yaru-purple-dark
```
```bash
nano .profile
```
- Paste this at the end.
```bash
export GTK_THEME=Yaru-purple-dark
```
---

## Installing dotfiles
- In this repository, you will find a folder called dotfiles that contains all the necessary files organized as I have them on my system. For zed, I will include the extension folders since there is no exact way to install extensions via terminal as in vscode.

- The last step to configure the system as I have it is to copy the contents of this dotfiles folder and paste it into your user directory.

- If you cannot see the folders inside this folder, it is because they are hidden by default in the browser. If you enable this option, you should be able to see everything.

- Remember, dont forget change username and mail from settings like .gitconfig.


## Contributing

These are my settings and preferences. I am making the repository public so that everyone feels free to use it, modify it, send me PRs, etc. I will also leave the file as a sketch that I created while running commands and researching.

## Usefull Links
- https://github.com/erik1066/fedora-setup-guide
- https://github.com/devangshekhawat/Fedora-43-Post-Install-Guide

GRUB_TIMEOUT=10
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=saved
GRUB_SAVEDEFAULT=true
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL_OUTPUT="gfxterm"
GRUB_GFXPAYLOAD_LINUX=keep
GRUB_CMDLINE_LINUX="rhgb quiet rd.driver.blacklist=nouveau,nova_core modprobe.blacklist=nouveau,nova_core"
GRUB_DISABLE_RECOVERY="true"
GRUB_ENABLE_BLSCFG=true
GRUB_THEME="/boot/grub2/themes/fedora/theme.txt"

