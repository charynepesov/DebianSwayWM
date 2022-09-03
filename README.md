# My SwayWM installation on Debian testing
#### Installing Debian Testing ans SwayWM
> There is great video about [installing debian with btrfs file structure](https://youtu.be/_i_InuWyfQE)
The essential commands on video. In order to enter to busybox Ctrl+Alt+F2
```bash
$ df -h
$ umount /target/boot/efi
$ umount /target
$ mount /dev/sda2 /mnt
$ cd /mnt
$ ls
$ mv @rootfs @
$ btrfs su cr @home
$ mount -o noatime,space_cache=v2,compress=zstd,ssd,discard=async,subvol=@ /dev/sda2 /target
$ mkdir -p /target/boot/efi
$ mkdir -p /target/home
$ mount -o noatime,space_cache=v2,compress=zstd,ssd,discard=async,subvol=@home /dev/sda2 /target/home
$ mount /dev/sda1 /target/boot/efi
$ cd /target/etc/
$ nano fstab
```
And change defaults to this lines.
```
/     noatime,space_cache=v2,compress=zstd,ssd,discard=async,subvol=@
/home noatime,space_cache=v2,compress=zstd,ssd,discard=async,subvol=@home
```
In order to go out of busybox Ctrl+Alt+F1

#### Login with root user. After install connect to the internet.
I am connecting with my phone. [NetworkConfiguration](https://wiki.debian.org/NetworkConfiguration),
[WiFi](https://wiki.debian.org/WiFi), [NetworkManager](https://wiki.debian.org/NetworkManager)
```bash
$ ifconfig -a
$ ifconfig usb0 up && dhclient usb0
```
#### Then give [sudo](https://wiki.debian.org/sudo/) for normal user if you didn't do that.
```bash
$ apt install sudo
$ adduser username sudo
$ exit
```
#### Login with your username
And first install nala package manager.
```bash
$ sudo apt install nala
```
#### Install these essential packages for running swaywm. 
```bash
$ sudo nala install \
$	firmware-iwlwifi \
$ 	network-manager \
$ 	firefox \
$	kitty \
$	sway
```
#### Configure sway.
```bash
$ mkdir ~/.config/sway
$ cp /etc/sway/config ~/.config/sway/config
```
Configure keyboard.
```bash
$ swaymsg -t get_inputs
```
Add this lines to your config file
```bash
$ echo "input - xkb_layout 'us,tr,ru' \
	input - xkb_options 'grp:win_space_toggle'" >> ~/.config/sway/config
```
Install other sway packages.
```bash
$ sudo nala install \
$	sway-backgrounds \
$	swayidle \
$	swaylock \
$	xdg-desktop-portal-wlr
```
Configure Wayland
```bash
$ sudo nala install \
$	qtwayland5 \
$	libreoffice-gtk3
```
Add environment variables in order to apps to work on wayland.
```bash
$ echo "GDK_BACKEND=wayland		#for GTK apps \n
$ CLUTTER_BACKEND=wayland		#for clutter apps \n
$ QT_QPA_PLATFORM=wayland;xcb	#for QT apps \n
$ SDL_VIDEODRIVER=wayland		#for SDL2 apps \n
$ MOZ_ENABLE_WAYLAND=1			#for Firefox \n
$ SAL_USE_VCLPLUGIN=gtk3		#for libreoffice \n
$ _JAVA_AWT_WM_NONREPARENTING=1	#for JAVA apps \n
$ ELM_DISPLAY=wl				#for EFL apps" >> .profile
```
For Chromium run program with
```
--ozone-platform=wayland
```
Also if you want to run some apps on xwayland install this.
```bash
$ sudo nala install xwayland
```
#### Install pipewire for audio.
pulseaudio-utils - installed for controlling volume keys.

I am not yet figured out how to configure it with native pipwire tools.
```bash
$ sudo nala install wireplumber \
$	pipewire-audio-client-libraries \
$	pipewire-pulse \
$	libspa-0.2-jack \
$	pulseaudio-utils
$
$ systemctl --user --now enable wireplumber.service
$ systemctl --user --now enable pipewire-pulse.service
$ sudo reboot
```
After reboot see what does show pactl.
```bash
$ LANG=C pactl info | grep '^Server Name'
$ sudo cp /usr/share/doc/pipewire/examples/alsa.conf.d/99-pipewire-default.conf /etc/alsa/conf.d/
$ sudo cp cp /usr/share/doc/pipewire/examples/ld.so.conf.d/pipewire-jack-*.conf /etc/ld.so.conf.d/
$ sudo ldconfig
```
#### install and configure bluetooth(not yet tried myself)
As a pairing software install one of these
- gnome-bluetooth (for GNOME) 
- bluedevil (for KDE)
- blueman (Gtk2) 
[Bluetooth](https://wiki.debian.org/BluetoothUser),
[Audio on bluetooth](https://wiki.debian.org/BluetoothUser/a2dp)
```bash
$ sudo nala install \
$	libspa-0.2-bluetooth \
$	bluetooth \
$	blueman
$ service bluetoth start
```

```bash
$ sudo nala install \
$	firmware-linux \
$	intel-microcode \
$	brightnessctl \
$	tlp \
$	pcmanfm \
$	ntfs-3g \
$	btrfs-progs \
$	audacity \
$	geany \
$	wofi \
$	waybar \
$	vim \
$	git 
```
Install fonts
```bash
$ sudo nala install \
$	ttf-mscorefonts-installer \
$	fonts-crosextra-carlito \
$	fonts-crosextra-caladea \
$	fonts-font-awesome \
$	fonts-jetbrains-mono \
```
Install some codecs
```bash
$ sudo nala install \
$	libavcodec-extra
```

