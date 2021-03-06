A place to store my ~~i3 and~~ Emacs configuration, plus other misc scripts.

## Update December 2020

Early December I moved my personal laptop to Fedora 33 using the default Gnome setup. It works great for tablet mode,
and I liked Fedora enough that I moved my work laptop too.  
I might go back to the i3 setup someday, maybe after learning what I like and works from Gnome to replicate it.  
There's pleasure in running your own "custom DE", and I learned a lot about the inner workings of many things in Linux.
However the experience I get from Gnome is really polished, it's hard to compete with that :)

## Dependencies for a fresh install:

* Manjaro i3 (duh!)
* Consolas font (ttf-vista-fonts in AUR)
* acpilight (xbacklight replacement), pulseaudio_ctl for brightness and volume controls
* evdev-right-click-emulation for right click w/long touch tap
* flameshot for screenshots
* Probably others I missed but will keep adding here

## Emacs config notes:

A use-package based configuration, that tries its best to be platform agnostic. I moved the Windows-only things to a 
private repo @ work, since they only make sense in that environment.

Some high level notes:

* In most cases I prefer smaller packages, if built-in, even better (eg icomplete over ivy/helm)
* LSP (but sometimes Eglot) used for C# and Python
* Sly for Common Lisp
* Deadgrep (rg) for non-LSP code search
* Doom themes and Doom modeline (but sometimes mood-line)

## i3 config notes:

* The monitor switch bound to Win+p has a mode with some common options, backed by `monitorswitch.py`. Win+Shift+p works as a lifesaver by enabling the laptop screen only.
* .Xmodmap swaps ctrl and caps, and maps Menu to Print Screen because Thinkpad keyboards.
* From https://bbs.archlinux.org/viewtopic.php?id=223949, to activate BT @ startup:

```
  1. set bluetooth service to start on boot: systemctl enable bluetooth.service
  2. set bluetooth adapter to automatically power on: edit /etc/bluetooth/main.conf and set AutoEnable=true
  3. set paired devices as trusted *this is what I was missing*: from the bluetoothctl util, enter trust XX:XX:XX:XX:XX:XX for each paired device (replace XX... with mac address)
  Also:
  https://wiki.archlinux.org/index.php/Bluetooth_keyboard
```

## Thinkpad tablet mode notes

* Firefox touch scroll and zoom via: https://superuser.com/a/1485044
* Touch screen gestures via Gester https://github.com/witty91/gester/, customizations in this repo
  * Screen rotation with 4-finger rotate gesture, flip screen with 4 finger swipe from top/bottom edge
  * 3 finger swipe sideways to move between workspaces
  * 3 finger swipe up/down to show xfce4-panel (more details on the panel below)
* `rotate.py` handles rotation of the screen via xrandr and the inputs for pen/touchscreen (invoked by gester)
* Pen button default configuration:
  * Tip is left click
  * Lower button is middle click
  * Top button (eraser) works on hover, used as right click
* Fingerprint reader doesn't have a driver yet.

### Touch keyboard

* Testing CellWriter as an input method. Like it quite a bit.
* Screen keyboard is `onboard`, added to the lightdm greeter
* Changed `/usr/bin/i3exit` to not lock the screen on suspend, more convenient to keep as tablet

### xfce4-panel

Modified from the idea of https://peterme.net/adding-touch-controls-to-the-i3-window-manager.html, it has a single panel at the bottom of the screen.

Buttons:
* Hold-for-menu button with window controls: toggle floating, move up/down/left/right
* Go to to previous/next workspace, from 1~10, wraps around
* Close focused application
* Move current app to a new workspace (uses `move_to_first_available_workspace.py`)
* Volume up/down, using a custom script to allow volume > 100% (remember to update pulseaudio-ctl's `UPPER_THRESHOLD`)
* Brightness up/down, using a custom script to avoid the screen turning off when going to 0%
* Onboard. Added autocomplete to it.
* CellWriter/Xournal++/Application menu
* Sleep/Shutdown

Will keep experimenting with the buttons to see what else I need and what I can discard. Configuring the panel is a drag, I added to the repository all the xfce4 config stuff (panel + power manager).
