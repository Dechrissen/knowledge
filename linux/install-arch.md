# Installing Arch Linux

1. follow Arch wiki install guide
    - use `cfdisk` for disk partitioning
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
npm install -g nodemon
```

### Dropbox
```
dropbox
python-gpgme
libappindicator
dropbox-cli
```
- install packages above
- run `dropbox-cli status` and it should prompt you to connect your account/login
- then `dropbox-cli autostart` should start it on boot
- a folder `~/Dropbox` should be created automatically
- ensure `./config/autostart/dropbox.desktop` was created as well