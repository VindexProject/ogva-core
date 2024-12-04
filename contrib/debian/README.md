
Debian
====================
This directory contains files used to package ogvad/ogva-qt
for Debian-based Linux systems. If you compile ogvad/ogva-qt yourself, there are some useful files here.

## ogva: URI support ##


ogva-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install ogva-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your ogva-qt binary to `/usr/bin`
and the `../../share/pixmaps/ogva128.png` to `/usr/share/pixmaps`

ogva-qt.protocol (KDE)

