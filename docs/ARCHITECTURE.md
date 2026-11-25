# Architecture and origin

## Data flow

```text
Lovelace RouterFlyoutCard
  -> Home Assistant button.press
  -> SpeedportDataCoordinator.async_request_refresh()
  -> SpeedportClient
  -> SagemcomXmoClient -> /cgi/json-req
  -> coordinator data and Home Assistant entities
  -> speedport.get_raw_data
  -> targeted DOM updates in the existing flyout
```

The router status page is a client-side application. Its initial HTML does not
contain current values, and browser same-origin rules prevent the Home
Assistant frontend from reading it directly. The backend therefore uses the
router's native XMO API.

## Connector selection

`SpeedportClient` inspects `/6.5/gui/js/gui-core.js` for the verified XMO,
SHA-512, and empty-password-salt fingerprint. Legacy JSON and encrypted
modern-router flows continue to use the upstream connector.

The XMO client:

1. Calculates the SHA-512 login credential.
2. Opens a session and stores the returned session token.
3. Batches status paths in XMO requests.
4. Reads active hosts from `Device/Hosts/Hosts`.
5. Maps values into the same `SpeedportData` model used by other routers.

## Origin

- Upstream: `FaserF/ha-speedport`
- Current fork base: upstream development version `1.1.9-dev0`
- Local extension: Sagemcom XMO/SHA-512 support and optional router flyout
- License: MIT; upstream attribution is retained

The integration remains in the `speedport` domain for Home Assistant
compatibility. This fork and upstream cannot be installed side by side.
