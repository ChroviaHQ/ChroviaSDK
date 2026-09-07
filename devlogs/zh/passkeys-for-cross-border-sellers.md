# 亚马逊开始用 Passkey：跨境卖家关心的账号关联、跨设备使用与托管方案

一些卖家登录亚马逊时，已经遇到了创建 Passkey（通行密钥）的提示。操作不难，但看到指纹、人脸识别、电脑 PIN 这些选项，难免会担心：平台会拿到哪些信息？几个店铺共用一台电脑，会不会出问题？

先说结论：正常的 Passkey 登录不会把指纹、人脸数据或电脑 PIN 交给亚马逊。不过，管理多个店铺时，仍有一些使用细节需要留意。

## 一、从 FIDO 到 Passkey：这不是亚马逊独有的技术

Passkey 基于 FIDO 标准，旨在减少密码泄露、撞库和钓鱼风险，同时简化登录。它的发展经历了几个阶段：

- **早期 FIDO 标准**：通过安全密钥等方式，减少对密码的依赖。
- **2018 至 2019 年**：FIDO2 逐步落地，WebAuthn 在 [2019 年成为 W3C 正式标准](https://www.w3.org/press-releases/2019/webauthn/)。其中，WebAuthn 让网站调用浏览器的认证功能，CTAP 负责客户端与安全密钥等认证器之间的通信。
- **2022 年起**：Apple、Google 和 Microsoft [共同推动无密码登录](https://fidoalliance.org/apple-google-and-microsoft-commit-to-expanded-support-for-fido-standard-to-accelerate-availability-of-passwordless-sign-ins/)，带动 Passkey 在常用设备和密码管理器中普及。

它采用公钥认证：网站保存公钥，设备或密码管理器保管私钥。登录时，设备根据网站发来的一次性请求生成签名，供网站核验，无需交出私钥。每条 Passkey 都有对应的网站范围，有助于防止仿冒页面盗用。

**目前，Chrome、Edge、Safari、Firefox 等主流浏览器都已支持 Passkey。** 对卖家来说，更值得关注的是：它会透露哪些信息、保存在哪里，以及如何管理多个店铺的 Passkey。

## 二、跨境卖家最关心的几个问题

### 1. 用指纹、人脸识别或 PIN，会把这些信息交给亚马逊吗？

**不会。正常 Passkey 登录不会上传指纹、人脸数据、设备 PIN 或私钥。**

设备或密码管理器通过指纹、人脸识别或 PIN，确认你有权使用这条 Passkey。亚马逊收到的是登录验证结果，而不是你的生物识别信息。

所以，“账号绑定了 Passkey”不等于“亚马逊绑定了你的指纹”。用同一根手指解锁不同账号的 Passkey，也不会向平台提供一个共同的“指纹编号”。

这一点可以放心。不过，你使用的登录工具仍可能透露一些信息。

### 2. 亚马逊能看出我用了什么电脑、什么工具吗？

**不会直接获得电脑的唯一编号，但可能看出登录工具的类型。**

例如，亚马逊有时能从 Passkey 的登记信息看出你用了 Windows Hello、Apple 钥匙串或其他工具，进而推测操作系统类型。如果店铺浏览器设置的系统类型与登录工具呈现的特征不一致，就需要留意。

不过，工具类型不一定代表当前电脑的系统。比如，你可以在 Windows 电脑上用 iPhone 扫码登录，也可以在不同设备上使用同步的 Passkey。

亚马逊还会知道本次使用的是哪条 Passkey、对应哪个登录账号，这是完成登录所必需的信息。

### 3. 一台电脑保存多个店铺账号，亚马逊能看到全部账号吗？

**标准流程不会向亚马逊提供全部账号的清单，但共用密码库可能导致串号。**

浏览器或密码管理器的弹窗可以列出多个可选账号，但这份列表显示在本地，不等于亚马逊网页能读取它。正常情况下，平台只收到本次选用的 Passkey 和对应账号。

更实际的风险是：你打开 A 店铺的浏览器环境，密码管理器却同时显示 A、B 两个账号。如果选了 B，就可能在 A 的环境里登录 B。

**浏览器环境分开了，Passkey 不一定也分开了。** 管理多个店铺时，要确认每个环境能使用哪些账号的 Passkey，避免选错账号、混用环境。

### 4. 有哪些常见方案？一定要用手机或指纹吗？

不一定。可选方式取决于电脑系统、浏览器和所用工具：

| 方案 | 日常怎么用 | 换机时要注意什么 |
|---|---|---|
| Windows Hello | 保存在 Windows 电脑上，用电脑 PIN、指纹或人脸识别确认 | 本地 Passkey 不会自动同步到另一台电脑 |
| Apple 密码 / iCloud 钥匙串 | 在 Mac、iPhone、iPad 上使用，以指纹、人脸识别或设备密码确认 | 开启 iCloud 钥匙串后，可加密同步到同一 Apple 账号下的受支持设备 |
| Google Password Manager（GPM） | 在 Android、Chrome 等受支持环境中保存，按提示解锁 | 在受支持的新设备上登录同一 Google 账号，完成 GPM 解锁验证后使用同步的 Passkey |
| Bitwarden、1Password 等 | 通过浏览器插件或手机应用使用密码库 | 通常支持加密同步，换机后需要解锁密码库 |
| FIDO2 硬件安全密钥 | 插入 USB 或通过 NFC 感应，按提示确认 | 携带密钥在兼容设备上使用，购买前确认型号支持 Passkey |
| 手机扫码 | 扫描电脑上的二维码，在手机上确认，通常需要开启蓝牙并保持近距离 | 用的是手机里的 Passkey，并未复制到电脑 |

注意：出现 Windows Hello 弹窗，不代表 Passkey 一定保存在 Hello 中。其他密码管理器也可能借它解锁，保存位置要看创建时选了哪个工具。

### 5. 为什么设置了 Passkey，亚马逊还要求输入验证码（TOTP）？

**在仍要求验证码的亚马逊登录流程中，Passkey 和 TOTP 是配合使用的。**

Passkey 用于登录认证，TOTP 是验证器应用中定时刷新的动态验证码，用来完成额外验证。设置 Passkey 不会自动取消账号原有的验证码要求，页面要求填写 TOTP 时，仍需按提示输入。

因此，设置成功后仍需验证码，不代表 Passkey 没有生效。保留原有验证器，按亚马逊页面提示操作即可。

### 6. 一个店铺要设置几次？主子账号、站点和电脑都要分别设置吗？

**不能只按店铺数量算，还要看登录邮箱、站点和保存方式。**

- **登录邮箱不同，分别设置。** 主账号和子账号，只要登录邮箱不同，就需要各自设置 Passkey。
- **不同站点，分别确认。** 要求独立登记的站点分别设置；支持共用的则不必重复，以实际登录提示为准。
- **本地保存，逐台设置。** 每台电脑各自使用 Windows Hello 本地 Passkey 时，需要分别创建。使用同步密码管理器、手机或安全密钥，则不一定要在新电脑上重做。

例如，1 个主账号和 2 个子账号，都需要在 2 个站点分别登记 Passkey。如果每个账号还要在 2 台电脑上各自保存本地 Passkey，设置次数就是：

**3 个账号 × 2 个站点 × 2 台电脑 = 12 次设置。**

这个次数取决于实际使用情况。同步方案可以减少换机时的重复设置，但每个独立登录账号，以及要求单独登记的站点，仍需完成首次设置。

### 7. 换电脑登录亚马逊，原来的 Passkey 怎么处理？

看保存方式，分两种情况：

- **本地保存，如 Windows Hello**：Passkey 不会自动转到新电脑，需要重新登记。换机时，确认备用登录方式可用后，可以先在亚马逊账号安全设置中解绑旧 Passkey，再用新电脑登录亚马逊并重新设置。
- **同步保存，如 GPM、iCloud 钥匙串**：在受支持的新设备上完成同步和解锁后，通常可以继续使用原有 Passkey，无需解绑重设。

账号、站点和电脑一多，需要考虑的就不只是设置次数。Passkey 由谁保管、员工能用哪些账号、换机和交接如何处理，都需要统一管理。这正是专门的托管方案要解决的问题。

## 三、两种托管方案：通用密码管理插件与卖家专用浏览器

“托管”就是由软件管理 Passkey 的保存和使用，不是把私钥交给亚马逊。选择时，要看它能否配合多店铺运营和团队协作。

### 方案一：以 Bitwarden 为代表的插件方案

Bitwarden 通过浏览器插件创建、保存和使用 Passkey，并通过加密密码库在受支持设备间同步。它不仅能管理多个账号，也提供组织共享和权限功能。

但它是通用密码管理工具，不是店铺管理工具。站点、主子账号和浏览器环境如何对应，仍需团队自行配置。安装插件，不会自动把各店铺的 Passkey 分开。

插件的接入方式也值得关注。如果通过网页脚本替换原有登录功能，可能留下网站能察觉的痕迹，Passkey 信息也可能带有管理工具的特征。是否会影响使用，需要实际测试，不能仅凭这些差异就认定插件一定会被识别或带来风险。

### 方案二：美刻浏览器的内核级方案

美刻面向跨境卖家的多账号使用需求，将 Passkey 处理接入浏览器内核，也就是浏览器内部负责认证的部分，而不是仅靠网页上的插件脚本。

主要价值有三点：

- **更贴近正常登录流程。** 网站发出标准 Passkey 请求，由浏览器内部处理，不需要在页面上另行模拟。
- **减少页面改动痕迹。** 美刻在浏览器内部处理 Passkey，无需用网页 JavaScript 替换登录功能，从源头避免这类脚本接管留下的痕迹，让登录过程更贴近浏览器原生体验。
- **更便于按店铺环境管理。** 将 Passkey 管理与店铺账号、浏览器环境结合，更贴近卖家的工作方式。账号隔离、同步、团队权限和恢复等功能，以产品实际支持为准。

美刻还参考 GPM 的特征和处理方式，让多设备使用更贴近主流同步方案，减少不必要的差异。这并不意味着 Passkey 保存在 Google，也不代表享有 Google 的安全保护、恢复服务或官方背书。采用 GPM 风格是为了提高兼容性，而不是保证“无法识别”。

两者的主要区别在于管理方式和接入位置：Bitwarden 以通用密码库为核心，美刻则更侧重卖家的浏览器环境。多店铺团队可以根据账号分工、换机和交接需求，选择适合自己的方案。

## 结语：不必担心上传指纹，更要管好店铺登录

Passkey 不会把指纹、人脸数据或电脑 PIN 交给亚马逊。对卖家来说，更值得关注的是登录工具透露的特征、账号是否混用，以及不同站点和设备上的设置与管理。

系统工具和通用密码管理器能满足许多日常需求；美刻浏览器则在内核中处理 Passkey，减少页面接管痕迹，让管理方式更贴近多店铺运营。

好的方案，不仅要让眼前的登录顺利完成，也要在日后新增店铺、更换电脑或交接工作时，让账号归属清楚、操作不易出错。

了解美刻浏览器：[https://merca.getchrovia.com](https://merca.getchrovia.com)

## 参考资料

1. [W3C：Web Authentication Level 3](https://www.w3.org/TR/webauthn-3/)：Passkey 背后的网页认证标准。
2. [W3C：Conditional UI 说明](https://github.com/w3c/webauthn/blob/main/explainers/conditional-ui.md)：浏览器显示账号与向网站披露凭证的区别。
3. [FIDO Alliance：Attestation White Paper](https://fidoalliance.org/wp-content/uploads/2024/06/EDWG_Attestation-White-Paper_2024-1.pdf)：网站可能获知哪些认证工具信息。
4. [Google：Manage passkeys in Chrome](https://support.google.com/chrome/answer/13168025)：Chrome 与 GPM 的使用说明。
5. [Google：Allow passkey reuse across your sites with Related Origin Requests](https://web.dev/articles/webauthn-related-origin-requests)：不同域名在满足条件时共用 Passkey 的方式。
6. [Apple：About the security of passkeys](https://support.apple.com/en-us/102195)：Apple Passkey 与 iCloud 钥匙串的安全说明。
7. [Microsoft：What are passkeys and why they matter](https://support.microsoft.com/en-us/windows/security/identity-signin/what-are-passkeys-and-why-they-matter)：本地保存与同步使用的区别。
8. [Bitwarden：Autofill Passkeys](https://bitwarden.com/help/storing-passkeys/)：通过浏览器扩展保存和使用 Passkey。
9. [Bitwarden：Organizations Quick Start](https://bitwarden.com/help/getting-started-organizations/)：组织共享与访问管理。
10. [Amazon：Passkeys coming to Seller Central in July](https://sellercentral.amazon.com/seller-forums/discussions/t/8cc9ea8c-6e1c-4c32-812c-28de469f3df4)：Seller Central 的 Passkey 推进安排。
11. [Amazon：Give your team secure, easy access with passkeys for secondary users](https://sellercentral.amazon.com/seller-forums/discussions/t/0afcf9c3-3203-4876-b678-e9f3b4396735)：独立子用户分别设置和使用 Passkey。
