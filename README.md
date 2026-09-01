# ESPHome Digital Magic 8 Ball

A shake-activated digital Magic 8 Ball built with ESPHome and an ESP32-S3. It
shows a randomly selected, editable phrase on a round LCD, colors the outer
ring according to battery level, follows device tilt with subtle text motion,
and switches off the display backlight after one minute of inactivity.

## Features

- Shake-to-roll using a QMI8658 accelerometer
- 38 phrases editable from Home Assistant or the device web page
- Editable curved text around the display ring
- Battery voltage and estimated percentage sensors
- Battery-level ring: green, yellow, or red
- Automatic wake on motion and backlight sleep after inactivity
- Encrypted ESPHome API, OTA updates, captive portal, and authenticated web UI

## Hardware and pin map

The configuration targets an `esp32-s3-devkitc-1` and these connected parts:

| Function | Component or pin |
| --- | --- |
| Display | 240×240 GC9A01A round LCD |
| LCD clock / MOSI | GPIO10 / GPIO11 |
| LCD CS / DC / reset | GPIO9 / GPIO8 / GPIO12 |
| LCD backlight | GPIO40 |
| Accelerometer | QMI8658 at I²C address `0x6B` |
| I²C SDA / SCL | GPIO6 / GPIO7 |
| Battery ADC | GPIO1, configured for a 3:1 voltage divider |

Confirm the pinout and battery-divider ratio for your board before flashing.
Connecting a battery directly to an ESP32 ADC pin can damage the device.

## Setup

1. Install ESPHome or use the ESPHome add-on in Home Assistant.
2. Copy `secrets.example.yaml` to `secrets.yaml`.
3. Replace every example credential in `secrets.yaml`. Generate fresh API and
   OTA credentials with the commands documented in that file.
4. Connect the board by USB and run:

   ```sh
   esphome run magic8ball.yaml
   ```

After the first installation, ESPHome can update the device over the air. The
device uses DHCP by default, so find its address through ESPHome, Home
Assistant, or your router.

## Customization

The 38 phrase fields and the ring-text field appear as editable text entities.
Their values are restored after reboot. Empty phrases are excluded from random
selection. You can also edit the `initial_value` entries in
`magic8ball.yaml` to ship a different default phrase set.

Useful tuning points in the YAML include the shake threshold (`0.85`), the
one-minute idle timeout (`IDLE_MS`), the ADC multiplier (`3.0`), and the LCD
`invert_colors` setting.

The QMI8658 integration is loaded from the third-party
[`dala318/esphome-qmi8658`](https://github.com/dala318/esphome-qmi8658)
component pinned to `v0.1.1`. Review that component before using it on a device
connected to a trusted network.

## Security

Never commit `secrets.yaml`. If credentials have previously appeared in a
shared file, repository, chat, or screenshot, rotate them before using the
device again. The included `.gitignore` prevents the normal secrets file from
being added to Git.

## License

Released under the [MIT License](LICENSE).
