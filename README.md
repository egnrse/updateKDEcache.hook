# updateKDEcache.hook
A pacman hook to update the kService system configuration cache automatically.

This is intendet for KDE apps like [Dolphin](https://apps.kde.org/dolphin/) running outside of Plasma, that rely on the [kService desktop file configuration cache](https://techbase.kde.org/Development/Architecture/KDE3/System_Configuration_Cache) on Arch Linux.

## Usage
Install the following dependecies:  
`pacman -S coreutils archlinux-xdg-menu kservice sudo`

Put the file [updateKDEcache.hook](./updateKDEcache.hook) into `/etc/pacman.d/hooks/updateKDEcache.hook`  
For Security reasons, I advise to `chown root:root updateKDEcache.hook` this file.


## Explanation
KDE has their own system configuration cache that dolphin uses to fetch all available desktop files. If this cache gets invalidated (eg. by installing software that adds a `*.desktop` file) dolphin stops using it.  
The cache can be manually updated with: `XDG_MENU_PREFIX=arch- kbuildsycoca6 --noincremental`  

The provided hook automatically runs a similar command:  
`/bin/bash -c "sudo -u \"$(logname)\" bash -lc 'XDG_MENU_PREFIX=arch- /usr/bin/kbuildsycoca6 --noincremental'"`

We want to update the cache of the current user (hooks are executed as root) => `sudo`  
`kbuildsycoca` needs some environment variables that are set by loginshells (hooks are executed in a slightly sandboxed environment) => `bash -lc`
