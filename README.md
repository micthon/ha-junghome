# ha-junghome

> \*\*Fork notice:\*\* This is a fork of \[mariusbiller/ha-junghome](https://github.com/mariusbiller/ha-junghome) that adds support for JUNG HOME room thermostats (see \[Room thermostats](#room-thermostats)). Everything else is the original work of Marius Biller and contributors.

This project integrates JUNG HOME devices into Home Assistant.
Communication with JUNG HOME devices uses the local network via the JUNG HOME Gateway - no cloud required.

## Supported device types

* On/Off Light
* Dimmable Light
* Tunable White Light (switching and brightness only)
* Socket (including power measurement)
* Window Cover
* Rocker Switch (an event entity per direction firing `press` and `release`, so automations can react to a tap or to how long a key is held - for example hold-to-dim; plus the status LED in the rocker face as an on/off light)
* Room Thermostat (climate entity with target temperature, current temperature and the presets Frost / Eco / Comfort - fork addition)
* Gateway as Hub device



## Room thermostats

Each JUNG HOME room thermostat shows up as a Home Assistant climate entity:

* Current temperature and target temperature (5 - 30 °C in steps of 0.5 °C).
* The presets **Frost**, **Eco** and **Comfort**. The gateway only knows these three; there is no preset "none" that could be set.
* The gateway offers no way to switch a thermostat off, so the entity is always in mode `heat`. Use the preset Frost as the "off" setting.
* A thermostat the gateway reports as unreachable is shown as unavailable.

How the presets work: The temperature behind each preset is stored in the thermostat itself and cannot be read directly. The gateway reports a preset together with the target temperature whenever the target matches one of the three. The entity remembers these pairs (attribute `preset\_temperatures`, kept across restarts). If you later set exactly that temperature with +/-, the preset is shown again. **Select each preset once per thermostat after the first start** so that all three temperatures are known. If no preset matches, the preset field stays empty.

The `switch` datapoint of the thermostat is deliberately not evaluated: its meaning is unclear (it reported `1` even when the room was well above the target temperature).

## Prerequisites

* JUNG HOME devices have already been installed and set up using the official JUNG HOME app.
* A JUNG HOME Gateway has been added, is active, and is on the same local network as your Home Assistant instance.
* Make sure your JUNG HOME Gateway's IP address does not change, for example by creating a DHCP reservation in your router.



## Installation

### Option 1: Manual Installation

1. Copy this folder to `<config\_dir>/custom\_components/junghome/`
2. Restart Home Assistant.

### Option 2: Install via HACS

> \*If you have not installed HACS (Home Assistant Community Store) yet, follow the official guide: \[HACS Installation Guide](https://hacs.xyz/docs/use/download/download/)\*

1. Open Home Assistant and go to **HACS > Integrations**.
2. Click the three dots in the top-right corner and select **Custom repositories**.
3. Add the URL of this GitHub repository: `https://github.com/micthon/ha-junghome`.
4. Select the category **Integration**.
5. Search for **JUNG HOME** in HACS and install the integration.
6. Restart Home Assistant.

## Setup

1. In Home Assistant, go to **Settings > Devices \& services** and click **Add Integration**.
2. Search for **JUNG HOME** and start the setup flow.
3. Enter your JUNG HOME Gateway's IP address. If Home Assistant has discovered the gateway automatically, confirm the suggested IP address.
4. Press the button on your JUNG HOME Gateway when the flow asks you to allow Home Assistant access. The integration will request the access token automatically.
5. If automatic token registration fails, enter the authentication token manually in the next step.

**Hints**

* *Make sure your JUNG HOME Gateway's IP address does not change, for example by creating a DHCP reservation in your router.*
* *For proper discovery, make sure your JUNG HOME Gateway and your Home Assistant instance are in the same network subnet.*

## Credits

* Special thanks to [@luismalves](https://github.com/luismalves) for bringing the integration to the next level.
* Special thanks to [@kkellermann](https://github.com/kkellermann) for integrating the socket measuring sensor function.
* Thermostat support in this fork: [@micthon](https://github.com/micthon), developed with the assistance AI.
* Notes on how the gateway handles thermostats (presets, temperature values) come from the documentation of [ernetas/junghome](https://github.com/ernetas/junghome).

## Disclaimer

*The following text is the original author's disclaimer and applies to this fork as well.*

This project is a privately developed open-source integration for the JUNG HOME smart home system provided by Albrecht Jung GmbH \& Co. KG. I have not been commissioned or authorized by them to create this integration.

This project is offered as is, without any guarantee or warranty of any kind. I do not assume any responsibility for any issues, damages, or losses that may arise from the use of this integration.
Users are encouraged to review and understand the terms and conditions of Albrecht Jung GmbH \& Co. KG's services before using this integration.

Contributions and feedback from the community are welcome and appreciated, but please keep the limitations and disclaimers stated here in mind.

Thank you.

