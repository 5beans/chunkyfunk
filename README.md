# chunkyfunk

After having put in considerable effort in amassing the ins and outs of these files, I had a laugh when I thought: klepto's generosity.
In this repo are configs and supporting scripts and whatnot that comprise the wicked functionality of my Debian desktop, which is a Lenovo laptop.

I run Debian Trixie (currently testing) with a Liquorix kernel and the openbox window manager. From a login tty I run startx and auto login
as the following line is in ~/.profile:

 [ "$(tty)" = "/dev/tty1" ] && exec startx

My current partition scheme is as folows, as you can see, I do not employ a SWAP partition:

![2025-03-17_657x155_15:28:05](https://github.com/user-attachments/assets/75f21922-8c97-48d0-98ad-92707a91241c)


  I added the following to /etc/fstab:

tmpfs     /tmp    tmpfs    defaults,noatime,mode=1777    0    0<br />
tmpfs     /var/spool    tmpfs    defaults,noatime,mode=1777    0    0<br />
tmpfs     /var/tmp    tmpfs    defaults,noatime,mode=1777    0    0<br />
tmpfs     /var/log    tmpfs    defaults,noatime,mode=1777    0    0<br />

  /etc/sysctl.conf reads:

 net.ipv4.tcp_fastopen = 3<br />
 net.ipv4.tcp_syncookies = 1<br />
 net.core.rmem_max=16777216<br />
 net.core.wmem_max=16777216<br />
 vm.dirty_background_ratio=20<br />
 vm.dirty_ratio=60

 I employ <a target="_blank" href="https://github.com/yokoffing/BetterFox">BetterFox</a> with some overrides in user.js:

user_pref("gfx.font_rendering.cleartype_params.rendering_mode", 5);<br />
user_pref("gfx.font_rendering.cleartype_params.cleartype_level", 100);<br />
user_pref("gfx.font_rendering.cleartype_params.force_gdi_classic_for_families", "");<br />
user_pref("gfx.font_rendering.directwrite.use_gdi_table_loading", false);<br />
user_pref("identity.fxaccounts.enabled", false);<br />
user_pref("signon.rememberSignons", false);<br />
user_pref("extensions.formautofill.addresses.enabled", false);<br />
user_pref("extensions.formautofill.creditCards.enabled", false);<br />
user_pref("urlclassifier.trackingSkipURLs", "");<br />
user_pref("urlclassifier.features.socialtracking.skipURLs", "");


I make some changes in about:config, as well, and will include these in the future.

I install Debian using the net installer burn'd to a USB stick using the expert tui installation option that I might install a targeted kernel.
Additionally, I allow root login. My partitioning scheme changes periodically and it you are using these files for a smiliarly wicked desktop/workstation
setup, you will partition as you see fit. When it comes time to install software in the Debian installer (tasksel) uncheck desktop and gnome and
leave standard system utilities as the only chosen option.

Upon reboot post a successful Debian installation, I log in as root to install: sudo aptitude. Then I run visudo to add my 
user account with the NOPASSWD:ALL option because, pfft...

I log out and back in as my user account to run (sudo) the following handy script that installs my desktop: ~/bin/baseinst.sh

<a target="_blank" href="https://github.com/leomarcov/debian-openbox/blob/master/README.md">Leo Marcov</a> has a couple handy post-install scripts
I employ, namely:

script_loginfetch<br />
install_sublimetext<br />
config_grub-skip<br />

He's got more that you might like, check it out.

After this I reboot. Upon loggin in my typical awesomeness is available to me and I can work/play. I install Liquorix not, fire-up a terminal:

curl -s 'https://liquorix.net/install-liquorix.sh' | sudo bash

reboot and enjoy.
