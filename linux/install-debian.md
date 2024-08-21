# Installing Debian from scratch

1. do graphical install, and at "Software selection" menu, select only 'SSH server' and 'standard system utilities'
2. on first boot, log in, then you need to set up sudo and add main user to the sudoers file
```
su
apt install sudo
sudo usermod -aG sudo [username]
sudo visudo
```

add `[username] ALL=(ALL:ALL) ALL` under 'User privelege specification', save, exit

```
exit
```

now you should be back to main user prompt.

3. install basic things

```
sudo apt install i3 i3blocks lightdm x11-xserver-utils pulseaudio nm-tray
sudo apt install curl git gnupg feh vim
sudo apt install chromium xfe xfe-themes neofetch
```

4. reboot

```
sudo reboot
```

5. set up i3 / i3blocks
    - reload i3 config with mod+Leftshift+R
    - log out with mod+E

```
mkdir .config/i3blocks
mkdir .config/i3blocks/modules
cd .config/i3blocks
touch config
```


### etc
- install font awesome package for bar icons
```
sudo apt install fonts-font-awesome
```
