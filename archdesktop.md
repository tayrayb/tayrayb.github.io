* So you want to install a desktop environment, I trust you have booted into your fresh Arch Install. Lets get started.
* Now what do you want? Cinnamon, Gnome? Or you a hardcore Openbox fan?
* Well, lets start with the basics. Installing your graphics drivers and display manager. For graphics driver, you need to identify what card you have.

| Intel | Nvidia | AMD | AMD (New Model GPU's) | Virtualbox |
| ----- | ------ | --- | --------------------- | ---------- |
| xf86-video-intel | xf86-video-nouveau | xf86-video-ati | xf86-video-amdgpu | xf86-video-vesa |

* For this install, I'll being using the `vesa` driver for Virtualbox, Run:

```
pacman -S xorg xf86-video-vesa
```
