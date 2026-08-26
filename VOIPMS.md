# voip.ms portal reference

A map of the voip.ms customer portal as of August 2026, with the settings that matter for a single home phone on an ATA. Written so a person (or an assistant reading over their shoulder) can find each setting and know what to put in it. Menu labels drift over time; if something has moved, the wiki link on each portal page ("Wiki Article", top right) is the current source.

Two account concepts to understand first:

- **Main account**: your login. It also has its own SIP credentials: SIP username = your 6-digit account number, and a "Main SIP/IAX Password" which **by default is the same as your portal login password** (Security tab). A device can register with these directly.
- **Sub-account**: an optional extra SIP login under the main account (username like `123456_phone`). voip.ms's own guidance: you need sub-accounts only if more than one device will be registered at the same time.

For one phone, **the simplest path is to register the ATA with the main account**, which is what I did. Two things follow from the password default: set a separate Main SIP/IAX Password so the adapter never holds your portal login, and if registration fails with "wrong password," the ATA is probably using the portal password after you changed the SIP one (or vice versa).

## Top navigation

| Menu | What's under it | You'll need it for |
|---|---|---|
| **Main Menu** | Home page (balance, settings summary, **registration status**), Account Information, Account Settings | Checking "is my ATA registered?"; caller ID; restrictions; notifications |
| **DID Numbers** | Order DID(s), Manage DID(s), Emergency Services, CallerID Filtering, Time Conditions, Voicemail, Phone Book, and many PBX-style features | Buying the number; routing it; 911; optional whitelist/bedtime hours |
| **Sub Accounts** | Create / manage sub-accounts | Only if you choose the sub-account path |
| **Finances** | Add Funds, billing history, call records (CDR) | Topping up; "who called whom" |
| **Support** | Tickets, live chat, wiki | When stuck |

## Main Menu → Home page

The first page after login, and the main diagnostic page. It shows:

- **Main Account Settings and Information**: account number (SIP/IAX Main Username), current balance, balance alert threshold, routing tiers, international calls on/off, CallerID Number, voicemail, "Protocol for inbound DID" (SIP) and "Device type."
- **Main Account Registration Status**: the account, the server it's registered to (e.g. `chicago1.voip.ms`), and a green **Registered** or a red not-registered. A "Refresh Registration Status" button sits above it.
- **Sub Account Registration Status**: the same, per sub-account (empty if you don't use them).

If the phone isn't working, look here first: is the status Registered, and to which server?

## Main Menu → Account Information

Read-only reference page. Useful bits:

- **SIP Username / SIP Authorization Username**: your account number, which is what the ATA uses as its user ID on the main-account path.
- **VoIP Servers**: the full POP list by region with hostnames (`atlanta1`, `chicago1–4`, `dallas1–2`, `denver1–2`, `houston1–2`, `losangeles1–4`, `newyork1–8`, `sanjose1–2`, `seattle1–3`, `tampa1–4`, `washington1–2`, all `.voip.ms`, plus Canadian and international tabs). Pick the city nearest you.
- **Dialing codes**: test numbers you can dial from the house phone once it's registered: `4443` echo/sound-quality test (the first thing to dial after Registered), `4747` touch-tone (DTMF) test, `*225` speaks your balance, `*97` your voicemail, `1-555-555-0911` plays back your caller ID and tests the E911 address *without* calling 911. Dialing format for US/Canada is 10 digits or 1+10 digits; `011`+country code for international (if enabled). 411 costs $0.99/call and is off by default.

## Main Menu → Account Settings

One page, nine tabs. What to set in each for a home phone:

| Tab | Setting | Set it to | Why |
|---|---|---|---|
| **General** | CallerID Number | "Use one of my DIDs" → your number | Outbound calls fail with a busy signal without this; also required for E911 address passing |
| General | Voicemail Associated to the Main Account | none (or a mailbox if you want voicemail) | |
| General | Record Calls / Call Transcription | No | |
| General | Dialing Mode | North America / NANPA (default for US/Canada) | |
| General | e911 Default CallerID | Leave None unless you buy E911 | |
| **Account Restrictions** | Allow International Calls | No (unless you need it) | Safety net against misdials and runaway charges |
| Account Restrictions | Allow Calls to Countries | US, Canada, Puerto Rico, Toll Free (default) | |
| Account Restrictions | Max. Call Time for US48/Canadian Calls | default (3 hours) is fine | |
| Account Restrictions | Allow 411 dialing | No | 411 costs extra |
| **Account Routing** | US / Canada / International routing | Value (default) or Premium | Value is cheapest (~half a cent/min for much of Canada); Premium (~1¢/min US) uses tier-1 carriers and is the only tier where caller ID and touch-tones are *guaranteed*. Either is fine for a home phone |
| **Security** | Main SIP/IAX Password | Set a strong, separate one; this is what the ATA uses (main-account path) | Defaults to your portal login password until you change it |
| Security | Foreign IP Guard | On (default) | Blocks logins from outside your country |
| Security | 2FA | Enable if you like | Portal login only, doesn't affect the phone |
| **Inbound Settings** | Protocol for inbound DIDs | SIP | |
| Inbound Settings | Device type | ATA device, IP Phone or Softphone | |
| **Notifications** | Balance Threshold | $2–5, with your email | The phone silently dies at $0; this is the fix |
| Notifications | 911 is Dialed | On, with your email | Good to know if a kid dials it |
| **Default DID Routing** | Per minute (Inbound); CallerID Name Lookup: No thanks; DID POP: nearest server | These are *defaults for numbers you buy later*; the number you already own is edited under Manage DIDs | |
| **Advanced** | NAT | yes | Home routers need this |
| Advanced | DTMF Mode | AUTO | |
| Advanced | Allowed Codecs | G.711U checked at minimum (voip.ms suggests allowing all unless troubleshooting) | G.711U is the safe phone-quality codec; G.722 "HD" only works between voip.ms users on the same server |
| Advanced | Encrypted SIP Traffic | No (simplest; the ATA would need matching TLS settings) | |

## DID Numbers → Order DID(s)

Pick **Local Numbers**, then country → state → area code (or city). The order screen asks:

- **Select Plan**: **Per minute (Inbound)** for a lightly used home phone. Flat rate includes ~3,500 inbound minutes/month and costs more per month.
- **CallerID Name Lookup**: "No thanks" (a per-query fee, ~$0.008, to look up *incoming* caller names). Unrelated: if you want *your* name to show on other people's phones (outbound CNAM), that's a one-time $10 under Manage DID, US local numbers only.
- **DID POP**: radio list of servers; pick the same city your ATA will register to.
- **Routing Settings → Main**: **SIP/IAX → [main account]** (or your sub-account). The other rows (IVR, Time Conditions, Voicemail, System → Hangup…) are alternatives you can switch to later for whitelisting or bedtime hours.

## DID Numbers → Manage DID(s)

The edit page for a number you already own. Same fields as the order screen: plan, POP, and the Routing Settings block, plus **Failover** routing (where calls go if the device is Busy / Unreachable / No Answer, e.g. to voicemail or a cell number; useful if the internet goes down) and **Ring Time**. This is where you fix "outbound works but inbound doesn't ring": confirm the routing row is **SIP/IAX → your account** and the POP is the same server (at least the same city) as your registration server.

## DID Numbers → Emergency Services

The E911 page. As of August 2026 (US): $1.50 activation + $1.50/month per number; without it, 911 calls still connect from any US/Canada number but cost **$75 per attempt** and your address isn't sent automatically. Enabling it: agree to the terms, fill in the address form, click **Validate**, wait for the confirmation email. Your CallerID Number must be exactly your 10-digit DID or the 911 call won't go through. Test with `1-555-555-0911`, which plays back your caller ID and the result without calling 911. See GUIDE.md's 911 section for the trade-off.

## DID Numbers → optional kid-friendly features (all free)

- **CallerID Filtering**: rules that route specific incoming numbers somewhere (e.g. to your account) while everything else goes to Hangup. This is how you whitelist grandparents and block everyone else.
- **Time Conditions**: route inbound calls differently by time of day (portal times are Eastern). Bedtime hours.
- **Phone Book**: names for numbers, used by the filters above.
- **Voicemail**: free. DID Numbers → Voicemail → Create New Voicemail Account (mailbox number, PIN, notification email; it can email you each message as an audio file). Then select it under Account Settings → General → Voicemail, or in the DID's Failover/Routing. Dial `*97` from the phone to check it. Transcription is the only paid extra ($0.05/min).

## Sub Accounts (optional path)

Sub Accounts → Create Sub Account: username (becomes `<account#>_<name>`), a strong password, Device Type "ATA device, IP Phone or Softphone", and the same NAT/codec/DTMF choices as the Advanced tab. Then route the DID to the sub-account instead of the main account, and use the sub-account username/password in the ATA.

## Finances

**Add Funds** (the link next to your balance on the home page also goes here). A $15–25 top-up lasts a long time at ~1¢/minute. **Call Detail Records** under Finances shows every call in and out with duration and cost.

## Quick checklist: minimum viable portal setup

1. Balance above $0, with a low-balance email alert set.
2. A local number (DID), per-minute plan, POP in your nearest city.
3. The DID's Routing set to SIP/IAX → your main account (or sub-account).
4. Account Settings → General → CallerID Number = the DID.
5. Security → Main SIP/IAX Password set (this, plus your account number, goes into the ATA).
6. Inbound Settings: SIP, ATA device. Advanced: NAT yes, G.711U allowed.
7. Emergency Services: address registered; E911 decision made.

## Network requirements (for router/firewall questions)

From the voip.ms firewall article. Home routers normally need none of this opened, but if calls register then fail, or audio is one-way:

- **Disable SIP ALG** (and "SPI firewall" if present) on your router. This is the single most common cause of one-way audio.
- Ports, all UDP, outbound and return traffic: SIP **5060** (5060–5080), RTP audio **10001–20000**. TLS-encrypted SIP uses TCP 5061, not needed for this project.
- NAT = Yes on the voip.ms side and Keep-Alive on the ATA are what keep a home-router connection alive; "registers then drops after a while" means one of them is off.
- Bandwidth: a G.711 call is roughly 90 kbit/s each way; choppy audio is almost never bandwidth on a modern connection, it's Wi-Fi or a throttling ISP. The `4443` echo test isolates whether the problem is your side or the far end.

## voip.ms wiki: articles worth knowing

The wiki ([wiki.voip.ms/article/Welcome](https://wiki.voip.ms/article/Welcome)) is extensive and current; every portal page has a "Wiki Article" link to its own page. The ones relevant to this project:

| Topic | Article |
|---|---|
| First-time setup sequence | [Getting Started](https://wiki.voip.ms/article/Getting_Started) |
| Buying a number | [Order a DID Number](https://wiki.voip.ms/article/Order_a_DID_Number) |
| Editing a number (routing, POP, failover) | [Manage DID](https://wiki.voip.ms/article/Manage_DID) |
| Picking a server | [Choosing Server](https://wiki.voip.ms/article/Choosing_Server) |
| Sub-accounts (when you need them) | [Sub Accounts](https://wiki.voip.ms/article/Sub_Accounts) |
| Every Account Settings tab explained | [Account Settings](https://wiki.voip.ms/article/Account_Settings) |
| ATA guides (Grandstream HT802 v1, HT802v2, OBi300, others) | [ATA Devices](https://wiki.voip.ms/article/ATA_Devices) · [HT802v2](https://wiki.voip.ms/article/Grandstream_HT802v2) · [HT802 v1](https://wiki.voip.ms/article/Grandstream_HandyTone_802_-_HT802) |
| 911 | [Emergency Services](https://wiki.voip.ms/article/Emergency_Services) |
| Caller ID and CNAM | [Caller ID](https://wiki.voip.ms/article/Caller_ID) |
| Voicemail | [Voicemail](https://wiki.voip.ms/article/Voicemail) |
| Whitelisting callers | [Whitelisting numbers](https://wiki.voip.ms/article/Whitelisting_numbers) · [CallerID Filtering](https://wiki.voip.ms/article/CallerID_Filtering) |
| Bedtime hours | [Time Conditions](https://wiki.voip.ms/article/Time_Conditions) |
| Value vs Premium routes | [Value vs Premium](https://wiki.voip.ms/article/Value_vs_Premium) |
| Test numbers and dialing format | [Dialing Codes](https://wiki.voip.ms/article/Dialing_Codes) |
| Won't register | [Registration issue](https://wiki.voip.ms/article/Registration_issue) |
| Can't call out | [Outgoing Calls not Working](https://wiki.voip.ms/article/Outgoing_Calls_not_Working) |
| Doesn't ring | [Incoming Calls not Working](https://wiki.voip.ms/article/Incoming_Calls_not_Working) · [DID Troubleshooting](https://wiki.voip.ms/article/DID_Troubleshooting) |
| One-way / choppy audio / echo | [Call quality issues](https://wiki.voip.ms/article/Call_quality_issues) |
| Ghost calls from scanners | [SIP Scanner Ghost Calls](https://wiki.voip.ms/article/Sip_Scanner_Ghost_Calls) |
| Router ports | [Firewall](https://wiki.voip.ms/article/Firewall) |
| Outages | [status.voip.ms](https://status.voip.ms/) |

Note on the ATA guides: the wiki's HT802 pages are written around a sub-account and a Toronto server; the field values are the same for the HT801 and for the main-account path (use your account number as the SIP User ID and your nearest server). The v1 guide's Register Expiration is 5 minutes, the v2 guide's is 2; either works.
