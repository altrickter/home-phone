# Home Phone: the setup guide

This guide links to the manufacturers' own instructions for the click-by-click parts (they keep those current; this repo can't) and adds the context, decisions, and gotchas around them. Expect about two hours the first time.

**Tip if an AI assistant is helping you:** both Claude and ChatGPT can read screenshots. When you're on a portal or adapter settings page and unsure what to pick, screenshot it and paste it in; that's far faster than describing it.

**The one rule: solve one problem at a time.** Set the phone up plugged directly into your main router first (Setup A), confirm calls work in both directions, and only then move it to the travel router (Setup B). Each step below ends with a checkpoint. Don't move on until it passes.

## Vocabulary (skip if you know VoIP)

- **VoIP** (Voice over IP): phone calls carried over the internet instead of copper phone lines.
- **SIP**: the standard protocol VoIP devices use to register with a provider and set up calls. When a guide says "SIP account" or "SIP credentials," it means the username/password/server your device uses to log in to the provider.
- **ATA** (Analog Telephone Adapter): the box that sits between an old analog phone and your network. It speaks analog on one side (dial tone, ring voltage, touch tones) and SIP on the other.
- **FXS port**: the phone jack on the ATA. You plug the phone into it.
- **DID** (Direct Inward Dial): provider-speak for "a phone number." When you "buy a DID," you're buying a phone number.
- **POP / server**: the provider's regional server your ATA registers with (e.g. `seattle.voip.ms`). Pick one near you.
- **Main account vs. sub-account**: on voip.ms your main account has its own SIP login (account number + a separate SIP password) that one device can use directly. Sub-accounts are optional extra SIP logins, one per additional device.
- **Codec**: how audio is compressed. G.711 (µ-law) is the safe default for phone calls; G.722 is "HD voice."
- **NAT**: your home router hides your devices behind one public IP. This is the usual reason VoIP registration or audio breaks; ATAs have "keep-alive" settings to deal with it.

## Step 0: Buy the parts

See [HARDWARE.md](HARDWARE.md). Minimum: phone, phone cord, ATA, ethernet cable. Add the travel router only if the phone can't be near your router.

## Step 1: Create the voip.ms account and buy a number

[VOIPMS.md](VOIPMS.md) is a map of the whole portal with every setting below in context; this is the short version.

1. Sign up at [voip.ms](https://voip.ms) and add a small balance ($15–25 is plenty; Finances → Add Funds).
2. **Buy a DID** (your phone number): DID Numbers → Order DID(s) → Local Numbers, your area code. On the order screen: plan **Per minute (Inbound)**; CallerID Name Lookup **No thanks**; **DID POP** = the server city nearest you (e.g. San Jose, Seattle, Chicago…); Routing **SIP/IAX → [main account]**. ([wiki](https://wiki.voip.ms/article/Order_a_DID_Number))
3. **Set the device password**: Account Settings → Security → **Main SIP/IAX Password**. By default this is the same as your portal login; change it to a separate strong one, because it's what the ATA will hold (together with your 6-digit account number as the username) and internet scanners guess these.
4. **Set your outbound Caller ID**: Account Settings → General → CallerID Number → "Use one of my DIDs" → your new number. Without this, outbound calls can fail with a busy signal.
5. **Check the device settings**: Account Settings → Inbound Settings: Protocol SIP, Device type "ATA device, IP Phone or Softphone." Account Settings → Advanced: NAT **yes**, codec **G.711U** checked. (Usually already default.)
6. **Set a low-balance alert**: Account Settings → Notifications → Balance Threshold $2–5 with your email. The phone silently stops working at $0; this is how you find out before that.

**Sub-account or main account?** The steps above register the ATA with your main account, which is the simplest path and what I did; voip.ms says sub-accounts are only needed when more than one device registers at the same time. *Sub-accounts* (Sub Accounts → Create Sub Account) are a separate SIP login per device. Use one if you'll add a second phone; then route the DID to the sub-account in step 2 and use its username/password in the ATA instead. Either works; the [voip.ms ATA guide](https://wiki.voip.ms/article/Grandstream_HT802v2) assumes a sub-account.

**Server matching.** The DID's POP and the server your ATA registers to should be in the same city; ideally the same hostname. Mine match exactly, and if inbound calls ever fail, a mismatch here is the first thing to check. (Careful with a lookalike: Account Settings → Default DID Routing also shows a POP picker, but that only sets defaults for numbers you buy later; the POP that matters for your existing number is under Manage DID.) ([Choosing a server](https://wiki.voip.ms/article/Choosing_Server))

Decisions to make here:
- **Voicemail:** I skipped it. A kid's phone without voicemail is simpler (and nobody checks it). If you want it, it's free: DID Numbers → Voicemail, then select it under General → Voicemail.
- **911:** see the [911 section](#911-and-emergency-calling) below. Register your address at DID Numbers → Emergency Services either way; decide now whether to pay the monthly fee.
- **International calls:** Account Settings → Account Restrictions → "Allow International Calls." If the grandparents are abroad, turn it on and enable their country. If not, leave it off; it's a nice safety net against a misdialed number.

**Checkpoint 1:** You have a balance, a number routed to your account with a POP near you, caller ID set, a SIP password set, and a balance alert. Write down: account number, SIP password (somewhere safe), server hostname, phone number.

## Step 2: Plug everything in (Setup A)

1. Phone → phone cord → the **PHONE** port on the ATA (the HT801 has one phone port and one ethernet port, so it's hard to get wrong).
2. ATA ethernet → your main router (or any ethernet jack on your home network).
3. ATA power → wall outlet.
4. Wait a minute for it to boot. Pick up the phone; you should hear a dial tone (a "local" one the ATA makes, even before it's configured).

**Checkpoint 2:** Dial tone. If silent, check the phone cord is in the phone port (not a data port) and the phone's ringer/handset switch.

## Step 3: Configure the ATA

Follow the voip.ms wiki guide for the Grandstream HT80x v2: **[Grandstream HT802v2](https://wiki.voip.ms/article/Grandstream_HT802v2)**. The HT801 v2 is identical except it has one port; the original HT801/802 use the same settings with a slightly older UI. Grandstream's own docs: [HT80x V2 User Guide](https://documentation.grandstream.com/knowledge-base/ht80x-v2-user-guide/).

The bits that guide assumes you already know:

**Finding the ATA's web page.** Pick up the phone and dial `***`. You're now in the ATA's voice menu. Dial `02` and it reads out its IP address. Type that IP into a browser on a computer on the same network. The admin password is on a sticker on the bottom of the unit (older units: `admin`). Change it.

**The settings that matter.** Tab and field names vary slightly by firmware; leave everything not listed at its default.

| Setting (FXS Port tab) | Set it to |
|---|---|
| Account Active | Yes |
| Primary SIP Server | `<city>1.voip.ms`, same city as your DID's POP (e.g. `chicago1.voip.ms`) |
| Outbound Proxy | same server |
| SIP User ID | your 6-digit voip.ms account number (or sub-account username like `123456_phone`) |
| Authenticate ID | same as SIP User ID |
| Authenticate Password | your Main SIP/IAX Password (or the sub-account's) |
| NAT Traversal | Keep-Alive |
| SIP Registration | Yes |
| Register Expiration | 2–5 minutes (short expiration keeps the connection alive through your router's NAT; mine is 5) |
| Enable SIP OPTIONS Keep Alive | Yes |
| DNS Mode | A Record |
| Preferred Vocoder | G.711 µ-law (PCMU) first; G.722 as a second choice is fine |
| Local SIP Port | something other than 5060, e.g. 5080 (see below) |
| Check SIP User ID for incoming INVITE | Yes (extra insurance against scanners; I never turned it on) |
| Allow incoming SIP messages from SIP proxy only | Yes (same) |
| Pulse Dialing | Yes, only if it's a rotary phone |

Save and **Apply**, then reboot the ATA if the Status page doesn't show "Registered" within a minute. For perspective: my adapter, exported two years in, differs from factory default in exactly five settings (server, outbound proxy, user ID/auth ID, NAT traversal, local port) plus a display name. Everything else is stock. Other voice-menu codes while you're here: `***` then `01` toggles DHCP/static; `99` is factory reset.

**Decision: SIP port.** The default local SIP port is 5060, and the whole internet scans 5060 constantly. That's what causes "ghost calls" (the phone rings, nobody's there). Moving the local port to something else (I used 5080, which voip.ms suggests; anything in 5061–65000 works) is what fixed it for me, and it's the only anti-scanner change on my adapter. The two checkboxes in the table are extra insurance recommended by voip.ms. The provider doesn't care which local port you use. See [SIP Scanner Ghost Calls](https://wiki.voip.ms/article/Sip_Scanner_Ghost_Calls).

**Decision: dialing.** By default you'll dial 10 digits (or 1+10). If you want your kid to dial 7 digits for local numbers, that's a dial plan change in the FXS port settings; it's fiddly and I didn't bother. What's worth doing: **add speed dials for the people she calls** (FXS Port → Speed Dial / `*74` style codes on the ATA, or use the phone's own memory buttons if it has them). A piece of tape on the phone with "Grandma = 1" is the actual UX.

**Checkpoint 3a:** ATA status page shows **Registered**. Dial `4443` from the house phone: the echo test should play and repeat your voice. In the voip.ms portal home page, Main Account Registration Status shows **Registered** and the server.

**Checkpoint 3b (inbound):** Call your DID from a cell phone. The house phone rings, you pick up, both sides hear each other.

**Checkpoint 3c (outbound):** Call your cell from the house phone (dial 10 digits). It rings, caller ID shows your DID, both sides hear each other.

If any of these fail, go to [Troubleshooting](#troubleshooting). Do not proceed to the travel router with a half-working phone.

## Step 4 (optional): Move to the travel router (Setup B)

Goal: the ATA keeps its ethernet connection, but to a small router that joins your home Wi-Fi. Tested on a GL.iNet Opal (GL-SFT1200), firmware 4.x.

1. Set up the Opal per its guide: [GL-SFT1200 user guide](https://docs.gl-inet.com/router/en/4/user_guide/gl-sft1200/). Join its default Wi-Fi (printed on the bottom) or plug a laptop into its LAN port, then open `192.168.8.1` and set an admin password.
2. **Internet → Repeater → Connect**, pick your home Wi-Fi (5 GHz if it reaches; it shows a "5G" badge once connected) and enter its password. The Internet page should then show the Repeater line lit and your home network's name with an IP address it got from your main router. (GL.iNet's note on this exact use: [Connect an ethernet-only device to Wi-Fi](https://docs.gl-inet.com/router/en/4/faq/produce_a_wired_connection/).)
3. Move the ATA's ethernet cable from the main router to the travel router's **LAN** port (not WAN). Power-cycle the ATA. The Internet page's "LAN Clients" count should go to 1, and **Clients** lists the ATA with its new IP.
4. **Leave the travel router's own Wi-Fi on.** I planned to turn it off (one less network), but it turns out to be the easy way back into both the router's and the adapter's settings pages later (see the gotcha below). If it bothers you, rename it and hide the SSID under Wireless → SSID Visibility rather than disabling it.
5. While you're in there, **System → Upgrade** and install the current firmware; the Opal ships with old firmware and repeater stability improved across the 4.x releases. "Keep Settings" on.
6. Put it where it lives. The travel router and ATA both need outlets; a small power strip behind the nightstand is the usual answer.

Gotchas:
- **The ATA's settings page is only reachable from the travel router's side.** In Repeater mode the Opal makes its own network (`192.168.8.x`) behind your home network, so a laptop on your home Wi-Fi can't open the ATA's IP. To get in, join the travel router's Wi-Fi (or plug into its second LAN port), then open the ATA's address, which the Opal's **Clients** page shows, so you don't need the `***` voice menu. Or temporarily move the ATA back to the main router.
- That same double NAT is exactly what the ATA's **Keep-Alive** and short registration expiry handle. If registration drops after a while, confirm those are set.
- If Wi-Fi in that room is weak, calls will break up. Prefer the 5 GHz band for the repeater link; otherwise try a mesh satellite or powerline adapter.
- GL.iNet firmware also offers "Extender"/"Access Point" style modes that bridge the ATA straight onto your home network with no double NAT. I haven't needed it; Repeater has run for two years. If you'd rather reach the ATA from anywhere in the house, that's the mode to try.

**Checkpoint 4:** Repeat 3a–3c with the phone in its final location. Then leave it for a day and check it still rings; that's the NAT-timeout test.

## Step 5: Nice-to-haves (all free on voip.ms)

- **Whitelist who can call in.** Route the DID to "Hangup" or "Busy" by default, then add CallerID Filtering rules that route specific numbers (grandparents, aunts, friends' parents) to your account. Unknown callers and spam never ring. [Whitelisting numbers](https://wiki.voip.ms/article/Whitelisting_numbers), [CallerID Filtering](https://wiki.voip.ms/article/CallerID_Filtering).
- **Bedtime hours.** Time Conditions let you route inbound calls to Hangup outside set hours. Note the portal uses Eastern time. [Time Conditions](https://wiki.voip.ms/article/Time_Conditions).
- **Call records.** The portal's CDR shows every call in and out, handy for "who did you call for 45 minutes?"
- **Low balance alert.** Account Settings → set a balance notification so the phone doesn't silently die when credit runs out.
- **Second handset.** With an HT802, a second phone in another room on the same number. With an HT801, a cheap RJ11 splitter and a second phone works for a simple shared line.

## 911 and emergency calling

Read voip.ms's current policy: [Emergency Services](https://wiki.voip.ms/article/Emergency_Services). As of mid-2026:

- **E911 service** is \~$1.50/month plus a $1.50 activation fee. Your registered address is sent to dispatch automatically.
- **Without E911**, 911 calls still connect, but voip.ms charges a **one-time $75 fee per 911 call**, and your address is not passed automatically.

I chose not to pay monthly (the point of the project is no recurring fees) but **registered the address in the portal anyway**, and made sure the adults in the house know that the cell phones are the 911 phones. That was the right trade-off for a kid's social phone in a house full of cell phones; it may not be for you. Either way, your outbound caller ID must be set to your DID for any address info to reach dispatch.

## Questions you'll probably have

These are the ones I asked (an AI assistant, mostly) while building mine.

**What does an ATA actually do?** Short version: it makes an old phone work over the internet. Longer: it has a phone jack on one side and an ethernet jack on the other; it produces the dial tone and ring voltage the phone expects, turns the phone's audio into digital packets, and logs into your VoIP provider using SIP so calls can go in and out. Longest: the phone jack is an FXS port, which emulates the phone company's side of the line (loop current, ring voltage, touch-tone detection); the network side runs a SIP client that registers with the provider's server and negotiates audio streams using a codec like G.711.

**Why an analog phone instead of a VoIP desk phone?** You can buy an IP phone that skips the ATA. The reason to use an ATA is the phone itself: a $10 thrift-store phone is the appeal for a kid, and IP phones look like office gear. Both use the same provider setup.

**Why voip.ms?** No monthly subscription: prepaid balance, about a dollar a month for a local number, about a penny a minute, and the only optional recurring fee is E911. Any SIP provider works with this hardware (Callcentric and Anveo are common alternatives); the steps have the same shape. I have no affiliation with any of them.

**Why does the server/POP matter?** Closer server, lower latency, better audio. And two places must agree: the server the ATA registers to, and the POP set on the number's routing. If they differ, outbound may work but inbound won't.

**Which port?** The ATA's *local* SIP port defaults to 5060, which the whole internet scans, producing ghost calls. Moving it off 5060 plus the two anti-scanner settings in the table above fixes it. Only the device's local port changes; the provider's side stays default.

**Do I need the travel router's own Wi-Fi?** Not for the phone: in repeater mode it joins your home Wi-Fi as a client and the ATA only uses the ethernet port. But leave it on anyway, because it's how you reach the router's and the ATA's settings pages later (they sit behind the travel router's own network). Hide the SSID if you don't want to see it.

## Different hardware or provider?

- **Other ATAs** (Cisco ATA 19x, Obihai/Poly OBi, Linksys PAP2): same fields, different names. "Proxy" = SIP server; "User ID"/"Auth ID" = account number or sub-account; "NAT Keep Alive" = Keep-Alive; "Register Expires" = expiration. voip.ms has wiki pages for most models: search `wiki.voip.ms <model>`.
- **Other providers**: identical shape (buy number → create device credentials → route number to device → set caller ID → register ATA). Use their ATA setup page for server names. One notable variant: **Twilio** can host an ATA via SIP registration and is fully API-drivable, so a developer (or their agent) can script the provider-side setup instead of clicking through a portal; it's more complex and business-oriented, and the ATA side stays manual either way, but it's a reasonable path if you live in that world.
- **Mesh satellite instead of a travel router**: skip Step 4; plug the ATA into the satellite's ethernet port.
- **Outside the US**: 911 becomes 112/999/000 etc.; emergency-call policy and number pricing differ; local number availability varies by provider.

## Troubleshooting

| Symptom | Likely cause | Check |
|---|---|---|
| No dial tone at all | Phone in wrong port, bad cord, ATA not powered | Cord in PHONE port; try another phone/cord; ATA power LED |
| Dial tone, but ATA shows "Not registered" | Wrong credentials, wrong server, router blocking | User ID = account number (not your email); password = the Main SIP/IAX Password (not the portal password); server hostname spelled right; firmware up to date; reboot ATA and router. [Registration issue](https://wiki.voip.ms/article/Registration_issue) |
| Registers, then drops after minutes/hours | NAT timeout | NAT Traversal: Keep-Alive; Register Expiration 2 min; "SIP OPTIONS Keep Alive" on |
| Inbound works, **outbound gets a busy/fast-busy** | The classic. Caller ID not set, $0 balance, international blocked, or account restrictions | Account Settings → CallerID Number = your DID; balance > 0; Account Restrictions; dial the full 10 digits. [Outgoing Calls not Working](https://wiki.voip.ms/article/Outgoing_Calls_not_Working) |
| Outbound works, **inbound doesn't ring** | DID not routed to your account, or DID POP far from the registration server | Manage DID → Routing → SIP/IAX → your account; set POP to the same server; check any CallerID Filter/Time Condition isn't sending it to Hangup |
| Rings but one-way or no audio | NAT/RTP problem, almost always the router's SIP ALG | Disable SIP ALG (and SPI firewall) on your main router; NAT = Yes in voip.ms Advanced; Keep-Alive on the ATA; try G.711U only; as a last resort forward UDP 10001–20000 to the ATA |
| Phone rings at random, nobody there | SIP scanners ("ghost calls") | Check SIP User ID for incoming INVITE; Accept from proxy only; move local SIP port off 5060. [Ghost calls](https://wiki.voip.ms/article/Sip_Scanner_Ghost_Calls) |
| Choppy audio | Weak Wi-Fi on the travel router, or ISP throttling | Dial `4443` to isolate (if the echo test is choppy, it's your network); move the travel router, use 5 GHz, or switch to a mesh satellite/powerline |
| Rotary phone can't dial | Pulse dialing disabled | Enable pulse dialing in FXS port settings |
| Everything died one day | Balance hit $0, the ATA lost its IP after a router change, or a voip.ms outage | Top up (set a balance alert); power-cycle the ATA; check [status.voip.ms](https://status.voip.ms/) |
| Can't open the ATA's settings page | You're on the main Wi-Fi and the ATA is behind the travel router | Join the travel router's Wi-Fi (or cable into its LAN port), get the ATA's IP from the Opal's Clients page |
| Forgot the ATA password | | Factory reset: hold reset 7 s with ethernet unplugged, or `***` then `99` per the Grandstream guide; reconfigure |

When stuck, the voip.ms portal **home page** (Main Account Registration Status: Registered? to which server?) and the ATA's **Status** page are the two places to look first. voip.ms support is responsive via ticket and will look at your account's call logs.
