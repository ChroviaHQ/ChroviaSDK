# Building Merca with ChroviaSDK: A Browser Built for Multi-Store Commerce

[简体中文](../zh/building-merca-with-chrovia-sdk.md)

Running one online store is straightforward enough: open a browser, sign in, get to work. Running several stores across marketplaces introduces a different set of problems. Sessions need to stay separate. Each account needs the right proxy connection. Passwords and verification codes need to be available without becoming a daily coordination exercise. And team members need access to the stores they work on, not unrestricted access to everything.

Opening more browser windows doesn't solve any of that on its own.

That's why we built Merca, a fingerprint browser and multi-store workspace for cross-border ecommerce sellers, on ChroviaSDK. It brings browser environments, proxies, sign-in credentials, and team access into one place, helping sellers keep accounts separate and reduce accidental overlap between stores.

**The idea is simple: give every store a browser environment of its own.** Then make that environment easy to use, whether you're working alone or managing a team.

For sellers, Merca is a ready-to-use product. For browser developers, it's a practical example of a different starting point: build on an existing Chromium-based SDK rather than taking on a Chromium fork before you can even begin building your product.

ChroviaSDK calls its licensed runtime capabilities Entitlements. Throughout this post, we'll name the Entitlements behind each feature so you can see what the SDK provides and what we built around it.

## A separate, consistent environment for every store

The first requirement is to keep one store's browser data out of another's.

Merca gives each store its own browser environment, with separate cookies, sign-in sessions, and caches. Downloads are organized by store, too. Opening a second store doesn't mean signing out of the first or clearing browser data before switching accounts.

Data separation is only part of the picture, though. Two windows on the same computer can still present the same device characteristics to a website.

Merca also assigns each store a distinct, persistent fingerprint configuration. Reopening a store reuses that configuration rather than creating a new device identity every time. Timezone and language settings follow the assigned proxy's location information to reduce mismatches between the browser environment and its network exit point.

The SDK capability behind this is Fingerprint Override. It applies fingerprint settings inside the browser runtime, rather than relying solely on scripts to alter a few page-visible properties. Data isolation uses Chromium's separate browser data directories; it does not require a dedicated Entitlement.

We also use Signed Extended Preferences to keep key settings under server control. The server signs the configuration, and the browser verifies it before applying it. Editing a local file isn't enough to override the store's assigned fingerprint or proxy settings.

Sellers don't need to manage those mechanisms themselves. They open a store, and Merca prepares its environment as part of that workflow.

## Bring your own proxies

A browser subscription shouldn't dictate where you buy your proxies.

**Merca lets sellers bring their own proxy connections, without a mandatory proxy bundle.** They can choose providers based on their markets and operational needs, and keep using those connections if they later move to another browser product.

Each store uses its assigned proxy. Merca also supports proxy chains for setups that need to route through a local proxy before reaching the store's proxy endpoint.

This uses the SDK's System Network Override Entitlement, which covers per-instance proxy settings and proxy chains. ChroviaSDK applies the network configuration; Merca handles proxy management, store assignments, and pre-launch checks.

The result is a store environment that includes both browser identity and network routing, rather than treating proxies as an unrelated setting users have to remember to switch.

## Catch mistakes before they become account mix-ups

Not every multi-account problem calls for more sophisticated fingerprint settings. Sometimes someone simply opens the wrong window, enters the wrong username, or reuses an IP address that was already assigned to another account on the same marketplace.

We built two checks around those everyday mistakes.

First, Merca checks its IP usage records before launching a store. If an IP has already been used for one account on a marketplace, attempting to launch a different account on that same marketplace can be blocked based on those records. This is a usage-conflict check, not a claim to know an IP address's entire history or reputation.

Second, Merca checks the account and marketplace in supported sign-in flows. If a submitted username doesn't match the store's registered account, or the user tries to sign in to a different, unassigned marketplace, Merca blocks the matching request and explains why.

The usage records and account-matching rules are Merca product logic. Inside the browser, Network Intercept lets us stop the request, while Native Dialog provides the user-facing explanation. There is no separate “IP protection” Entitlement doing the business logic for us.

Keeping accounts separate is about more than fingerprints. It's also about helping people use the right account in the right environment.

## Keep sign-in tools with the store

Managing several stores often means bouncing between seller dashboards, password managers, and authenticator apps. On a team, it can also mean waiting for someone else to send a password or verification code.

Merca brings the common sign-in tools into the store workflow:

- **Password autofill:** Configure the store's credentials and reduce repetitive typing on the corresponding sign-in pages.
- **Saved passwords and sync:** Manage saved credentials by store instead of maintaining scattered copies.
- **Two-step verification codes:** Keep the store's TOTP entries together and retrieve a current code when needed.
- **Passkeys:** Register and use passkeys on compatible websites alongside password-based sign-in.

Three SDK Entitlements provide the underlying credential capabilities. Injected Credentials preloads the store's login details. Passwords gives the product access to the instance's password store, allowing us to build synchronization around it. Software Authenticator supports passkey registration and sign-in.

Messaging connects runtime events to our extension logic—for example, passing a newly registered passkey to the product layer for storage.

TOTP management and code generation are implemented by Merca, not by a dedicated TOTP Entitlement. Together with the SDK's password and passkey capabilities, they give sellers a more coherent sign-in workflow.

The point isn't to put four unrelated tools on one screen. It's to have the right environment, connection, and sign-in tools ready when someone opens a store.

## Built for teams, not just individual operators

As a business grows, the question shifts from “How do I sign in?” to “Who can use this store, and who can change its settings?”

Merca supports store-level access assignments, with separate permissions for using a store and changing its configuration. Administrators can also restrict password reveal and developer tools for particular team members, limiting unnecessary access to sensitive information and browser controls.

Administrative actions—such as launching stores, assigning proxies, and changing member permissions—are logged for later review. These are workspace activity records, not recordings of every click someone makes inside a seller dashboard.

We also gave stores their own window names and colors. With several windows open, visible labels in the browser and taskbar help operators recognize where they're working before they click.

Merca owns the team model, permissions, and activity logs. The browser-side controls come from Password Reveal Block and Disable DevTools, while Instance Appearance provides the store-specific visual identity.

Navigation Redirect lets us point the new tab page to Merca's workspace, giving the browser a consistent product entry point. These are relatively small capabilities individually, but together they make a multi-store setup easier to navigate and manage.

## Fit store management into existing workflows

Merca isn't limited to manual operation. Its HTTP API and MCP interface let scripts and compatible tools launch and close stores, manage store configurations, and work with proxy connections.

These interfaces manage browser environments. They don't directly perform tasks inside marketplace pages.

The API, MCP integration, and browser launch logic belong to Merca. On the SDK side, Process Management supports graceful instance shutdown, and Supervisor Process allows store browsers to close when their supervising desktop app exits. Together, they help us manage the browser lifecycle without leaving it disconnected from the workspace that launched it.

## Why we built on ChroviaSDK

A multi-store browser requires much more than a store list and a Launch button.

Starting from Chromium source means taking responsibility for fingerprint changes, network behavior, credential handling, desktop builds, distribution, and ongoing maintenance of those modifications. That work is necessary, but it can consume a team long before the features that matter most to customers are ready.

**ChroviaSDK let us put more of our effort into the product.**

It provides a Chromium-based runtime with capabilities for fingerprints, networking, credentials, and browser appearance. We built Merca's desktop workspace, store model, proxy policies, team features, and bundled extensions on top of that runtime instead of creating another browser fork for Merca.

Here's the division of responsibilities at a glance:

| Product feature | ChroviaSDK Entitlement | What Merca adds |
| --- | --- | --- |
| Distinct, persistent fingerprints | Fingerprint Override | Per-store configuration and identity management |
| Separate sessions, caches, and downloads | No dedicated Entitlement | Store-specific Chromium data directories and download settings |
| Tamper-resistant environment settings | Signed Extended Preferences | Server-side configuration issuance and signing |
| Bring-your-own proxies and proxy chains | System Network Override | Proxy management and store assignments |
| IP usage conflict checks | No dedicated Entitlement | Pre-launch checks against IP, marketplace, and account records |
| Account and marketplace sign-in checks | Network Intercept, Native Dialog | Matching rules and user-facing explanations |
| Login autofill | Injected Credentials | Store credentials and marketplace login configuration |
| Saved passwords and synchronization | Passwords | Cloud synchronization around the instance password store |
| Two-step verification codes | No dedicated TOTP Entitlement | TOTP entry management and code generation |
| Passkey registration, sign-in, and storage | Software Authenticator, Messaging | Credential persistence and product integration |
| Team access and browser restrictions | Password Reveal Block, Disable DevTools | Store permissions and member-specific policies |
| Administrative activity logs | No dedicated Entitlement | Records of store, proxy, and team management actions |
| Store window names and colors | Instance Appearance | Per-store visual settings |
| Workspace as the new tab page | Navigation Redirect | Merca's own workspace interface |
| Scripted store management and browser shutdown | Process Management, Supervisor Process | HTTP/MCP interfaces, launch logic, and lifecycle coordination |

The SDK supplies browser capabilities. Merca turns them into a product sellers can use. Store management, team workflows, IP usage rules, and business permissions remain product-level decisions—and that's where a development team can create something specific to its customers.

ChroviaSDK's commercial licensing and distribution model gives other teams the same starting point: an existing browser runtime on which to build their own brand, workflows, and specialized product.

## Try Merca—or build your own browser

Merca is available for Windows and macOS, with multi-store workflows for sellers working across marketplaces such as Amazon, Shopee, and TikTok Shop.

If you're a seller looking for one place to manage store environments, proxies, sign-in tools, and team access, Merca is ready to explore.

If you're planning a browser product of your own, Merca shows what's possible with ChroviaSDK: start with the browser capabilities already in place, then focus on the experience your customers actually need.

- [Explore and download Merca](https://merca.getchrovia.com)
- [Learn about ChroviaSDK](https://console.getchrovia.com/products/sdk)
- [Download Chrovia Studio to try the SDK capabilities](https://console.getchrovia.com/download)
- Commercial licensing and partnerships: <support@getchrovia.com>
