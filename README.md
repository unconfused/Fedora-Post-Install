# Fedora-Post-Install 
Stuff I install after installing Fedora 39 Workstation

## Based on:
https://github.com/devangshekhawat/Fedora-39-Post-Install-Guide


sudo nano /etc/dnf/dnf.conf

[main]
gpgcheck=1
installonly_limit=3
clean_requirements_on_remove=True
best=False
skip_if_unavailable=True
fastestmirror=1
max_parallel_downloads=10
deltarpm=true


RPM Fusion

sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm


Update OS

sudo dnf -y upgrade --refresh


Update Firmware

sudo fwupdmgr get-devices
sudo fwupdmgr refresh --force
sudo fwupdmgr get-updates
sudo fwupdmgr update


Media Codecs

sudo dnf groupupdate 'core' 'multimedia' 'sound-and-video' --setop='install_weak_deps=False' --exclude='PackageKit-gstreamer-plugin' --allowerasing && sync
sudo dnf swap 'ffmpeg-free' 'ffmpeg' --allowerasing
sudo dnf install gstreamer1-plugins-{bad-\*,good-\*,base} gstreamer1-plugin-openh264 gstreamer1-libav --exclude=gstreamer1-plugins-bad-free-devel ffmpeg gstreamer-ffmpeg
sudo dnf install lame\* --exclude=lame-devel
sudo dnf group upgrade --with-optional Multimedia


Video

sudo dnf install ffmpeg ffmpeg-libs libva libva-utils
sudo dnf install intel-media-driver


H264 for Firefox

sudo dnf config-manager --set-enabled fedora-cisco-openh264
sudo dnf install -y openh264 gstreamer1-plugin-openh264 mozilla-openh264


Update Flatpak Repo

flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak update


Set Hostname

Go to settings, About...or...
hostnamectl set-hostname YOUR_HOSTNAME


Custom DNS Servers

sudo mkdir -p '/etc/systemd/resolved.conf.d' && sudo -e '/etc/systemd/resolved.conf.d/99-dns-over-tls.conf'

[Resolve]
DNS=1.1.1.2#security.cloudflare-dns.com 1.0.0.2#security.cloudflare-dns.com 2606:4700:4700::1112#security.cloudflare-dns.com 2606:4700:4700::1002#security.cloudflare-dns.com
DNSOverTLS=yes


Gnome Tweaks
	sudo dnf install gnome-tweaks

That will let you enable minimize and maximize buttons for windows.


Custom Kernel Parameters

	Allow you to squeeze out a little bit more performance from your system. Do not follow this if you share services and files through your network or are using fedora in a VM.
	Install Grub Customizer to implement these tweaks by
	sudo dnf install grub-customizer


Disable Mitigations

	Increases performance in multithreaded systems. The more cores you have in your cpu the greater the performance gain. Not advised for host systems on some networks for increased security vulnerabilities, using it on daily driver systems won't fetch any problems. 5-30% performance gain varying upon systems.
	Add mitigations=off in Kernel Parameters under General Settings in Grub Customizer and click save.


Zswap (for systems with <16 gigs of RAM)

	Acts as virtual memory. Useful for sytems with <16 gigs of ram.
	Add zswap.enabled=1 in Kernel Parameters under General Settings in Grub Customizer and click save.


Modern Standby

	Can result in better battery life when your laptop goes to sleep.
	Add mem_sleep_default=s2idle in Kernel Parameters under General Settings in Grub Customizer and click save.


Add fonts for Starship to work:
https://www.nerdfonts.com/
Download the FiraCode Nerd Font
mkdir ~/.local/share/fonts 
extract the font files into that directory.


Starship (terminal theme)

	Configure starship to make your terminal look good (refer https://starship.rs)
I’m using…”Nerd Font Symbols Preset”
Put the downloaded .TOML file in ~/.config and then follow their configuration command line.


Power settings

Change auto suspend for plugged in to off.  On battery change to 20 minutes.


Git



