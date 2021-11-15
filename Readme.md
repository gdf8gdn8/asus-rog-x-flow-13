#  Asus Rog X Flow 13


> With small tweaks almost everything (surprisingly touch, pen input and camera) works fine.
> Notable exceptions is fingerprint  
source: https://github.com/CO-1/asus-flow-x13-linux

Currently missing: support for fingerprint sensor, fan-control

## grub
on archlinux install 
edit boot config and append to cmdline
> rd.driver.blacklist=nouveau

edit /etc/default/grub
```config
GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 quiet rd.driver.blacklist=nouveau nvidia-drm.modeset=0"
GRUB_CMDLINE_LINUX="rd.driver.blacklist=nouveau nvidia-drm.modeset=0"
```

##  install regquired packages

install packages from https://asus-linux.org/
1. base devel requirements 
> base-devel curl git
 ``` 
 git clone https://aur.archlinux.org/yay
 makepkg -si
 ```

2. xorg
>xf86-input-synaptics amdvk xf86-video-amd xorg xdg-utils xdg-user-dirs primus_vk lib32-nvidia-utils lib32-primus_vk  nvidia-dkms nvidia-prime  

copy x11 config und xorg.conf.d
```
sudo cp -r xorg.conf.d/* /etc/X11/xorg.conf.d/
sudo cp -r hwdb.d /etc/udev/
```

3. power and sensors
tlp acpid lm_sensor amdfand-bin xsensors
```
systemctl enable --now acpid
systemctl enable --now sensors
systemctl enable --now tlp
systemctl enable --now amdfand
```

4. Graphics switching
supergfxctl - https://gitlab.com/asus-linux/supergfxctl.git

```sh
cd supergfxctl
makepkg -si
systemctl enable --now supergfxd
```

3. utility for Linux to control many aspects of various ASUS laptop
```sh
yay -s asusctl-git
susystemctl enable asusd
```

4. git clone https://github.com/CO-1/asus-flow-x13-linux.git
```
cd asus-flow-x13-linux
sudo sh ./install.sh
```


### Nvidia
source: https://github.com/CO-1/asus-flow-x13-linux   
Nouveau drivers hangs laptop at boot. It can be blacklisted by appending
`nouveau.blacklist=1` to kernel command line or by following commands as root
```sh
echo "blacklist nouveau" >  /etc/modprobe.d/asus-flow-x13-nouveau.conf
echo "alias nouveau off" >> /etc/modprobe.d/asus-flow-x13-nouveau.conf
```



