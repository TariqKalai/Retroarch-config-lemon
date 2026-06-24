# RetroArch on Lemon — debos recipe

A debos recipe that builds a ready-to-flash Ubuntu image for the **Citronics Limeboard**, with RetroArch pre-configured and launching automatically on boot via Openbox/Xorg.

Built on top of [Citronics/debos-lemon](https://github.com/Citronics/debos-lemon) which provides the base overlays, scripts, and kernel.

---

## What's included

- Ubuntu Noble (24.04) base — minimal variant
- RetroArch with OpenGL driver, XMB menu, fullscreen on boot
- Xorg + Openbox as a minimal display session
- Screen auto-rotated to landscape (DSI-1, `xrandr --rotate right`)
- Adreno 330 GPU firmware (`a330_pm4.fw` / `a330_pfp.fw`)
- French keyboard layout pre-configured
- `citro` user with correct groups (`video`, `render`, `audio`, `input`)
- Network Manager, ModemManager, Bluetooth, SSH

---

## Prerequisites

- A Linux machine with `debos` installed
- `android-sdk-libsparse-utils` for `img2simg`
- `fastboot`
- `lk2nd` flashed on the Limeboard boot partition (see [debos-lemon](https://github.com/Citronics/debos-lemon))

```bash
sudo apt install debos android-sdk-libsparse-utils fastboot
```

---

## Build & flash

```bash
# 1. Clone the base debos-lemon repo (provides overlays and scripts)
git clone https://github.com/Citronics/debos-lemon
cd debos-lemon

# 2. Drop the recipe in
curl -O https://raw.githubusercontent.com/TariqKalai/Retroarch-config-lemon/main/retro_recipe_ubuntu.yaml

# 3. Build the image (takes a while)
sudo debos retro_recipe_ubuntu.yaml

# 4. Convert to sparse image
img2simg retroarch-ubuntu-lemon.img sparse-retroarch-ubuntu-lemon.img

# 5. Set dipswitch to fp2 mode, enter fastboot, then flash
fastboot flash userdata sparse-retroarch-ubuntu-lemon.img

# 6. Restore dipswitch to host mode and reboot
```

RetroArch launches automatically on first boot.

---

## Flashing a pre-built image

Go to the [Releases](https://github.com/TariqKalai/Retroarch-config-lemon/releases) page and download `sparse-retroarch-ubuntu-lemon.img`, then flash directly:

```bash
fastboot flash userdata sparse-retroarch-ubuntu-lemon.img
```


## Based on

- [Citronics/debos-lemon](https://github.com/Citronics/debos-lemon) — base debos recipes and board support
- [libretro/RetroArch](https://github.com/libretro/RetroArch)
