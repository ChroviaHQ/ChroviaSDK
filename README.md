# Chrovia SDK

A Chromium kernel you can ship as your own browser product.

Kernel capabilities live in the browser, not in injected scripts. Enable them with a signed [Chrovia License](https://console.getchrovia.com), then use the JavaScript APIs and per-instance configuration to build your product.

[Website](https://console.getchrovia.com) · [SDK](https://console.getchrovia.com/products/sdk) · [Download](https://console.getchrovia.com/download) · [Studio](https://console.getchrovia.com/products/sdk#studio)

## Development Journal / 开发日志

Notes and product stories from building browsers with ChroviaSDK, published in [English](devlogs/en/) and [简体中文](devlogs/zh/).

- **Amazon Passkeys: What Cross-Border Sellers Need to Know About Account Linking, Devices, and Credential Management** — [English](devlogs/en/passkeys-for-cross-border-sellers.md) / [中文](devlogs/zh/passkeys-for-cross-border-sellers.md)
- **Building Merca with ChroviaSDK: A Browser Built for Multi-Store Commerce** — [English](devlogs/en/building-merca-with-chrovia-sdk.md) / [中文](devlogs/zh/building-merca-with-chrovia-sdk.md)

## Why not fork Chromium yourself

Starting from Chromium means maintaining multi-platform builds, integrating browser capabilities, packaging releases, and managing per-customer licensing.

Chrovia provides that layer, already integrated into Chromium:

- **Native, not injected.** Fingerprint, network, credentials, and automation capabilities run in Chromium itself.
- **One JS API.** Allowlisted Provision extension service workers use `globalThis.chrovia`; page access requires Unrestricted API. Gated namespaces are `undefined` without their Entitlement.
- **Ship it. Sell it.** Brand the binary, load your own Provision package, and distribute under a Chrovia License. Windows, macOS, and Linux share the same kernel and API, with prebuilt SDK packages currently available for Windows and macOS.
- **Pay for what you enable.** Choose platforms, Entitlements, and validity for your commercial license. Console manages the monthly lease quota.

This is a browser-product SDK, not a test driver. If you only need to script stock Chrome, Playwright or Puppeteer is enough. If you need to *ship* a Chromium product with kernel-level identity, network, and credential control, that is what Chrovia is for.

## How it fits together

```
+----------------------------------------------------------+
|  Your product                                            |
|    Provision/            always-on extensions + metadata |
|    Extended Preferences  per-instance config (optional)  |
|    Chrovia License       signed Entitlements + validity  |
+------------------------------+---------------------------+
                               |  window.chrovia.*
                               v
                    Chrovia kernel (Chromium)
```

1. **Kernel** — a Chromium-based desktop runtime. Capabilities are enforced at each feature entry point.
2. **Chrovia License** — a signed document the browser verifies with embedded public keys. Private signing keys stay on the server. A license may require a short-lived online lease.
3. **Provision** — a directory next to the executable on Windows/Linux, or at `Chrovia.app/Contents/Provision/` on macOS. Extensions under `Extensions/` load at startup. Product behavior that needs privileged APIs belongs here, not in arbitrary web origins.
4. **Extended Preferences** — a general preferences store. With Signed Extended Preferences enabled, the browser validates the signed internal section and rejects configurations that fail verification.

Missing, invalid, or unauthorized state fails closed. Gated APIs do not run.

## JavaScript API

`globalThis.chrovia` is available in allowlisted Provision extension service workers. With **Unrestricted API**, it is also available on pages and in other service workers, but not in dedicated or shared workers. Automation remains Window-only; Passwords remains restricted to allowlisted Provision extension service workers.

Each gated namespace is `undefined` until its Entitlement is granted. `chrovia.license` requires no separate Entitlement and is available whenever `chrovia` itself is exposed.

| Namespace | Entitlement | Role |
| --- | --- | --- |
| `chrovia.automation` | Automation | Deep DOM access, closed shadow roots, trusted events |
| `chrovia.network` | Network Intercept | Intercept, inspect, or cancel matching requests |
| `chrovia.messaging` | Messaging | Structured channels between pages and service workers |
| `chrovia.prefs` | Extended Preferences | Read and write Extended Preferences |
| `chrovia.passwords` | Passwords | Read and write the profile password store |
| `chrovia.ui` | Native Dialog | Native alert, confirm, and notify dialogs |
| `chrovia.process` | Process Management | Graceful instance exit |
| `chrovia.license` | — | Read license state, apply a lease, or invalidate the license |
| `chrovia.instanceMetadata` | Instance Metadata | Host, OS, hardware, and branding data |

### Messaging

```js
const CHANNEL = 'app:page-to-extension';

chrovia.messaging.on(CHANNEL, data => {
  chrovia.messaging.emit('app:extension-to-page', {
    received: data,
    at: new Date().toISOString(),
  });
});
```

### Network intercept

The `timeout` value is in seconds.

```js
chrovia.network.intercept(
  {urls: ['https://example.com/api/*'], timeout: 120},
  async request => {
    if (String(request.method).toUpperCase() !== 'POST') {
      return {};
    }
    return {cancel: true};
  },
);
```

### Automation

```js
const api = window.chrovia.automation;
const host = api.querySelector('#app');
const shadow = api.getShadowRoot(host);
const button = shadow && api.querySelector(shadow, 'button[type="submit"]');
if (button) {
  api.dispatchTrustedEvent(button, new MouseEvent('click', {bubbles: true}));
}
```

Live reference demos for these APIs ship in [Chrovia Studio](https://console.getchrovia.com/products/sdk#studio). Studio boots the same kernel the SDK ships.

## Capabilities

The capabilities below require their corresponding Entitlements. Granting an Entitlement permits use of the capability; per-instance configuration may still be required.

**Identity**

| Capability | What it does |
| --- | --- |
| Fingerprint Override | Per-instance user agent, timezone, hardware, fonts, and rendering traits |
| Instance Appearance | Name, title-bar color, and icon so instances are visually distinct |
| Instance Metadata | Expose host, OS, hardware, and brand fields to pages |
| Extended Preferences | One preferences file for identity, kernel, and product settings |
| Signed Extended Preferences | Validate the signed internal section; reject configurations that fail verification |

**Network**

| Capability | What it does |
| --- | --- |
| Network Intercept | Inspect matching requests in real time and optionally cancel them |
| Declarative Net Request | Static block / redirect / allow rules before the page loads |
| Navigation Redirect | Redirect or block navigations by URL pattern |
| System Network Override | Per-instance network path and regional egress |

**Credentials**

| Capability | What it does |
| --- | --- |
| Passwords | Read and write the instance password store from a Provision extension |
| Injected Credentials | Supply predefined credentials for Chromium autofill on matching login forms |
| Software Authenticator | Software WebAuthn / virtual passkeys, including headless |
| Password Reveal Block | Restrict password reveal and input-type changes, and hide reveal/copy controls in the password manager UI |

**Automation and control**

| Capability | What it does |
| --- | --- |
| Automation | Query through shadow roots; dispatch trusted events |
| Messaging | Page ↔ service worker channels |
| Process Management | Graceful instance exit via `chrovia.process.exit()` |
| Supervisor Process | Browser exits when the supervisor process exits |
| Native Dialog | Chromium-styled alert / confirm / notify |
| Disable DevTools | Turn off DevTools UI and remote inspection per instance |
| Unrestricted API | Extend API access to pages and other service workers; namespace-specific restrictions still apply |

## Get started

1. **Create an account** at [console.getchrovia.com](https://console.getchrovia.com/auth).
2. **Issue a trial license** in Console and select the Entitlements you want to evaluate. Trials last 30 days and are limited to one active trial per account.
3. **Download Studio** (fastest path) or the **SDK browser** for [Windows x64](https://console.getchrovia.com/download) or [macOS Apple Silicon](https://console.getchrovia.com/download).
4. **Run a gallery demo** in Studio — messaging, network intercept, credentials, software authenticator, automation, and more — on the real kernel.
5. **Ship your product** with a Provision directory in the platform-specific location below and a commercial license for the Entitlements you need.

On Windows/Linux, place `Provision/` beside the executable:

```text
YourDistribution/
  chrome.exe | chrovia
  Provision/
    manifest.json
    Extensions/
      your-product/
        manifest.json
        background.js
```

On macOS, place it inside the app bundle:

```text
Chrovia.app/
  Contents/
    MacOS/
      ...
    Provision/
      manifest.json
      Extensions/
        your-product/
          manifest.json
          background.js
```

Extension IDs are derived from directory paths relative to `Provision/Extensions/`; directory names are not themselves extension IDs. Loading defaults to component extensions.

## Built on this kernel

Products built with ChroviaSDK:

| Product | What it is |
| --- | --- |
| [Persona Hub](https://console.getchrovia.com/products/persona-hub) | Isolated multi-account browser identities |
| [美刻浏览器（Merca）](https://merca.getchrovia.com) | Multi-store operations; each store has its own node and login environment |
| [Chrovia Studio](https://console.getchrovia.com/products/sdk#studio) | Local gallery of kernel demos for SDK developers |

## Requirements

| Platform | Prebuilt SDK | Notes |
| --- | --- | --- |
| Windows 10+, x64 | Yes (`.7z`) | |
| macOS 13+, Apple Silicon | Yes (`.zip`) | Minimum version reflects the current source build configuration |
| Linux x86-64 | Not yet | License and kernel target Linux; minimum runtime requirements are not yet confirmed |

## License

Embedding and redistributing the Chrovia SDK requires a Chrovia License issued by Console. Entitlements, platforms, and validity are defined by the signed document, not by local toggles. Monthly lease quotas are managed separately by Console and enforced when leases are issued.

See the [software license](https://console.getchrovia.com/license) and [terms](https://console.getchrovia.com/terms). Commercial licensing: [support@getchrovia.com](mailto:support@getchrovia.com).

## Links

- [Chrovia](https://console.getchrovia.com)
- [SDK product page](https://console.getchrovia.com/products/sdk)
- [Downloads](https://console.getchrovia.com/download)
- [X / Twitter](https://x.com/ChroviaHQ)
- [Security](https://console.getchrovia.com/security)
