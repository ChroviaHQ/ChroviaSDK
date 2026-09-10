# What Is a Dedicated Static Native IP? And What Is an ISP IP?

[简体中文](../zh/dedicated-static-native-and-isp-ips.md)

The IP you buy may have passed through several resellers before reaching you. A large company leases addresses to a provider, that provider supplies another business, and that business sells the service to you. The company taking your payment may not run the network. This supply chain matters when you are buying an IP advertised as “dedicated,” “static,” “native,” or “ISP.”

## How an IP reaches you

IP addresses are allocated and registered by specialist organizations. Those responsible for different regions are called Regional Internet Registries, or **RIRs**—a little like offices that manage telephone numbers. Telecom operators, cloud providers, and other companies obtain groups of addresses, called **address blocks**, to use themselves or rent to others. They may receive an allocation, acquire existing addresses from another company, or rent them.

Providers can rent out these IPs with servers or offer proxy services. A **proxy** acts as a middleman between you and a website. When you open your seller dashboard through it, the website normally sees the proxy’s address, known as the **exit IP**. It is a little like the number shown on caller ID.

There are wholesalers and retailers in this business, too. A provider can sell directly to customers or supply another company that resells the service:

Address holder → network provider → wholesaler → reseller → end user

Some IPs pass through only one or two layers; others change hands several times. For ordinary customers, “buying an IP” usually means renting its use. After you cancel, the provider can assign it to someone else.

## Dedicated: reserved for you

A dedicated IP is an exit address reserved for you during the agreed service period, rather than assigned to other customers at the same time.

Having your own proxy username and password does not prove the IP is dedicated. Company employees can have separate phone extensions while their outgoing calls all display the same main number.

This is difficult to verify independently. Public tools can show registration details and some historical records, but not which customers the provider has assigned an address to. You are largely relying on the provider’s reputation. Buying directly from the company operating the network cuts out middlemen and reduces opportunities for a shared address to be resold as dedicated. It still does not prove that you are the only customer using it.

If having an IP to yourself is particularly important, you can rent a server from a reputable cloud provider such as AWS, Google Cloud, or Microsoft Azure, request a public IP reserved for your use, and set up your own proxy.

This requires configuring and maintaining it yourself, but it lets you bypass opaque proxy-reseller chains. IPs set up this way are classified as data center (hosting) IPs.

## Static: keeping the same address

A static IP stays the same under the agreed conditions of your service; a dynamic IP may change. It is like keeping one phone number versus being assigned a different one later.

Home broadband commonly uses dynamic addresses, and residential proxy services often have changing exit IPs. Reconnecting a home router may change its address. If the household device providing the proxy goes offline, that exit may become unavailable. When this happens while you are processing orders, you may need to reconnect, and the website may ask you to verify your login again.

Some home connections keep their addresses for a long time, and some plans offer static IPs, so “dynamic” does not necessarily mean unreliable. For sellers who regularly use a dashboard, a static IP can reduce the disruption of frequent exit-address changes. It does not guarantee an uninterrupted connection.

Static does not mean dedicated, either: several customers can share the same fixed address.

## Native: matching the registration and exit location

“Native IP” has no single industry-wide definition. Providers generally use it to mean that an address’s registration details and location lookup results match the country where the connection exits.

Suppose you get a phone number in New York and keep it after moving to Los Angeles. A lookup may still place the number in New York. An IP’s registered location and the place where it is actually used can differ in much the same way.

Websites commonly use **IP geolocation databases** to estimate where a visitor is connecting from. These work like IP location directories, but different websites may use different sources, and the records may be out of date.

For example, you might pay for a US proxy with a server in the United States, yet a website still places its address block in another country. You could see the wrong region, language, or currency, or face an extra verification step because the login location looks unusual. This is the main practical problem associated with many “non-native” addresses.

Checking the RIR alone is not enough. Each RIR covers a broad region rather than a single country, and registration does not prove where a server is located. You can compare a few IP lookup services before buying, but pay particular attention to how the platform you use identifies the address.

## ISP IP: a telecom address is not necessarily a home connection

ISP stands for Internet Service Provider. AT&T, Verizon, and T-Mobile are familiar U.S. examples; all three offer home internet services.

If an address block is registered to a telecom company, an IP lookup may show that company’s name, even when the addresses are used in a data center.

A carrier can use its addresses for home broadband or for servers. Knowing that a phone number belongs to AT&T does not tell you whether it is being used at home or in an office.

In the proxy market, “ISP IP” often refers to a proxy running on a server with an ISP-associated address. Some providers call it a “static residential proxy,” although the traffic does not necessarily pass through a home broadband connection.

Buyers are interested in these addresses because websites may consider the company and network type associated with an IP. Some websites may trust an ISP-associated address more than one clearly belonging to a cloud provider. But they may also consider past abuse, whether they detect a proxy, and unusual account activity. The carrier’s name in a lookup does not guarantee that the IP has a good reputation or that you will avoid verification checks.

## Merca recommends keeping your IP under your own control

Merca Browser recommends buying or setting up your proxy independently and managing the IP service yourself, then connecting it to Merca, rather than buying an IP bundled with the browser.

That way, you know where the IP comes from and who provides the service, and you control the provider account and renewals. If you later switch to other software, you can keep using the same IP. Your network service does not tie you to the browser.

Merca does not sell proxies or require you to use a particular network provider. We do not need to gloss over an IP service’s limitations to sell it, which lets us share this information more candidly.

## References

- [IANA: Number Resources](https://www.iana.org/numbers)
- [RIPE NCC: IP address and ASN transfers](https://www.ripe.net/languages/en/transfers/)
- [ARIN: Registration information and physical-location limitations](https://www.arin.net/about/relations/law_enforcement/)
- [MaxMind: User context and IP usage classifications](https://support.maxmind.com/knowledge-base/articles/maxmind-user-context-data)
- [Merca Browser: Independent network-provider selection](https://merca.getchrovia.com/zh)
