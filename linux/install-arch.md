# Installing Arch Linux

1. follow Arch wiki install guide (this video is what I used: https://www.youtube.com/watch?v=68z11VAYMS8)
    - use `cfdisk` for disk partitioning
    - `IMPORTANT`: use UUID when creating `/etc/fstab` instead of the partition names, so when adding drives in the future, drive names don't get swapped. otherwise, a boot partition might not be found on the drive it's looking at 
2. `systemctl enable NetworkManager`
3. install basic things post-install and post-login
```
sudo pacman -S git kitty firefox
```
```
sudo pacman -S 
```
4. install `yay` and change ownership to user (it's owned by root by default)
```
cd /opt
sudo git clone https://aur.archlinux.org/yay-git.git
sudo chown -R derek:derek yay-git/
cd yay-git/
makepkg -si
```
then update
```
yay -Syu
```
5. install display server and WM and stuff (hyprland here)
```
sudo pacman -S hyprland
sudo pacman -S uwsm
```
open `~/.bash_profile` to set hyprland to start on boot. add this block:
```
if uwsm check may-start; then
    exec uwsm start hyprland.desktop
fi
```
install some things needed for hyprland
```
sudo pacman -S xdg-desktop-portal-hyprland hyprpolkitagent
```
reboot, it should load into hyprland after login. also get some more stuff
```
reboot
sudo pacman -S hyprpaper dunst wofi nemo waybar wl-clipboard
```
audio stuff (do i need this?)
```
sudo pacman -S pipewire-pulse pipewire-alsa jack2 
```

## get Derek's dotfiles
remember to switch to `arch` branch
```
cd
git clone https://github.com/Dechrissen/dotfiles.git
cd dotfiles
git checkout arch
```
run install script
```
chmod +x dotfiles/scripts/setup.sh
/dotfiles/scripts/setup.sh
```

## packages to install
```
fastfetch
keepassxc
rclone
python-pip
man-db
signal-desktop
*neovim (skip if you want ot use bob, package manager for nvim instead)
cifs-utils
grep
htop
wget
imv (image viewer)
unzip
ripgrep (?)
gcc
make
fd
```
## Fonts
```
otf-font-awesome
ttf-jetbrains-mono-nerd
ttf-roboto-mono-nerd
ttf-0xproto-nerd
```

## Setting up Git SSH key
generate a key for the machine
```
ssh-keygen -t ed25519 -C "you@email.com"
```
start the ssh agent in the background and add ssh private key to the agent, then `ssh-add -l` to make sure it was added
```
eval `ssh-agent -s`
ssh-add ~/.ssh/id_ed25519
ssh-add -l
```
then add the public key to your GitHub account in settings

## Screenshots
```
grim
slurp
swappy (or wl-clipboard if you don't want editing tools for the screenshot)
```

## neovim
```
bob (package manager for nvim to get nightly builds -- needed for vim.pack until it's in stable)
```

## node
```
sudo pacman -S nvm
```
add `source /usr/share/nvm/init-nvm.sh` to `.bashrc`
then...
```
nvm install node
nvm use node
```

install some node things
```
npm install -g nodemon
npm install -g @anthropic-ai/claude-code
```

### Claude Code (claude-code node CLI app)
to run Claude, run `claude` and authenticate. To use interactively, run `claude` in whatever project directory/repo and prompt it there. Can also do one-shot prompts with `claude -p "your prompt"`

## `lm_sensors` and CoolerControl for hardware monitoring
lm_sensors docs: https://wiki.archlinux.org/title/Lm_sensors
CoolerControl docs: https://gitlab.com/coolercontrol/coolercontrol

```
sudo pacman -S lm_sensors
sudo sensors-detect --auto
yay -S coolercontrol
```

`sudo pacman -S --asdeps qt6-wayland` (for Wayland)

then enable the service
```
sudo systemctl enable --now coolercontrold
```
and run CoolerControl from wofi or something

### Dropbox
```
dropbox
python-gpgme
libappindicator
dropbox-cli
```
- install packages above
- run `dropbox-cli start` and it should prompt you to connect your account/login in browser
- `dropbox-cli status` should print a confirmation that it's running
- then `dropbox-cli autostart y` should start it on boot
- a folder `~/Dropbox` should be created automatically
- ensure `./config/autostart/dropbox.desktop` was created as well

### Trezor Suite
Use the AppImage. udev rules are already installed by default on Arch.
- go to https://trezor.io/guides/trezor-suite/installing-trezor-suite-on-linux
- scroll down to Arch Linux to find this link: https://trezor.io/trezor-suite
- click "More" by the downloads button and select the option for Linux
- save the AppImage file, `cd` to its location, run `chmod a+x` on it, then you can simply run it with `./` + filename

### Yubico / Yubikey
No solution yet, their AppImage and their source code option don't seem to work. Just use Windows or Mac for now. This repo might be an option (CLI tool) but udev rules might need to be added: https://github.com/Yubico/yubikey-manager

Here is their direct article for Linux but nothing works so far: https://support.yubico.com/s/article/Installing-Yubico-Software-on-Linux

### Rufus
for flash drive management / fixing broken flash drives
```
yay -S rufus-linux
```

## Godot
Wine needed for Windows builds?
```
sudo pacman -S godot wine
```

### Steam
1. `sudo nano /etc/pacman.conf` and uncomment the 2 lines for "[multilib]" and "Inlcude..." at the bottom so you use it for 32bit packages
2. `sudo pacman -Syu`
3. `sudo pacman -S steam`
    - when installing, it will prompt you for vulkan drivers, make sure to use the right one for your GPU, e.g. `vulkan-radeon` for AMD
    - refer to https://wiki.archlinux.org/title/Graphics_processing_unit#Installation

### Wiping and partitioning and mounting a disk
> !IMPORTANT
> make sure to use UUID identifiers in `/etc/fstab` instead of partition names (like `/dev/nvme0n1p1`) otherwise you can have issues on boot where the drive names get reordered, and your second drive name becomes the original drive, the `/boot/efi` doesn't exist
1. find the right disk with `lsblk -o NAME,SIZE,TYPE,MOUNTPOINT`
2. partition the disk with `cfdisk /dev/sdX` (or `nvmeXn1`)
    - delete any partitions and create a new full-size partition and write/quit
3. format the partition (not the whole disk) with `mkfs.ext4 /dev/sdX1` (or `nvmeXn1`)
    - it might say old stuff is on there such as "DOS/MBR boot sector", just proceed anyway
#### mounting the new partition
1. for a drive for Steam games for instance, create mount point with `sudo mkdir /mnt/games`
2. then mount the partition there with `sudo mount /dev/sdX1 /mnt/games` (or `nvmeXn1`)
3. set ownership `sudo chown -R $USER:$USER /mnt/games` (or replace with your actual username)

Set it to auto-mount on boot using UUID:
1. get UUID with `lsblk -f` (look for the new partition and get the UUID)
2. add it to `/etc/fstab`:
```
UUID=your-uuid-here  /mnt/games  ext4  defaults,noatime  0  2
```
3. `sudo systemctl daemon-reload` and then mount everything in fstab again: `sudo mount -a`
