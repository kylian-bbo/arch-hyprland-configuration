# Arch Linux/Hyprland configuration

Voici des notes sur mes découvertes et avancées faites durant la configuration de Arch Linux avec le gestionnaire de fenêtres Hyprland.

Je rajouterais les *.config* et autres dossiers au fur et à mesure.

## Installation de Arch Linux

### Clavier FR
```
loadkeys fr
```
### Connexion au Wi-fi
```
iwctl
  device list  -  Pour afficher les récepteurs Wi-Fi
  station name scan
  station name get-networks  -  Pour afficher le nom des réseaux détectés
  station name connect SSID  -  Pour se connecter au réseau de son choix
```

### Installation
```
archinstall
```
- Profile Type : Minimal
- Audio : Pipewire
- Network configuration : Copy ISO network configuration to installation

## Arch Linux

### Packages

Principaux :
- nano, acpilight, alsa-utils, Hyprland [+ wpa_supplicant], eww, kitty [+ gtk4], Thunar [gvfs *(pour USB)*], code [libsecret et gnome-keyring *(Pour push)*], git, yay, brave-bin, pfetch

Fonts (probablement pas définitif) :
  - noto-fonts, nerd-fonts *(pour les icônes système - Commit Mono)*

Et en supplément :
  - xorg-xev *(pour obtenir des informations sur les touches du clavier, utiles pour la gestion de la luminosité par exemple)*

### Installer depuis AUR (Arch User Repository)
```
yay -S X
``` 
<p align="center">
  OU
</p>

```
git clone X
cd x
makepkg -si
```

## Hyprland

### Contrôle de la luminosité de l'écran

- Installer le package :
  - acpilight
- Permettre à l'utilisateur de changer la luminosité sans "sudo" en créant un fichier "99-backlight.rules" dans *"etc\udev\rules.d"* contenant :
```
SUBSYSTEM=="backlight", ACTION=="add", \
	RUN+="/bin/chgrp USER_NAME /sys/class/backlight/%k/brightness", \
	RUN+="/bin/chmod g+w /sys/class/backlight/%k/brightness"
```
- Rajouter dans la configuration de Hyprland (exemple dans le cas d'un Dell Inspiron) :
```
bind = , XF86MonBrightnessDown, exec, xbacklight -dec 10
bind = , XF86MonBrightnessUp, exec, xbacklight -inc 10
```
### Contrôle du volume

- Installer le package :
  - alsa-utils

- Rajouter dans la configuration de Hyprland (exemple dans le cas d'un Dell Inspiron) :
```
bind = , XF86AudioMute, exec, amixer set Master 0%
bind = , XF86AudioLowerVolume, exec, amixer set Master 2%-
bind = , XF86AudioRaiseVolume, exec, amixer set Master 2%+
```