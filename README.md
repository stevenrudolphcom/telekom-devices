# Telekom Devices for Home Assistant

A custom Home Assistant integration for Telekom Speedport routers, based on
the MIT-licensed [FaserF/ha-speedport](https://github.com/FaserF/ha-speedport)
project.

This fork keeps the existing Speedport integration features and adds a native
connector for Speedport Pro Plus routers that expose the Sagemcom XMO API.
It also includes an optional Lovelace router flyout.

> This is an unofficial project and is not affiliated with Deutsche Telekom.

## Features

- Monitor internet, DSL, IPv4/IPv6, traffic, firmware, and connected devices.
- Track network clients and display dynamically discovered mesh nodes.
- Control supported Wi-Fi, WPS, reconnect, reboot, and firmware functions.
- Generate a Wi-Fi QR code.
- Support legacy JSON/MD5, encrypted JSON/SHA-256, and Sagemcom XMO/SHA-512
  login flows.
- Refresh and inspect cached raw router data through Home Assistant services.
- Optional frontend card with router status, manual refresh, and raw XMO details.

## Supported models

The upstream model support is retained. Confirmed local XMO testing covers:

- **Speedport Pro Plus**
- Firmware `120141.6.5.021.0`

Other Speedport models remain supported according to the upstream integration,
but the XMO extension is not a blanket compatibility claim for every model or
firmware.

## Install through HACS

1. Add `https://github.com/stevenrudolphcom/telekom-devices` as a custom
   HACS repository in the **Integration** category.
2. Install **Telekom Devices / Speedport**.
3. Restart Home Assistant.
4. Add the **Telekom Speedport** integration and enter the router address and
   password.

Only one integration with the Home Assistant domain `speedport` may be
installed. Remove or back up an existing Speedport custom integration first.

## Manual installation

Copy [`custom_components/speedport`](custom_components/speedport) to
`/config/custom_components/speedport`, restart Home Assistant, and add the
integration from **Settings > Devices & Services**.

## Speedport Pro Plus and XMO

The connector detects the verified Pro Plus GUI fingerprint before selecting
XMO. Authentication uses the router's SHA-512 challenge-response flow with
temporary session tokens. The password remains in the Home Assistant backend
and is never required by the frontend card.

The following controls are currently unavailable through XMO and return an
explicit error:

- Wi-Fi switching
- Guest Wi-Fi switching
- Internet reconnect
- WPS
- Firmware installation

Reboot support is implemented but still requires a deliberate real-device test.

## Optional router flyout

1. Copy the files in [`www`](www) to `/config/www`.
2. Add `/local/router-flyout-card.js?v=1` as a **JavaScript module** Lovelace
   resource.
3. Import or adapt [`examples/dashboard.yaml`](examples/dashboard.yaml).
4. Replace the example entity IDs with the IDs from your Home Assistant.

The card calls `button.press` and then reads `speedport.get_raw_data` through
the Home Assistant WebSocket API. It does not contact the router directly.

See [architecture](docs/ARCHITECTURE.md) and
[current testing status](docs/STATUS.md).

## Troubleshooting

Speedport routers can enforce a single active administration session. If the
Telekom Zuhause app reports that its device list cannot be loaded, enable
**Log out after each fetch** in the integration options. This releases the
router session after polling but increases network overhead.

For Pro Plus:

- Use a trusted local network.
- Confirm that the router GUI at `/6.5/gui/` is reachable.
- Do not expose the router management interface to the internet.
- Review diagnostics before sharing them because they can contain network
  details.

## Development

```powershell
uv sync --extra test
uv run python -m pytest tests
uv run python -m compileall -q custom_components
npm ci
npm test
```

Python mock tests do not replace installation and authentication tests on a
real Home Assistant system and router.

## Origin, license, and trademark

This repository is a maintained fork of
[FaserF/ha-speedport](https://github.com/FaserF/ha-speedport). The original
attribution and MIT license are preserved in [LICENSE](LICENSE).

Telekom and Speedport are trademarks of their respective owners. They are used
only to identify compatibility.
