These are scripts, services, and configuration files I needed to get Wii Remotes working properly under Dolphin/RetroArch with a [MAYFLASH W010 Wireless Sensor Dolphinbar](https://www.amazon.com/Mayflash-W010-Wireless-Sensor-DolphinBar/dp/B00HZWEB74) on Linux (Debian 12). Please check the main README for configuration settings required inside of Dolphin or RetroArch and more details on the environment.

## BlueTooth Auto Scan
For a while, I was stuck on getting my Wii Remotes to work under Debian 12 using the Dolphinbar mentioned above. Eventually, I found the issue was due to BlueTooth not actively scanning and therefore, either had to initialize scanning manually via the `bluetoothctl scan on` command or use my scripts below to automatically enable scanning on boot.

The [bluetooth_autoscan.sh](./scripts/bluetooth_autoscan.sh) script basically powers on and starts scanning for BlueTooth devices. The [bluetooth-autoscan.service](./systemd/bluetooth-autoscan.service) service file will run on startup so you don't need to enable BlueTooth scanning manually.

Copy `scripts/bluetooth_autoscan.sh` to `/opt` and `systemd/bluetooth-autoscan.service` to `/etc/systemd/system`. Afterwards, run `systemctl enable --now bluetooth-autoscan.service` (as root or with `sudo`) to start and enable the service on boot.

## UDEV Rules
You'll most likely need to setup udev rules if you want to run Dolphin/RetroArch as non-root (recommended) and get Wii Remotes to work.

Copy [`51-dolphin.rules`](./udev/51-dolphin.rules) to `/etc/udev/rules.d/`.