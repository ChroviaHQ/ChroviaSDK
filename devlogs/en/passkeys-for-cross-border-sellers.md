# Amazon Passkeys: What Cross-Border Sellers Need to Know About Account Linking, Devices, and Credential Management

[简体中文](../zh/passkeys-for-cross-border-sellers.md)

Some sellers are already seeing prompts to create a passkey when signing in to Amazon. The setup is simple enough, but options such as fingerprints, facial recognition, and a computer PIN can raise questions: What information does Amazon actually receive? Could using one computer for several stores cause problems?

The short answer: a normal passkey sign-in does not send Amazon your fingerprint, facial data, or computer PIN. But if you manage multiple stores, there are still some practical details worth understanding.

## From FIDO to passkeys: this isn't an Amazon-only technology

Passkeys are based on FIDO standards. They are designed to make signing in easier while reducing the risks of password theft, credential stuffing, and phishing. The technology has developed in several stages:

- **Early FIDO standards** introduced ways to reduce reliance on passwords, including hardware security keys.
- **In 2018 and 2019**, FIDO2 began gaining traction, and WebAuthn [became an official W3C standard in 2019](https://www.w3.org/press-releases/2019/webauthn/). WebAuthn lets websites use a browser's authentication capabilities; CTAP handles communication between the client and authenticators such as security keys.
- **From 2022 onward**, Apple, Google, and Microsoft [expanded their support for passwordless sign-in](https://fidoalliance.org/apple-google-and-microsoft-commit-to-expanded-support-for-fido-standard-to-accelerate-availability-of-passwordless-sign-ins/), helping bring passkeys to everyday devices and password managers.

Passkeys use public-key authentication. The website stores a public key, while your device or password manager holds the private key. At sign-in, the device signs a one-time challenge from the website, which the website then verifies. The private key is never handed over. Each passkey is also scoped to a particular website, helping prevent phishing sites from using it.

**Major browsers, including Chrome, Edge, Safari, and Firefox, now support passkeys.** For sellers, the more useful questions are what information they reveal, where they are stored, and how to manage them across multiple stores.

## Common questions from sellers

### 1. Does using a fingerprint, facial recognition, or a PIN send that information to Amazon?

**No. A normal passkey sign-in does not upload your fingerprint, facial data, device PIN, or private key.**

Your device or password manager uses your fingerprint, face, or PIN to verify that you are allowed to use the passkey. Amazon receives authentication data it can verify, not your biometric information.

Registering a passkey with an account is therefore not the same as registering your fingerprint with Amazon. Using the same finger to unlock passkeys for different accounts does not give the platform a shared "fingerprint ID" that links those accounts.

That concern can be set aside. The authentication tool you use may still reveal other information, though.

### 2. Can Amazon tell which computer or authentication tool I'm using?

**The standard passkey flow does not directly disclose a unique computer ID, but it may reveal the type of authentication tool.**

For example, passkey registration data may sometimes indicate that you used Windows Hello, Apple iCloud Keychain, or another tool. That can provide a clue about the operating system. If a store's browser environment is configured to present one operating system while its authentication tool suggests another, that mismatch is worth noting.

However, the tool does not necessarily identify the operating system of the computer in front of you. You can scan a QR code with an iPhone to sign in on a Windows computer, for instance, or use a synced passkey across different devices.

Amazon also knows which passkey was used and which sign-in account it belongs to. That information is necessary to complete the sign-in.

### 3. If one computer holds passkeys for several stores, can Amazon see all the accounts?

**The standard flow does not give Amazon a list of every account, but a shared credential vault can make it easier to sign in to the wrong one.**

A browser or password manager may display several accounts in its account picker. That list is shown locally; it does not mean the Amazon webpage can read it. Normally, the platform receives only the selected passkey and its associated account.

The more practical risk is a mix-up: you open Store A's browser environment, but the password manager offers accounts for both Store A and Store B. Choose B, and you could end up signing in to Store B inside Store A's environment.

**Separate browser environments do not necessarily mean separate passkeys.** When managing multiple stores, check which accounts' passkeys are available in each environment so that staff do not accidentally mix accounts and environments.

### 4. What are the main options? Do I have to use a phone or fingerprint?

No. The available options depend on your operating system, browser, and authentication tool.

| Option | Everyday use | When moving to another device |
|---|---|---|
| Windows Hello | Store a passkey on a Windows computer and authorize use with a PIN, fingerprint, or facial recognition | Local passkeys do not automatically sync to another computer |
| Apple Passwords / iCloud Keychain | Use passkeys on a Mac, iPhone, or iPad, authorizing with a fingerprint, facial recognition, or device passcode | With iCloud Keychain enabled, passkeys can sync securely to supported devices using the same Apple Account |
| Google Password Manager (GPM) | Save passkeys in supported environments such as Android and Chrome, then unlock them as prompted | Sign in to the same Google Account on a supported new device and complete GPM's unlock verification to use synced passkeys |
| Bitwarden, 1Password, and similar tools | Access a credential vault through a browser extension or mobile app | These tools generally support encrypted sync; you need to unlock the vault on the new device |
| FIDO2 hardware security key | Connect via USB or tap via NFC, then confirm as prompted | Carry the key with you and use it on compatible devices; check that the model supports passkeys before buying |
| Phone-based QR sign-in | Scan the computer's QR code and approve the sign-in on your phone; this usually requires Bluetooth and close proximity | The passkey stays on the phone; it is not copied to the computer |

One detail to watch: seeing a Windows Hello prompt does not necessarily mean the passkey is stored in Windows Hello. Other password managers may use it to unlock their vaults. The storage location depends on the provider you chose when creating the passkey.

### 5. Why does Amazon still ask for a verification code (TOTP) after I set up a passkey?

**In Amazon sign-in flows that still require a code, passkeys and TOTP work together.**

The passkey authenticates your sign-in. TOTP is the rotating code generated by an authenticator app and used for an additional verification step. Setting up a passkey does not automatically remove an account's existing code requirement. If Amazon asks for a TOTP code, you still need to enter it.

Being asked for a code does not mean your passkey failed. Keep your existing authenticator and follow the prompts on Amazon's sign-in page.

### 6. How many passkeys do I need to set up? Do primary users, secondary users, marketplaces, and computers each need their own?

**The number depends on sign-in accounts, marketplaces, and storage methods, not just the number of stores.**

- **Different sign-in email addresses need separate setup.** Primary and secondary users each need their own passkey if they sign in with different email addresses.
- **Check each marketplace separately.** Some require their own registration; others may support reuse. Follow the actual sign-in prompts rather than assuming one setup covers every marketplace.
- **Local storage means setup on each computer.** If each computer uses its own local Windows Hello passkey, you need to create one on each. A synced password manager, phone, or security key may let you use an existing passkey on a new computer instead.

For example, suppose one primary user and two secondary users each need to register separately for two marketplaces. If each account also needs a local passkey on each of two computers, the total is:

**3 accounts × 2 marketplaces × 2 computers = 12 registrations.**

Your actual total will depend on your setup. Sync can reduce repeated setup when changing devices, but each independent sign-in account—and each marketplace that requires separate registration—still needs an initial registration.

### 7. What happens to my existing passkey when I switch computers?

It depends on where the passkey is stored:

- **Locally stored passkeys, such as those in Windows Hello:** These do not automatically move to the new computer, so you need to register a new one. After confirming that a backup sign-in method works, you can remove the old passkey in Amazon's account security settings, then sign in on the new computer and set up a replacement.
- **Synced passkeys, such as those in GPM or iCloud Keychain:** Once you have synced and unlocked them on a supported new device, you can generally keep using the existing passkey without removing and registering it again.

As accounts, marketplaces, and computers multiply, registration is only part of the job. You also need to decide who holds the passkeys, which accounts each employee can access, and how device changes and staff handovers work. This is where dedicated passkey management becomes useful.

## Two approaches to passkey management: password manager extensions and seller-focused browsers

Here, "management" means using software to handle passkey storage and use. It does not mean handing private keys to Amazon. The question is how well the tool fits multi-store operations and teamwork.

### Option 1: a password manager extension, such as Bitwarden

Bitwarden creates, stores, and uses passkeys through a browser extension, with an encrypted vault that syncs across supported devices. Alongside support for multiple accounts, it offers organization sharing and access controls.

But it is a general-purpose password manager, not a store management tool. Your team still needs to map marketplaces, primary and secondary users, and browser environments to the right credentials. Installing the extension does not automatically separate each store's passkeys.

The integration method also matters. If an extension overrides the page's authentication functions with JavaScript, it may leave changes that the website can observe. Passkey data may also carry characteristics of the management tool. Whether those differences affect actual use requires testing; their presence alone does not prove that an extension will be identified or cause problems.

### Option 2: Merca's browser-engine integration

Merca is built for cross-border sellers managing multiple accounts. It handles passkeys inside the browser engine—the browser's own authentication layer—rather than relying solely on extension scripts running on the page.

This approach offers three main benefits:

- **A more native sign-in flow.** The website sends a standard passkey request, and the browser handles it internally. There is no need to simulate the flow at the page level.
- **Fewer page-level modifications.** Merca does not need to replace authentication functions with webpage JavaScript. This avoids the traces left by that kind of script-based interception and keeps the experience closer to native browser behavior.
- **Management organized around store environments.** Connecting passkey management to store accounts and browser environments better matches how sellers work. The availability of account isolation, sync, team permissions, and recovery features depends on what the product currently supports.

Merca also draws on GPM's characteristics and behavior to keep multi-device use closer to mainstream sync implementations and reduce unnecessary differences. This does not mean passkeys are stored with Google, covered by Google's security or recovery services, or endorsed by Google. Following a GPM-style approach is intended to improve compatibility, not to guarantee that the tool cannot be identified.

The main distinction is where each tool integrates and how it organizes credentials: Bitwarden centers on a general-purpose vault, while Merca centers on sellers' browser environments. Multi-store teams can choose based on how they assign account access, change devices, and hand over responsibilities.

## The takeaway: focus on account management, not fingerprint uploads

Passkeys do not send Amazon your fingerprint, facial data, or computer PIN. For sellers, the more relevant concerns are what the authentication tool reveals, whether accounts can be mixed up, and how setup and access are managed across marketplaces and devices.

Built-in system tools and general-purpose password managers cover many everyday needs. Merca takes a browser-engine approach, avoiding page-level interception and organizing passkey management around multi-store operations.

A good setup should do more than get you through today's sign-in. It should keep account ownership clear and reduce mistakes when you add stores, replace computers, or hand work over to someone else.

Learn more about Merca: [https://merca.getchrovia.com](https://merca.getchrovia.com)

## References

1. [W3C: Web Authentication Level 3](https://www.w3.org/TR/webauthn-3/) — The web authentication standard behind passkeys.
2. [W3C: Conditional UI Explainer](https://github.com/w3c/webauthn/blob/main/explainers/conditional-ui.md) — The distinction between displaying accounts in the browser and disclosing credentials to a website.
3. [FIDO Alliance: Attestation White Paper](https://fidoalliance.org/wp-content/uploads/2024/06/EDWG_Attestation-White-Paper_2024-1.pdf) — What information websites may receive about authenticators.
4. [Google: Manage passkeys in Chrome](https://support.google.com/chrome/answer/13168025) — Guidance on using passkeys with Chrome and GPM.
5. [Google: Allow passkey reuse across your sites with Related Origin Requests](https://web.dev/articles/webauthn-related-origin-requests) — How sites on different domains can share passkeys when the required conditions are met.
6. [Apple: About the security of passkeys](https://support.apple.com/en-us/102195) — Passkey and iCloud Keychain security.
7. [Microsoft: What are passkeys and why they matter](https://support.microsoft.com/en-us/windows/security/identity-signin/what-are-passkeys-and-why-they-matter) — The difference between local and synced passkeys.
8. [Bitwarden: Autofill Passkeys](https://bitwarden.com/help/storing-passkeys/) — Storing and using passkeys through the browser extension.
9. [Bitwarden: Organizations Quick Start](https://bitwarden.com/help/getting-started-organizations/) — Organization sharing and access management.
10. [Amazon: Passkeys coming to Seller Central in July](https://sellercentral.amazon.com/seller-forums/discussions/t/8cc9ea8c-6e1c-4c32-812c-28de469f3df4) — Amazon's Seller Central passkey rollout announcement.
11. [Amazon: Give your team secure, easy access with passkeys for secondary users](https://sellercentral.amazon.com/seller-forums/discussions/t/0afcf9c3-3203-4876-b678-e9f3b4396735) — Setting up and using passkeys for individual secondary users.
