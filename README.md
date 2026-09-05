# Chrovia SDK

A Chromium kernel you can ship as your own browser product.

Kernel capabilities live in the browser, not in injected scripts. You grant them with a signed [Chrovia License](https://console.getchrovia.com), then call them from JavaScript as `window.chrovia`.

[Website](https://console.getchrovia.com) · [SDK](https://console.getchrovia.com/products/sdk) · [Download](https://console.getchrovia.com/download) · [Studio](https://console.getchrovia.com/products/sdk#studio)

## Development Journal / 开发日志

Notes and product stories from building browsers with ChroviaSDK, published in [English](devlogs/en/) and [简体中文](devlogs/zh/).

- **Building Merca with ChroviaSDK: A Browser Built for Multi-Store Commerce** — [English](devlogs/en/building-merca-with-chrovia-sdk.md) / [中文](devlogs/zh/building-merca-with-chrovia-sdk.md)

## Why not fork Chromium yourself

Starting from Chromium is a high bar: multi-platform builds, capability plumbing, a distribution story, and a way to turn features on and off per customer.

Chrovia is that layer, already wired into the kernel:

- **Native, not injected.** Fingerprint, network, credentials, and automation run in Chromium itself. Performance matches a normal browser.
- **One JS API.** Provision extensions and pages call `window.chrovia` / `globalThis.chrovia`. Ungranted capabilities are `undefined` — fail closed.
- **Ship it. Sell it.** Brand the binary, load your own Provision package, and distribute under a Chrovia License. Windows, macOS, and Linux share the same kernel and API.
- **Pay for what you enable.** Commercial licenses select platforms, capabilities, validity, and monthly lease quota.

This is a browser-product SDK, not a test driver. If you only need to script stock Chrome, Playwright or Puppeteer is enough. If you need to *ship* a Chromium product with kernel-level identity, network, and credential control, that is what Chrovia is for.

## How it fits together

```
+----------------------------------------------------------+
|  Your product                                            |
|    Provision/            always-on extensions + metadata |
|    Extended Preferences  per-instance config (optional)  |
|    Chrovia License       signed entitlements + quota     |
+------------------------------+---------------------------+
                               |  window.chrovia.*
                               v
                    Chrovia kernel (Chromium)
```

1. **Kernel** — a Chromium-based desktop runtime. Capabilities are enforced at each feature entry point.
2. **Chrovia License** — a signed document the browser verifies with embedded public keys. Private signing keys stay on the server. A license may require a short-lived online lease.
3. **Provision** — a directory next to the executable. Every extension under `Extensions/` loads at startup. Product behavior that needs privileged APIs belongs here, not in arbitrary web origins.
4. **Extended Preferences** — a general preferences store. Selected internal fields can be server-signed so instance identity cannot be edited locally.

Missing, invalid, or unauthorized state fails closed. Gated APIs do not run.

## JavaScript API

`globalThis.chrovia` is injected into allowlisted extension service workers. With **Unrestricted API** it is also present on pages. Automation is window-only; passwords is service-worker-only. Each namespace is `undefined` until the matching entitlement is granted.

| Namespace | Entitlement | Role |
| --- | --- | --- |
| `chrovia.automation` | `automation` | Deep DOM access, closed shadow roots, trusted events |
| `chrovia.network` | `networkIntercept` | Intercept, inspect, rewrite, or cancel requests |
| `chrovia.messaging` | `messaging` | Structured channels between pages and service workers |
| `chrovia.prefs` | `extendedPrefs` | Read and write Extended Preferences |
| `chrovia.passwords` | `passwords` | Read and write the profile password store |
| `chrovia.ui` | `nativeDialog` | Native alert, confirm, and notify dialogs |
| `chrovia.process` | `processManagement` | Graceful instance exit |
| `chrovia.license` | — | License snapshot, lease apply, invalidate |
| `chrovia.instanceMetadata` | `instanceMetadata` | Host, OS, hardware, and branding data |

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

Every capability is off until the license grants it.

**Identity**

| Capability | What it does |
| --- | --- |
| Fingerprint Override | Per-instance user agent, timezone, hardware, fonts, and rendering traits |
| Instance Appearance | Name, title-bar color, and icon so instances are visually distinct |
| Instance Metadata | Expose host, OS, hardware, and brand fields to pages |
| Extended Preferences | One preferences file for identity, kernel, and product settings |
| Signed Extended Preferences | Server-signed internal section; local edits do not apply |

**Network**

| Capability | What it does |
| --- | --- |
| Network Intercept | Watch requests in real time; rewrite or cancel |
| Declarative Net Request | Static block / redirect / allow rules before the page loads |
| Navigation Redirect | Redirect or block navigations by URL pattern |
| System Network Override | Per-instance network path and regional egress |

**Credentials**

| Capability | What it does |
| --- | --- |
| Passwords | Read and write the instance password store from a Provision extension |
| Injected Credentials | Pre-load logins so forms fill on first paint |
| Software Authenticator | Software WebAuthn / virtual passkeys, including headless |
| Password Reveal Block | Block reveal, type-toggle, and copy on password fields |

**Automation and control**

| Capability | What it does |
| --- | --- |
| Automation | Query through shadow roots; dispatch trusted events |
| Messaging | Page ↔ service worker channels |
| Process Management | Host app starts, stops, and observes the browser |
| Supervisor Process | Browser exits when the supervisor process exits |
| Native Dialog | Chromium-styled alert / confirm / notify |
| Disable DevTools | Turn off DevTools UI and remote inspection per instance |
| Unrestricted API | Expose `window.chrovia` on every page and worker, not only allowlisted extension service workers |

## Get started

1. **Create an account** at [console.getchrovia.com](https://console.getchrovia.com/auth).
2. **Issue a trial license** in Console. Trials last 30 days, include every capability, and are limited to one active trial per account.
3. **Download Studio** (fastest path) or the **SDK browser** for [Windows x64](https://console.getchrovia.com/download) or [macOS Apple Silicon](https://console.getchrovia.com/download).
4. **Run a gallery demo** in Studio — messaging, network intercept, credentials, software authenticator, automation, and more — on the real kernel.
5. **Ship your product** with a Provision directory beside the binary and a commercial license for the entitlements you need.

```text
YourDistribution/
  Chrovia.app | chrome.exe | chrovia
  Provision/
    manifest.json
    Extensions/
      your-product/
        manifest.json
        background.js
```

Directory names under `Provision/Extensions/` are stable extension IDs. Loading defaults to component extensions.

## Built on this kernel

The same SDK powers Chrovia's own desktop products:

| Product | What it is |
| --- | --- |
| [Persona Hub](https://console.getchrovia.com/products/persona-hub) | Isolated multi-account browser identities |
| [Merca](https://merca.getchrovia.com) | Multi-store operations; each store has its own node and login environment |
| [Chrovia Studio](https://console.getchrovia.com/products/sdk#studio) | Local gallery of kernel demos for SDK developers |

## Requirements

| Platform | Prebuilt SDK | Notes |
| --- | --- | --- |
| Windows 10+, x64 | Yes (`.7z`) | |
| macOS 12+, Apple Silicon | Yes (`.zip`) | |
| Linux x86-64, glibc 2.31+ | Not yet | License and kernel already target Linux |

## License

Embedding and redistributing the Chrovia SDK requires a Chrovia License issued by Console. Capabilities, platforms, validity, and lease quota are defined by that document, not by local toggles.

See the [software license](https://console.getchrovia.com/license) and [terms](https://console.getchrovia.com/terms). Commercial licensing: [support@getchrovia.com](mailto:support@getchrovia.com).

## Links

- [Chrovia](https://console.getchrovia.com)
- [SDK product page](https://console.getchrovia.com/products/sdk)
- [Downloads](https://console.getchrovia.com/download)
- [X / Twitter](https://x.com/ChroviaHQ)
- [Security](https://console.getchrovia.com/security)
