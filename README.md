# chunkyfunk

Debian Trixie + Openbox wickedness. In this repo are configs and supporting scripts and whatnot that comprise the wicked functionality of my Debian desktop, which is running on a Lenovo P53s laptop. It's a very simple desktop configuration that is fast as hell and reliable. Most of what I do is set once and forget it and I employ many handy keybinds, aliases, and scripts. I am not using a login manager or wayland and there's no ricing going on here.

Following is a handy reference to get to the same wicked functionality but it's not a bunch of fancy scripts or push-button functionality to get it. The steps include:

installing Debian with the net installer
installing sudo and aptitude
running a handy script to install my desktop
a few handy post-installation tweaks

Included in this repo are some supporting files like: .bashrc and supporting, openbox configs, kitty, and some other stuff. Comes down to the pretty and some handy function.

Tip: I always install using the same user account so I can use the configs I made years ago. I have everything I need on an external drive and usb nub for those times I use a new user account that I may just run mc real quickly to copy everything over to the new user ~/ directory.

My current partition scheme is as folows, as you can see, I do not employ a SWAP partition:

![2025-09-14_772x171_12:20:11](https://github.com/user-attachments/assets/481b85e8-b548-48bd-9fab-e60d9f4a087c)

For the installation I use the expert tui installation option that I might install a targeted kernel.
Additionally, I allow root login. My partitioning scheme changes periodically and it you are using these files for a smiliarly wicked desktop/workstation
setup, you will partition as you see fit. When it comes time to install software in the Debian installer (tasksel) uncheck desktop and gnome and
leave standard system utilities as the only chosen option and proceede with the installation.

Upon reboot post a successful Debian installation, I log in as root to install: sudo and aptitude. Then I run visudo to add my 
user account with the NOPASSWD:ALL option because, pfft...

I log out and back in as my user account to run (sudo) the following handy script that installs my desktop: ~/bin/kevin

Following this I login and fire-up a terminal in order to accomplish a few post-install tasks.

	sudo update-alternatives --install /usr/bin/x-file-manager x-file-manager /usr/bin/thunar 210
	sudo update-alternatives --install /usr/bin/x-terminal-emulator x-terminal-emulator ~/Downloads/kitty-0.42.2-x86_64/bin/kitty 210

I added the following to /etc/fstab:

	tmpfs     /tmp          tmpfs    defaults,noatime,mode=1777    0    0
	tmpfs     /var/spool    tmpfs    defaults,noatime,mode=1777    0    0
	tmpfs     /var/tmp      tmpfs    defaults,noatime,mode=1777    0    0
	tmpfs     /var/log      tmpfs    defaults,noatime,mode=1777    0    0

I add the following to:  /etc/sysctl.conf

	net.core.default_qdisc=fq
	net.ipv4.tcp_congestion_control=bbr
	vm.swappiness=10

In FIrefox, I make some changes in about:config if I wiull use a new profile:

	privacy.resistFingerprinting = true
	privacy.trackingprotection.fingerprinting.enabled = true
	privacy.firstparty.isolate = true
	network.dns.disablePrefetch = true
	network.prefetch-next = false
	dom.event.clipboardevents.enabled = false
	browser.tabs.loadBookmarksInTabs true
	browser.tabs.loadBookmarksInBackground true
	browser.search.openintab true

I copy some fonts/icons/themes over to /usr/share/:

	sudo cp -R .local/share/fonts/* /usr/share/fonts/
	sudo cp -R .local/share/themes/* /usr/share/themes/
	sudo cp -R .local/share/icons/* /usr/share/icons/

Then, I make root look pretty:

	sudo lxappearance

If I will use a new profile I do:

    gsettings set org.gnome.desktop.interface gtk-theme "Nordic-darker"
    gsettings set org.gnome.desktop.wm.preferences theme "Nord-Openbox-theme-master"
    gsettings set org.gnome.desktop.interface icon-theme "Obsidian-Gray"

in ~/.gtkrc-2.0
    include "/usr/share/themes/Nordic-darker/gtk-2.0/gtkrc"

To get the notifications working, pfft, I do:

	sudo cp /usr/share/applications/notification-daemon.desktop /etc/xdg/autostart/
	sudo chmod +x /etc/xdg/autostart/notification-daemon.desktop

I'll add some other repos next, install some apps, remove a couple that I don't need, and install the Liquorix kernel:

Librewolf:

	sudo apt update && sudo apt install extrepo -y
	sudo extrepo enable librewolf

floorp:

	curl -fsSL https://ppa.ablaze.one/KEY.gpg | sudo gpg --dearmor -o /usr/share/keyrings/Floorp.gpg
	sudo curl -sS --compressed -o /etc/apt/sources.list.d/Floorp.list 'https://ppa.ablaze.one/Floorp.list'

phoenix:

	echo 'deb https://download.opensuse.org/repositories/home:/celenity/Debian_12/ /' | sudo tee /etc/apt/sources.list.d/home:celenity.list
	wget -O- https://download.opensuse.org/repositories/home:celenity/Debian_12/Release.key 2>/dev/null | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/home_celenity.gpg > /dev/null

sublime text:

	wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo tee /etc/apt/keyrings/sublimehq-pub.asc > /dev/null
	echo -e 'Types: deb\nURIs: https://download.sublimetext.com/\nSuites: apt/stable/\nSigned-By: /etc/apt/keyrings/sublimehq-pub.asc' | sudo tee /etc/apt/sources.list.d/sublime-text.sources

firefox:

	wget -q https://packages.mozilla.org/apt/repo-signing-key.gpg -O- | sudo tee /etc/apt/keyrings/packages.mozilla.org.asc > /dev/null
	echo "deb [signed-by=/etc/apt/keyrings/packages.mozilla.org.asc] https://packages.mozilla.org/apt mozilla main" | sudo tee -a /etc/apt/sources.list.d/mozilla.list > /dev/null

    echo '
    Package: *
    Pin: origin packages.mozilla.org
    Pin-Priority: 1000
    ' | sudo tee /etc/apt/preferences.d/mozilla

update
install firefox sublime-text

	remove exim* xdg-desktop-portal xdg-desktop-portal-gtk

	sudo update-alternatives --install /usr/bin/x-text-editor x-text-editor /usr/bin/subl 210
	
	curl -s 'https://liquorix.net/install-liquorix.sh' | sudo bash

 I don't add all of the above repos every time I reinstall, but, sublime and firefox are mainstays.

<a target="_blank" href="https://github.com/leomarcov/debian-openbox/blob/master/README.md">Leo Marcov</a> has a couple handy post-install scripts
I employ, namely:

script_loginfetch
config_grub-skip

He's got more that you might like, check it out.

Add:

	[ "$(tty)" = "/dev/tty1" ] && exec startx
 
to your  ~/.profile file to auto login from tty after inputting your credentials.
