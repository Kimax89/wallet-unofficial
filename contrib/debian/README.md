
Debian
====================
This directory contains files used to package clashicd/clashic-qt
for Debian-based Linux systems. If you compile clashicd/clashic-qt yourself, there are some useful files here.

## bitcoinclashic: URI support ##


clashic-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install clashic-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your clashic-qt binary to `/usr/bin`
and the `../../share/pixmaps/clashic128.png` to `/usr/share/pixmaps`

clashic-qt.protocol (KDE)

