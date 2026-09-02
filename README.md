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

## Bill of materials

| Qty. | Item | Notes and example source |
| ---: | --- | --- |
| 1 | Waveshare ESP32-S3-LCD-1.28 | Use the non-touch 1.28-inch round LCD version targeted by this configuration. [Waveshare product page](https://www.waveshare.com/esp32-s3-lcd-1.28.htm) · [Amazon search](https://www.amazon.com/s?k=Waveshare+ESP32-S3-LCD-1.28) |
| 1 | 3.7 V LiPo battery, approximately 950 mAh | A protected 1-cell battery such as a 503450-size 950 mAh pack works if it fits the enclosure. The board uses an **MX1.25 2-pin connector**; verify connector type and polarity before connecting it. [Amazon search](https://www.amazon.com/s?k=3.7V+950mAh+LiPo+MX1.25+2-pin) |
| 5 | M2 × 12 mm machine screws | Used to fasten the enclosure. Choose the head style that matches the printed recesses. [Amazon search](https://www.amazon.com/s?k=M2+x+12mm+machine+screws) |
| 1 | Miniature SPDT slide switch | Use the switch style and dimensions required by your enclosure and wiring; wire it as the project's on/off switch. [Amazon search](https://www.amazon.com/s?k=mini+SPDT+slide+switch) |
| 1 | USB-C data cable | Needed for the first flash and useful for charging and diagnostics. [Amazon search](https://www.amazon.com/s?k=USB-C+data+cable) |
| As needed | Hookup wire, solder, and heat-shrink tubing | For the battery/switch connections. [Amazon search](https://www.amazon.com/s?k=electronic+hookup+wire+solder+heat+shrink+kit) |
| 1 | 3D printer | A typical FDM printer with a 0.4 mm nozzle is suitable for the enclosure. |
| About 60 g | PLA or PETG filament | I love PETG for heat resistance and durability, but the shiny nature feels weird to hold all day. PLA has been durable enough and feels better. 

Shopping links are examples, not endorsements or affiliate links. Availability
and listings change. Check dimensions, plug type, battery polarity, and voltage
before ordering or connecting any part. Never force a battery connector, be careful tightening the hold down bar to avoid unnecessary pressure. 

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

## Assembly and photos

Assembly photos will live in [`docs/images`](docs/images). A useful sequence is:

1. Print the enclosure parts and remove any supports. You can print it from Makerworld: https://makerworld.com/en/models/3249742-magic-8-ball#profileId-3682836
2. Test-fit the ESP32-S3 display board, battery, and switch before soldering.
3. Check battery connector type and polarity with the battery disconnected.
4. Wire and insulate the switch and battery connections.
5. Flash and test the device before closing the enclosure. At this point, turn the display on and make sure the screen is evenly black when lit up. If you tighten the cover too much, then it may have some white spots on the screen. You want it snug enough that people tapping aggressively don't knock it down, but not so tight that it puts pressure on the screen that causes discoloration. 
6. Route the wiring away from screw posts, then close the case with the three
   M2 × 12 mm screws, and the battery cover with two more. Don't over tighten the battery hold down bar.
   7. Screw on the top cover. It's a weird fit, depending on your printer tolerances, you may need to try several times to get it connected. 


## License

Released under the [MIT License](LICENSE).
