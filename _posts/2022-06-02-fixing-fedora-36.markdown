---
layout: post
title:  "Fixing Fedora 36 - repositories, theme inconsistencies and choppiness"
date:   2022-06-01 08:00:00 +1000
categories: gnome linux fedora
---
# Fixing Fedora 36
Fedora 36 is an undoubtedly great release of Fedora with new software and the same old Fedora features that you love, now with the new Gnome 42. 

This isn't to say it is flawless though: it has some problems.

These problems are:
- Inconsistent theming between GTK and libadwaita apps (if you're like me this will drive you crazy)
- Not including Flathub out of the box
- Not enough software in the repos
- Gnome 42 rendering fonts badly on 1080p screens
- Gnome 42 being choppy

### Inconsistent theming

This really doesn't need to be a problem. I know that the adw-gtk3 theme has some bugs, but you could always just change the theme slightly so it works properly.
It doesn't need to be completely accurate, just good enough, which the vanilla situation isn't. 

Here's how to fix it:

1. Install Gnome tweaks: `sudo dnf install gnome-tweaks`
2. Install the adw-gtk3 theme: `sudo dnf copr enable nickavem/adw-gtk3 && sudo dnf install adw-gtk3`
3. Install that theme in flatpak as well (it will automatically set): `flatpak install org.gtk.Gtk3theme.adw-gtk3 org.gtk.Gtk3theme.adw-gtk3-dark`
4. Set the theme in Gnome tweaks.

Here is the full set of commands:

```
sudo dnf install gnome-tweaks; \
sudo dnf copr enable nickavem/adw-gtk3 && sudo dnf install adw-gtk3; \
flatpak install org.gtk.Gtk3theme.adw-gtk3 org.gtk.Gtk3theme.adw-gtk3-dark
```

Now just set it in tweaks, and you're all done. 

Note: If you change your light/dark theme setting in Settings, you will also have to change it in Tweaks as well if you want to keep them in sync. I've heard there's an extension called [Night Theme Switcher](https://nightthemeswitcher.romainvigier.fr/) can help with that. I haven't tried it.

### Not including Flathub out of the box

This is probably due to some sort of licensing issue, but it is annoying regardless.

Thankfully it is a pretty easy fix.

`flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo`

### Not enough software in the repos

This is also due to licensing issues and this is also annoying. 

Here's how to fix it (paste into your terminal):

```
sudo dnf install -y https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm; \
sudo dnf groupupdate -y core; \
sudo dnf groupupdate -y multimedia --setop="install_weak_deps=False"
```

This downloads the free software and non free software RPMFusion repositories, giving you a much larger array of software to choose from.

### Gnome 42 rendering fonts badly on 1080p screens.

If you have a HiDPI screen don't bother with this one. 

No idea how this one got shipped considering the fix is so easy. Bug report [here](https://pagure.io/fedora-workstation/issue/295). 

And here's the fix: 
`echo "[Settings]\ngtk-hint-font-metrics=1" >> ~/.config/gtk-4.0/settings.ini`

### Gnome 42 being choppy

All you need to do is install a patched version of Gnome with triple buffering support.

Enable the COPR repo,
`sudo dnf copr enable calcastor/gnome-patched`

then update your system. This might take a while.
`sudo dnf upgrade`

### Some personal touches

These aren't necessary, I just like them on my system. 

1. Installing [Blur My Shell](https://extensions.gnome.org/extension/3193/blur-my-shell/). Makes Gnome look better.
2. Installing this cool [Firefox theme](https://github.com/rafaelmardojai/firefox-gnome-theme). Fits in much better with Gnome this way. 
3. Changing the fonts from Cantarell to IBM Plex Sans. It looks much better.
4. Installing [Oh My ZSH](https://extensions.gnome.org/extension/3193/blur-my-shell/).
