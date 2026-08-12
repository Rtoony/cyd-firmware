# cyd-firmware

Firmware images for a small family of ESP32 "Cheap Yellow Display" wall panels —
a 2.8" 320×240 board showing a clock, the weather, and indoor light level.

The panels check this repository every few hours and install newer firmware
themselves. That is the only reason it is public: a panel sitting on someone
else's Wi-Fi, behind their router, can still be fixed without asking anything of
them and without any inbound access to their network.

## Is it safe for these to be public?

Yes, by construction. These images contain **no credentials** — no Wi-Fi
password, no API key, no OTA password — because a panel needs none:

- Wi-Fi credentials are entered by the owner through a setup page the panel
  serves itself, and are stored on the device, never compiled in
- nothing pushes to a panel, so there is no API key
- updates are pulled rather than pushed, so there is no OTA password
- weather comes from [Open-Meteo](https://open-meteo.com/), which needs no key

Every image is scanned for known credentials, as text and base64-decoded, and
the publishing tool refuses to release on a hit.

## Layout

```
<panel-name>/manifest.json          version pointer the panel polls
<panel-name>/<panel-name>-<ver>.bin the image itself
```

Served over HTTPS by GitHub Pages. Each panel has its own directory, so a fix
can be staged to one before the rest.

`manifest.json` is the [ESP Web Tools](https://esphome.github.io/esp-web-tools/)
format that ESPHome's `update` platform parses:

```json
{
  "name": "cyd-gift-01",
  "version": "1.8.0",
  "builds": [{ "chipFamily": "ESP32", "ota": { "path": "...bin", "md5": "..." } }]
}
```

## Owning one of these panels

Plug it in. If it cannot find a network it knows, it starts its own Wi-Fi and
shows the name on screen — join that from a phone and the setup page opens by
itself.

Moved house, or changed your router? Power-cycle the panel five times in quick
succession. It forgets the old network and returns to setup mode. A countdown
appears on screen so you can see it working.

## Source

Firmware is built with [ESPHome](https://esphome.io/). The configuration lives
in a private repository; these are the published build outputs.
