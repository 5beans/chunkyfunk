# chunkyfunk

After having put in considerable effort in amassing the ins and outs of these files, I had a laugh when I thought: klepto's generosity. In this repo are configs and supporting scripts and whatnot that comprise the wicked functionality of my Debian desktop, which is a Lenovo laptop.

My current partition scheme is as folows, as you can see, I do not employ a SWAP partition:

╭─────────────────────────────────────────────────────────────────────╮
│ 3 local devices                                                     │
├────────────┬────────┬───────┬────────┬────────┬──────┬──────────────┤
│ MOUNTED ON │   SIZE │  USED │  AVAIL │  USE%  │ TYPE │ FILESYSTEM   │
├────────────┼────────┼───────┼────────┼────────┼──────┼──────────────┤
│ /          │  54.8G │  6.9G │  45.1G │  12.6% │ ext4 │ /dev/nvme0n1 │
│            │        │       │        │        │      │ p2           │
│ /boot/efi  │ 974.1M │  4.4M │ 969.7M │   0.4% │ vfat │ /dev/nvme0n1 │
│            │        │       │        │        │      │ p1           │
│ /home      │ 874.2G │ 47.2G │ 827.0G │   5.4% │ xfs  │ /dev/nvme0n1 │
│            │        │       │        │        │      │ p3           │
╰────────────┴────────┴───────┴────────┴────────┴──────┴──────────────╯


/etc/fstab looks like this:

# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/nvme0n1p2 during installation
UUID=b0819dc2-04ae-4744-89fd-56db99824e30 /               ext4    discard,noatime,errors=remount-ro 0       1
# /boot/efi was on /dev/nvme0n1p1 during installation
UUID=30A1-1C52  /boot/efi       vfat    umask=0077      0       1
# /home was on /dev/nvme0n1p3 during installation
UUID=74571b73-ec54-4d31-ae2c-e29741e05918 /home           xfs     noatime         0       0
tmpfs     /tmp    tmpfs    defaults,noatime,mode=1777    0    0
tmpfs     /var/spool    tmpfs    defaults,noatime,mode=1777    0    0
tmpfs     /var/tmp    tmpfs    defaults,noatime,mode=1777    0    0
tmpfs     /var/log    tmpfs    defaults,noatime,mode=1777    0    0

/etc/sysctl.conf reads:

 net.ipv4.tcp_fastopen = 3
 net.ipv4.tcp_syncookies = 1
 net.core.rmem_max=16777216
 net.core.wmem_max=16777216
 vm.dirty_background_ratio=20
 vm.dirty_ratio=60

 I employ <a target="_blank" href="https://github.com/yokoffing/BetterFox">BetterFox</a> with some overrides in user.js:

 /****************************************************************************

 * START: MY OVERRIDES                                                      *

****************************************************************************/

// visit https://github.com/yokoffing/Betterfox/wiki/Common-Overrides
// visit https://github.com/yokoffing/Betterfox/wiki/Optional-Hardening
// Enter your personal overrides below this line:
// PREF: improve font rendering by using DirectWrite everywhere like Chrome [WINDOWS]
user_pref("gfx.font_rendering.cleartype_params.rendering_mode", 5);
user_pref("gfx.font_rendering.cleartype_params.cleartype_level", 100);
user_pref("gfx.font_rendering.cleartype_params.force_gdi_classic_for_families", "");
user_pref("gfx.font_rendering.directwrite.use_gdi_table_loading", false);
user_pref("identity.fxaccounts.enabled", false);
// PREF: disable login manager
user_pref("signon.rememberSignons", false);
// PREF: disable address and credit card manager
user_pref("extensions.formautofill.addresses.enabled", false);
user_pref("extensions.formautofill.creditCards.enabled", false);
// PREF: do not allow embedded tweets, Instagram, Reddit, and Tiktok posts
user_pref("urlclassifier.trackingSkipURLs", "");
user_pref("urlclassifier.features.socialtracking.skipURLs", "");
// PREF: disable disk cache
//user_pref("browser.cache.disk.enable", false);



I run Debian with a dedicated /home partition on a drive so I can maintain my config's throughout however many ridiculous installs I do. I haven't scripted anything, instead, borrowing from Leo 
https://github.com/leomarcov/debian-openbox/blob/master/README.md
to kick off some immediate install handiness following a Debian net-install, bare-bones. Then I login as root, install sudo, logout, log back in as me, and run a quick apt install to get my stuff back. I could probably make a live distro out of my system, or, any number of other ways to install my Debian system following a freesh net-install
