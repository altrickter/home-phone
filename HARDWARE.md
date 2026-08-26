# Hardware

Everything here is a **guide, not a strong recommendation**, and I have no affiliation with any of these companies. These are cheap parts that worked for me; newer or different models will work too as long as they fit the category. The "what matters" notes tell you what to look for if you substitute.

## "I don't care, just tell me what to buy"

Copy-paste shopping list. Click, add to cart, done. (Plain links, no affiliate codes.)

<!-- SHOPPING-LIST-START -->
| # | Part | Buy | ~Price |
|---|---|---|---|
| 1 | **Grandstream HT801 v2** (the adapter; the one part that matters) | [Amazon](https://www.amazon.com/dp/B0DPZSNL8K) | $35 |
| 2 | **A corded phone** with an RJ11 jack. Thrift store is ideal; any of these work new | [Amazon: corded phones](https://www.amazon.com/s?k=corded+phone+rj11) | $10–30 |
| 3 | **RJ11 phone cord** (skip if the phone includes one) | [Amazon](https://www.amazon.com/dp/B00006HSK6) | $5 |
| 4 | **Ethernet cable**, any short one (skip if you have one; the travel router includes one) | [Amazon: ethernet cable](https://www.amazon.com/s?k=ethernet+cable+3ft) | $5 |
| 5 | **GL.iNet Opal GL-SFT1200 travel router** — *only if the phone will be in a different room from your router* | [Amazon](https://www.amazon.com/dp/B09N72FMH5) | $40 |

Phone service (no hardware, sign up later during setup): [voip.ms](https://voip.ms), pay-as-you-go.
<!-- SHOPPING-LIST-END -->

## Part by part

### The phone

<img src="images/phone-closeup.jpg" alt="Close-up of the push-button retro phone" width="420" align="right">

What matters: it's an **analog** phone with an **RJ11** jack (the small, 4-wire phone plug). That covers essentially every corded home phone made since the 1970s, and cordless-handset base stations too. Avoid anything described as "digital," "PBX," "ISDN," or "VoIP phone" (those are a different category, see below).

Notes:
- **Rotary phones** use pulse dialing. The HT801 supports pulse dialing but it may be off by default; look for a "Pulse Dialing" or "Enable Pulse Dialing" setting in the FXS port settings. Push-button (DTMF) phones need nothing special.
- **Very old phones** may have a hardwired cord or a 4-prong plug; adapters exist, but for a first build pick one with a normal RJ11 jack.
- The ATA provides the ring voltage. Most phones ring fine; some mechanical bell phones from the rotary era can be picky. If the bell doesn't ring, try the ATA's ring settings before giving up.

### The ATA (Analog Telephone Adapter)

What matters: a SIP ATA with at least one **FXS** port (that's the port you plug the *phone* into). Any current model from Grandstream, Cisco, Obihai/Poly, etc. will work with voip.ms. Grandstream is the cheapest with good documentation.

- **HT801 v2** (1 phone port): what I'd buy today. One line is all most households need.
- **HT802 v2** (2 phone ports): [link](https://www.amazon.com/dp/B0DR9MR18Y). A few dollars more; lets you add a second phone in another room on the same number, or a second number. The voip.ms wiki guide is written for this model, and the settings are identical to the 801.
- **HT801 (v1)**: what I actually bought. It's discontinued but still works and shows up cheap used/refurbished. Settings are the same.

Don't confuse **FXS** (phone port, what you want) with **FXO** (a port for plugging into a real phone line, which you don't have). Some ATAs have both.

### The VoIP provider

What matters: prepaid/pay-as-you-go, cheap local numbers, plain SIP so it works with any ATA, decent documentation.

I use **voip.ms**. Prepay a balance (a $15–25 top-up lasts a long time), buy a local number, done. There's no "plan." Alternatives people use with the same hardware: Callcentric, Anveo, VoIP.ms's competitors in your country. If you pick another provider, the ATA setup is the same idea, just different server names.

### The travel router (optional)

You only need this if the phone can't physically be near your main router (or any ethernet jack on your network). It connects to your home Wi-Fi and gives the ATA an ethernet port to plug into.

What matters: **"repeater," "WISP," or "client" mode** (connects to an existing Wi-Fi network as a client) and at least one **LAN ethernet port**. Nearly every travel router and many mesh satellites do this.

- **GL.iNet Opal (GL-SFT1200)**: ~$40, easy web UI, repeater mode is a first-class feature. Plenty of newer GL.iNet models (Beryl, Slate, etc.) do the same thing for more money.
- **Mesh Wi-Fi satellite with an ethernet port** (eero, Google Wifi, Deco): if you already have one in the right room, just use its ethernet port. No extra purchase.
- **Powerline ethernet adapters**: another option if Wi-Fi in that room is weak.

Tip: in repeater mode the travel router makes its own small network behind your home one, so the ATA's settings page is only reachable from the travel router's side. Keep the travel router's Wi-Fi on (hidden if you like); it's your way back in later.

## "Why not just…?"

- **…buy a VoIP desk phone instead of an ATA?** You can. An IP phone (e.g. Grandstream GXP series) plugs into ethernet directly and skips the ATA. But then you don't get the retro phone, which is half the point, and the cheap IP phones look like office equipment.
- **…use a Wi-Fi ATA?** A few exist (e.g. some Obihai models had Wi-Fi dongles). Usually more expensive than an HT801 plus nothing. The travel router approach is more flexible and more reliable.
- **…use Google Voice / an app?** Works for adults with smartphones. Doesn't give a kid a phone of her own.
- **…buy a Ooma / magicJack / kid-phone product?** Totally fine, and easier. They cost more up front and/or monthly, and you don't learn anything.

## Link hygiene

All Amazon links in this repo are plain `amazon.com/dp/ASIN` links with no referral or tracking parameters, and none are affiliate links. If a link dies, search the model name; these products get refreshed with "v2"/"v3" suffixes every few years and the setup doesn't change.
