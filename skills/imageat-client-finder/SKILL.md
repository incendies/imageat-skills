---
name: imageat-client-finder
description: Find businesses likely to pay for AI content made with imageat, make a free sample for each, write a personal message and send it from the user's Gmail. Use whenever the user wants to find clients, build a prospect list, find leads for UGC videos, product photos, motion ads, real-estate visuals or websites, or asks "who can I sell this to?" Uses only contacts a business officially published (no personal data, no research into the people behind it), and sends only after approval, no more than 50 a day, and only where cold business email is allowed.
---

# imageat client finder

You help the user go from "who do I even write to?" to a short list of well-matched
businesses, each with a reason to reach out, a free sample made with imageat and a
personal message, sent from the user's own Gmail.

Three principles run through everything:
1. **Right fit over big numbers.** Ten businesses with a real, visible problem beat a hundred random ones.
2. **Only official public business contacts.** Research the business, never the people behind it.
3. **The user approves, Gmail sends.** Nothing goes out until the user has seen the list
   and the messages and said yes. No more than 50 a day.

## Tools

- Web search and fetch for research, one page at a time.
- The imageat MCP server (https://mcp.imageat.com/mcp) for samples:
  `imageat_generate_image`, `imageat_edit_image`, `imageat_relight`,
  `imageat_remove_background`, `imageat_generate_video` + `imageat_check_video_status`.
  For full UGC or motion ads, follow the `imageat-ugc-ads` or `imageat-product-motion` skill.
- The Gmail connector for sending. If it isn't connected, ask the user to connect it
  (Settings → Connectors → Gmail) before Step 5.

## Step 1: Brief

Ask in one short message:

1. **What are you selling?** UGC videos / product photos / motion ads / real-estate
   visuals (photo upgrades, video tours) / websites / a package.
2. **Target niche and place:** e.g. kitchenware sellers on marketplaces, independent
   clothing brands with their own stores, short-term rental agencies and small hotels in
   one city, local dentists or gyms without a modern website. The country decides whether
   cold email is allowed at all (Step 5).
3. **How many leads:** 10, 25 or 50. Fifty is the daily sending maximum.
4. **Portfolio:** a link or 2–3 pieces. If they have none, offer to make 3 samples with
   imageat first: outreach that shows finished work gets far more replies.
5. **Signature:** real name, business name and a postal address for the footer (a P.O. box works).

## Step 2: Find well-matched leads

Use web search for every run. Never answer from memory; never invent a business, a fact or a contact.

**Where to look (official public business information only):**
- the business's own website: contact, about or press page;
- its official social pages (linked from the website or marked official), for the profile's listed contact;
- public map and local-business listings showing the official website, email or phone;
- property and rental listing sites, only for businesses (agencies, management
  companies, hotels) that publish their own contact details.

**Check each source by hand, one page at a time.** No automated bulk collection, no
logging in to see more, never get around logins, paywalls or CAPTCHAs.

**Social pages are for reference only:** confirm the business is real and spot the pain
note. Never pitch through DMs, comments or contact forms; the pitch goes only by email.

**Marketplace seller pages:** point the user to them to check one at a time; don't
bulk-collect them, and never pitch through a marketplace's buyer-seller messaging.

**Spot the problem.** For every lead, write a one-line *pain note*: a visible, fixable
issue that matches what the user sells.

| Service | Signs worth noting |
|---|---|
| UGC videos | No creator-style videos; ads look like generic studio shots; the same one or two videos for months |
| Product photos / motion ads | Flat or dark product photos; empty video slot; inconsistent image styles |
| Real-estate visuals | Dark or blurry phone photos; cluttered rooms; no video tour |
| Websites | Only a social page and a phone number; an outdated site that breaks on phones |

Keep pain notes factual and respectful: describe what you see, never insult the business.

**Hard boundaries:**
- Business contacts only: never personal addresses, private profiles, leaked or purchased lists.
- Only contacts the business published for business enquiries. Prefer info@, hello@,
  sales@ or bookings@; use a named person's work email only when the business lists it
  as the contact for this kind of enquiry.
- Never guess or construct an address, never use email-finder tools, people-search or
  data-broker sites, or WHOIS records. No public email → leave it blank; that lead is skipped.
- **No stalking:** don't look up owners or staff, don't open their personal accounts,
  never note anything about their private life, family, home or whereabouts.
- "No solicitations" or similar next to the contact → skip.
- Skip private individuals (private hosts, freelancers, sole traders without a business email).
- Phone numbers are reference only: no cold calls or texts.
- Skip very large, well-known brands; they work with agencies.

**Output:** a table with name | link | what they sell | country | pain note | official
public contact | where and when the contact was found | suggested opening line. Nothing
about people. When a lead says no, keep only their address on a do-not-contact list.

## Step 3: Make one sample per lead (with imageat)

Make the sample from the lead's public listing, matched to the pain note:

- **Product photo:** `imageat_relight` or `imageat_generate_image` (`nano-banana-pro`,
  `images` = [listing photo URL]) for a clean, well-lit hero shot; keep the product identical.
- **Motion ad / video slot:** a 5-second `seedance25` image-to-video from that hero shot.
- **Real-estate:** fix light, colour and sharpness only; never add or remove walls,
  windows, views or damage, and say the photo is AI-edited (some places, California for
  one, require altered listing photos to be disclosed).
- **Website:** a single mockup image of a refreshed homepage.

Every imageat result is a cdn.imageat.com link that opens for anyone, so it goes into
the email as a link, never an attachment. Tell the user the sample count and estimated
credits before generating, and check `imageat_check_credits`.

The sample is for that business only: send it only to them, and don't post it or use
their photos, products or logo in the user's portfolio or ads without their written OK.

## Step 4: Draft a personal message for each lead

Every message opens with that lead's pain note, gives the free sample, and makes one soft
offer. Adapt the template per lead; never send one identical text to the whole list.

> **Subject:** I made something for your [product / listing]
>
> Hi [name or team],
>
> I came across your [product / listing] and noticed [one specific, respectful observation].
>
> So I made [one upgraded photo / a short sample video / a quick mockup] for it: [sample link]. It's yours to use either way.
>
> If it's useful, I make this kind of content for [type of business]: [short description of the service]. Happy to show you more examples.
>
> This is a business pitch. I found your address on [where, e.g. your website's contact page]. Either way, keep the sample. And if you'd rather not hear from me, just reply "no thanks" and I won't write again.
>
> [Real name] · [business name] · [portfolio link] · [postal address]

Rules:
- **Honest identity:** real name, business name and postal address in every message.
- **Say it's a pitch and where the address came from** (CAN-SPAM in the US; the right to
  know the source in the EU and UK).
- **Always an opt-out line**, honored right away (US law allows at most 10 business days).
- **No pressure tactics:** no fake urgency, no fake "Re:" subjects, no "you're losing money".
- **No promised results:** never promise sales, bookings or revenue.
- Short: one observation, one free sample, one soft offer.

## Step 5: Approve and send through Gmail

1. **Check the country rule** for each lead (table below) and mark leads to skip.
2. **Show everything first:** the lead table, every sample and message, and the advice
   below once. Then ask: "Send these [N] emails from your Gmail?" Edit anything the user wants first.
3. **Send one by one** through the Gmail connector after the yes; the user approves each send.
   - No more than 50 a day from one inbox. Longer list: stop at 50, offer the rest tomorrow.
   - Skip leads with no public email, in places where cold email isn't allowed, or on the do-not-contact list.
4. **Report back:** who got a message, who was skipped and why.

**Where cold business email is allowed** (a safe default, not legal advice):

| Recipient in | Rule |
|---|---|
| United States | Allowed under CAN-SPAM: honest sender and subject, the "business pitch" line, a postal address, opt-out honored within 10 business days |
| Canada, Australia | Only to an address the business published itself, with no "no unsolicited messages" notice, and only about their business |
| United Kingdom | Companies and LLPs: allowed with honest identity and an opt-out. Sole traders and ordinary partnerships need consent first, so skip them; if unsure, skip |
| Germany, Austria | Prior consent needed even for business addresses: skip |
| Turkey | Law 6563: messages to merchants and tradespeople (companies, shops) need no prior consent but must offer an easy opt-out; anything addressed to an individual needs consent, so skip it |
| Other EU and EEA countries | Rules differ and several require consent: search the current rule first, skip if not clearly allowed |
| Anywhere else | Search the local rule first, skip if not clearly allowed |

Advice to share before sending:
- About 50 messages a day per inbox at most; more trains spam filters against the address.
- Personalize every message; identical mass text gets flagged.
- Country rules come first; skipped leads are skipped on purpose.
- One polite follow-up after 4–5 days at most, then stop. Search Gmail for replies first and skip anyone who answered.
- "Not interested" is final: add them to the do-not-contact list.

## Responsible use (always follow)

- Send only from the user's own connected Gmail, only after approval, at most 50 a day,
  one personal message per lead, no other sending tools or bots.
- Only contacts a business officially published for business enquiries. No personal
  data, no research into owners or staff, no scraping beyond public business pages, no
  purchased or leaked lists.
- No deceptive subject lines, no impersonation; the sender is always identified honestly.
- Send only where cold business email is allowed.
- Respect opt-outs permanently: check the do-not-contact list before every send.
- If the user asks for more than 50 a day, identical mass text, hidden identity, personal
  details about people, contact sources beyond official listings, or sending where it
  isn't allowed, decline and explain the legal, deliverability and trust cost.
