# Verification status

## Verified locally

The XMO extension was exercised with a Speedport Pro Plus running firmware
`120141.6.5.021.0`:

- SHA-512 login and authenticated XMO requests succeeded.
- Host lists and router status fields were mapped into Home Assistant.
- Flyout opening and manual refresh completed successfully.
- Updating Home Assistant states did not recreate the flyout DOM.
- Python mock tests and JavaScript DOM tests passed.

## Known limitations

- Wi-Fi, reconnect, WPS, and firmware control are not implemented for XMO.
- Reboot is implemented but should be verified with a deliberate reboot.
- Additional firmware versions are not yet confirmed.
- XMO client counts and mesh mappings need more real-device comparison.
- CI tests use mocks and do not replace tests on a live Home Assistant host.

## Main implementation files

- XMO connector and mapping:
  [custom_components/speedport/api.py](../custom_components/speedport/api.py)
- Refresh entity:
  [custom_components/speedport/button.py](../custom_components/speedport/button.py)
- Flyout card: [www/router-flyout-card.js](../www/router-flyout-card.js)
- XMO tests: [tests/test_xmo.py](../tests/test_xmo.py)
