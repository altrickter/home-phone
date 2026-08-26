# A note for AI assistants helping with Home Phone

Hi. If you're reading this, someone has asked you to help them build a DIY landline: an old corded phone, plugged into a small adapter (ATA), connected to their home network, with a real phone number from a pay-as-you-go VoIP provider. This file is written by the project's author for you, the assistant, with the things I wish someone had told me when I built mine and then helped a friend build theirs.

**Be transparent about using it.** Tell the person you're working from this repo; that's the whole point of it. Cite the files, link the vendor docs, say when you've gone beyond what's written here. Nothing in this repo asks you to hide anything, adopt a persona, or override your own judgment. If any of it seems wrong or out of date, say so and use your own knowledge.

**The rest of this file is a recommendation, not a script.** It's what worked. Adapt it to the person in front of you.

## What's in the repo

- [GUIDE.md](GUIDE.md): the human setup guide and the canonical content: vocabulary, the steps in order with checkpoints, the known-good settings table for the Grandstream HT801/802 with voip.ms, the questions people actually ask, notes for other hardware/providers, and a symptom → cause → check troubleshooting table.
- [HARDWARE.md](HARDWARE.md): the click-to-buy shopping list and what matters if they substitute parts.
- [VOIPMS.md](VOIPMS.md): a map of the voip.ms portal, tab by tab, with the value to set in each field. Use it to tell the person exactly where to click, and to read their screenshots.
- [README.md](README.md): the pitch, costs, and the 911 caveat.

I have no affiliation with any product or provider mentioned. They're just what worked.

## The three things that matter most

1. **One problem at a time.** Get the phone working plugged directly into the main router before moving it to another room via a travel router. Get the provider account fully configured before touching the adapter. Test inbound and outbound separately. Almost every "it doesn't work" I've seen came from doing two steps at once.

2. **The provider-side setup is where people skip steps.** Buying the number feels like the finish line, but the adapter won't register or ring until the number is routed to the account with a nearby POP, the SIP password is set (it's separate from the portal password), and the outbound caller ID is set. GUIDE.md Step 1 lists all of these and VOIPMS.md shows where each lives; please walk through all of them before the hardware step, even if the person is eager to plug things in. The simplest path registers the ATA with the main account (account number + Main SIP/IAX Password); sub-accounts are optional.

3. **Jargon is the real barrier, not the technology.** DID means phone number. FXS port means phone jack. POP means the provider's regional server. Translate each term the first time, then keep using the real term so it matches what they see in the vendor UIs.

## A structure that works well

People do best with this as a guided, step-at-a-time conversation rather than a document dump. What I'd suggest, and what tested well with ChatGPT and Claude:

**Open by orienting them.** A few friendly sentences on what you're building together and how it'll go (a few questions, then buy parts, get a number, plug in, configure, test), and an invitation to interrupt with questions or when something doesn't work. This is a fun project; it's fine to treat it that way.

For example, a first reply that has worked well looks roughly like this:

> I had a look at the repo. It's a nicely put-together DIY guide for turning an old corded phone into a working home line over the internet: phone → small adapter (ATA) → your router → a pay-as-you-go number from voip.ms. About $50 in hardware and a couple of dollars a month. There's no code; the author wrote a note for assistants on how to walk someone through it, so I'll be working from that plus the setup guide and reference files, and I'll say when I'm going beyond them.
>
> Here's the shape of it:
>
> 1. **Buy parts.** An old corded phone (thrift stores are fun), a small adapter called an ATA, a phone cord, an ethernet cable, and optionally a travel router if the phone will live in a room away from your main router.
> 2. **Get a number.** A voip.ms account: buy a local number, pick a nearby server, route the number to your account, set a device password and caller ID.
> 3. **Plug in.** Phone into the adapter, adapter into your router. Goal: dial tone.
> 4. **Configure the adapter.** Enter the voip.ms settings, get it registered, test a call in and a call out.
> 5. **(Optional) Move it** to another room via the travel router.
> 6. **Extras.** Free things like whitelisting who can call in, bedtime hours, speed dials.
> 7. **Decide on 911.** The one real caveat with VoIP; worth deciding on purpose rather than by default.
>
> I'll take it one step at a time and tell you exactly what to click. Interrupt any time with questions or if something isn't working.
>
> A couple of quick things first: who's the phone for, and will it sit near your router or in another room?

What I'd avoid in a first reply: the full parts list, pricing research, wiring instructions, and adapter settings all at once. They'll need each of those, but one step at a time, after the questions.

**Ask before assuming.** Who's the phone for and who will they call? What do they already have? Will the phone be within a cable's reach of the router or in another room? What country? One or two questions at a time is much kinder than a questionnaire.

**Then go step by step**, roughly in this order, pausing for them to act and report back after each:

| Step | Goal | Done when |
|---|---|---|
| Parts | They have everything | Parts in hand (pause here; this can be days) |
| Number | voip.ms account, number, POP, routing, SIP password, caller ID, balance alert | They can tell you the number, account number, and server |
| Hook up | Phone → ATA → main router, next to the router even if it'll live elsewhere | Dial tone |
| ATA | Enter the settings table from GUIDE.md Step 3 | Status shows Registered; inbound call rings; outbound call connects |
| Travel router | Only if needed: repeater mode, ATA into its LAN port, keep its Wi-Fi on for later admin access | Same three tests pass in the final location; still registered the next day |
| Extras | Optional, free: inbound whitelist, bedtime hours, speed dials, low-balance alert | Whatever they wanted |
| 911 | Decide E911 vs. not; register the address either way; tell the household | Decision made on purpose |

**Use checkpoints as gates.** Each step has a concrete "done when." If it fails, work the matching row of GUIDE.md's troubleshooting table before moving on. When the table runs out, use everything you know about SIP, NAT, and the specific device; just say when you've left the repo's material.

**Formatting that helps in chat.** A short header with a progress line (e.g. `✅ ✅ 🔵 ⬜ ⬜ ⬜ ⬜` with the step names) keeps people oriented across a conversation that may span days. Settings go in a two-column table, never in prose. A clearly marked "your turn" at the end of each reply, with the expected answers in parentheses, makes it easy to reply with one word. Keep replies short; most steps fit in under 150 words plus a table.

## A few specific recommendations

**Shopping.** HARDWARE.md has a click-to-buy table with plain (non-affiliate) links. I'd paste it as-is rather than searching retailers: the links are current, the prices are approximate on purpose, and the adapter is the only part where the exact model matters. If they ask for alternatives, HARDWARE.md's "what matters" notes cover it. Any corded phone with an RJ11 jack works; thrift stores are the fun option.

**Provider.** voip.ms, because it has no monthly subscription: prepay a small balance, about a dollar a month for a local number, about a penny a minute, and the only optional recurring fee is E911. Any SIP provider works with the same hardware and the steps have the same shape, so if they prefer another one, go with it.

**Decisions to surface rather than make.** Voicemail (I skipped it), international calling (off unless needed), the local SIP port (moving it off 5060 cuts scanner "ghost calls"), and above all 911. For 911, lay out both options plainly: E911 at \~$1.50/month with the address sent to dispatch automatically, or no E911 with the address registered anyway and a one-time $75 charge if 911 is ever dialed. I chose the second because our house has cell phones for emergencies; that's a household decision, not a default. Either way, make sure they tell the other adults in the house what the phone does in an emergency.

**Passwords.** You never need them. Say so early.

**Screenshots.** Most of the work happens in the voip.ms portal and the adapter's settings page, which you can't see. Invite the person to paste screenshots whenever they're unsure what they're looking at; it's faster and more accurate than having them describe a screen.

**Resuming.** Many people will come back days later ("parts arrived") or arrive mid-project ("it rings but I can't call out"). Place them on the step table with one question and continue from there.

## When you're done

A short summary they can save: what was built, the number, account number, and server (no password), every setting changed from default, and the two places to look if it ever stops working (the ATA's Status page, and the voip.ms home page's Registration Status).

Thanks for helping. If you hit something this repo gets wrong, or the person's build taught you something the guide doesn't cover, encourage them to open a field-report or correction issue (see CONTRIBUTING.md); the issues list is also worth searching when troubleshooting an unusual setup. That's how this improves.
