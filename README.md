# MIROS v1.0: Web Flasher & Showcase

A tiny operating system for **ESP32-C3 boards with the 0.42" (72×40) OLED**, driven by a single button. This repo is the showcase page with an interactive simulator, plus a **one-click in-browser flasher**. No tools or drivers to install, just Chrome.

**Live page:** https://puzzledzen.github.io/miros-web-flasher/

## Flash it yourself

1. Open the live page in **Google Chrome or Microsoft Edge** on a computer. Firefox, Safari and phones don't support Web Serial.
2. Plug the board in with a USB-C **data** cable.
3. Click **⚡ Flash It Yourself**, then **Connect & Install MIROS**.
4. In Chrome's popup choose the board's port (**USB JTAG/serial debug unit**, or CH340/CP210x on some boards) and press **Connect**.
5. Press **Install** and wait for 100%. Tick **Erase device** for a clean install (this also clears saved high scores).
6. Press RST or replug. The MIROS menu appears on the OLED.

### Troubleshooting

- **No port listed:** use a different cable (it must carry data) or another USB port.
- **Failed to connect:** hold **BOOT**, tap **RST**, release BOOT, then try again.
- **Port busy:** close any serial monitor (Arduino IDE, PlatformIO) or other flasher tabs.
- **Windows with CH340/CP210x:** install the USB-serial driver and replug.
- **Linux:** `sudo usermod -aG dialout $USER`, then log out and back in.

## Controls

MIROS uses only the **BOOT** button.

| Gesture | Action |
|---|---|
| Single tap | Scroll menu / primary action in an app |
| Double tap | Reset stopwatch, timer or counter |
| Hold 0.7 s | Enter an app / exit to menu |
| Hold 3 s | Start countdown timer |
| Hold 2 s (Dino) | Exit Dino Runner |

## Built-in apps (16)

Stopwatch, Countdown Timer, Pomodoro, Dice, Magic 8-Ball, Coin Flipper, Breathing Coach, Tally Counter, Flashlight & SOS, Snake, Flappy Bird, Pong, Space Defender, Dino Runner, QR Code Display, About. High scores and the tally counter are stored in NVS flash and survive power loss.

## Repo contents

| File | Purpose |
|---|---|
| `index.html` | Showcase page, simulator and flasher popup |
| `manifest.json` | [ESP Web Tools](https://esphome.github.io/esp-web-tools/) manifest (chip `ESP32-C3`, merged image at offset `0`) |
| `miros-merged-firmware.bin` | Merged firmware image (bootloader + partitions + app) |

## Run locally

Web Serial needs HTTPS or `localhost`:

```bash
python3 -m http.server 8000
# open http://localhost:8000 in Chrome
```

## Updating the firmware

1. Build a new merged image, for example:
   ```bash
   esptool.py --chip esp32c3 merge_bin -o miros-merged-firmware.bin \
     --flash_mode dio --flash_size 4MB \
     0x0 bootloader.bin 0x8000 partitions.bin 0xe000 boot_app0.bin 0x10000 firmware.bin
   ```
2. Replace `miros-merged-firmware.bin` and bump `version` in `manifest.json`.
3. Commit and push. GitHub Pages redeploys automatically.

## Credits

Made by [PuzzledZen](https://github.com/PuzzledZen). Flashing is powered by [ESP Web Tools](https://github.com/esphome/esp-web-tools).
