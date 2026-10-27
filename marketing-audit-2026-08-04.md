# Marketing Site Audit — fogartyholdings.com

**Date:** 2026-08-04
**Ground truth as of:** 2026-08-03
**Scope:** all 22 `.html` files in the repo root + `barbarons/invite/index.html`, `sitemap.xml`, `.well-known/apple-app-site-association`
**Method:** every file read in full; claims compared bidirectionally against the supplied ground truth (stale → overclaim → unshipped)
**No HTML files were modified.** This is a report only.

> **Note added 2026-08-13 — superseded copy.** The suggested "🔐 Face ID protection" replacement block under **H-2** states the app lock unconditionally ("Your scan history is locked behind Face ID"). NetScanPro 1.11.9 adds a Settings toggle that makes the lock optional (default ON at the time; since 1.12.1 on 2026-10-17 it starts off on new installs), so that copy must not be reused as written. The same caution applies to every figure quoted below — prices most of all: NetScanPro’s Pro unlock moved from $6.99 to $9.99 on 2026-08-21, so no price, count or limit in this report should be reused as current without re-verifying it first. The audit below is unchanged as a record of what was true on 2026-08-04.

> **Note added 2026-09-04 — superseded platform finding.** **H-4**, the NetScanPro half of the summary line at :36, and the `netscanpro.html` row at :526 all record iPad support as not shipped, and the H-4 fix removed "or iPad" from the Restore Purchase answer. NetScanPro 1.11.15 and 1.11.16 ship native iPad support, so that finding is reversed and the answer names iPhone and iPad again. The **L-1** open question at :270 and :529 — whether CertFlash is iPhone-only — is closed: CertFlash is iPad-native, so the iPad line it flags stands as written. The audit below is unchanged as a record of what was true on 2026-08-04.

There are no shared includes, nav partials, or footer partials in this repo — every page is a standalone file with its nav, footer, and `<style>` block inlined. Any fix has to be applied file-by-file.

---

## 1. Summary

| App | Page file(s) | HIGH | MEDIUM | LOW |
|---|---|---:|---:|---:|
| CertFlash | `certflash.html` | 1 | 2 | 1 |
| Referra | `referra.html` | 5 | 4 | 2 |
| NetScanPro | `netscanpro.html`, `netscanpro-field-techs.html`, `best-private-network-scanner-iphone.html` | 5 | 3 | 1 |
| PrepPeriod | `prepperiod.html` | 1 | 4 | 1 |
| Field Stub | `fieldstub.html` | 1 | 1 | 1 |
| Remindfully | `remindfully.html` | 0 | 0 | 2 |
| Bar Barons | `bar-barons.html` | 0 | 3 | 1 |
| Digital Legacy Vault | `digital-legacy-vault.html` | 0 | 1 | 1 |
| — (cross-page) | `index.html`, `services.html`, `sitemap.xml` | 0 | 2 | 4 |
| **Total** | | **13** | **20** | **14** |

**The three things to fix first:**

1. **`netscanpro-field-techs.html` is a whole page built on an unshipped feature.** Multi-network organization and device reattachment are on your "must not appear" list; the page's title, H1, meta description, JSON-LD, and four of its six feature cards are about exactly that. The same claim has also leaked into `netscanpro.html` and `best-private-network-scanner-iphone.html`.
2. **Referra's Pro tier advertises three things that don't exist** (Data export, Tags and filters, Advanced analytics), and gates analytics that ship free. This is the exact overclaim you corrected in the 1.6.1 App Store metadata pass — the website still carries it.
3. **PrepPeriod claims iPad sync** in the privacy section, and **NetScanPro implies iPad support** in its FAQ. Both are on the removed/not-shipped list.

---

## 2. HIGH severity — overclaims, unshipped features, wrong prices, wrong platform claims, CySA+

### H-1 · NetScanPro · `netscanpro-field-techs.html` · entire page · UNSHIPPED

This page exists to sell multi-network organization and device reattachment, both of which are on the "NOT SHIPPED — must not appear" list. It is not a copy nit that can be patched line by line; the page's premise is the unshipped feature. It is also in `sitemap.xml` (line 94) and linked twice from `netscanpro.html` (lines 716, 823).

| Line | Offending text |
|---|---|
| 15 | `<title>NetScanPro for Field Techs and MSPs — Multi-Site Network Scanner for iOS \| Fogarty Holdings</title>` |
| 16 | `"iOS network scanner built for site visits. Keep look-alike client networks separate, document each site with PDF reports…"` |
| 23–24, 31–32 | og:title / og:description / twitter:title / twitter:description repeat the same claim |
| 46 | JSON-LD `description`: `"Keep look-alike client sites separate with manual networks and scan pinning, document each site with per-network PDF reports…"` |
| 485 | `<h1 class="hero-title">One scanner. Every client site. <span>Kept separate.</span></h1>` |
| 502 | `"NetScanPro fixes this by letting you draw the boundary yourself: create a named network for each site, pin your scan to it, and the records stay separate no matter how identical the sites look."` |
| 516–517 | Feature card **"Manual networks and pinning"** — `"Create a network per client site and pin scans to it. Look-alike subnets never merge. Switching sites is one tap."` |
| 522–523 | Feature card **"Move devices"** — `"Inherited a merged record? Create the second site and move its devices over. Scan history stays where it happened."` (device reattachment — unshipped) |
| 528–529 | Feature card **"Per-site documentation (Pro)"** — `"Export a PDF report per network…"` (PDF export ships; *per-network* does not) |

**Suggested action:** unpublish. Delete the file, remove the `<url>` block at `sitemap.xml` line 94, and remove the two inbound links in `netscanpro.html`. If you want to keep an MSP/field-tech landing page, rebuild it around what actually ships — on-device-only scanning (the "where does our data go?" answer), the severity-ranked security summary, PDF export, and the 25-port scan — with no per-site/per-network framing. Only the "Client-safe by design" card (line 541) and the pricing card (line 547) survive as-is.

---

### H-2 · NetScanPro · `netscanpro.html` line 715–716 · UNSHIPPED

**Offending text (full feature card):**
> **Every network, kept separate**
> `Each Wi-Fi network keeps its own device list and scan history. Create networks manually and pin scans to a specific one, so two sites that look identical (same subnet, same gateway) never merge into one record. Move devices between networks to reorganize your catalog. Field tech or MSP? See how NetScanPro handles multi-site work →`

Three unshipped features in one card: multi-network organization, manual network creation/pinning, and device reattachment.

**Suggested replacement:** delete the card entirely (including the trailing link to `netscanpro-field-techs.html`). If you want to keep six cards in the grid, replace it with something that ships — e.g.:

> 🔐 **Face ID protection**
> Your scan history is locked behind Face ID, so the device list for your home or a client's office isn't readable by anyone who picks up your phone.

---

### H-3 · NetScanPro · `netscanpro.html` line 822–823 · UNSHIPPED

**Offending text:**
> **Can I use it across multiple sites or client networks?**
> `Yes. Each network keeps its own separate device list and scan history. You can create networks manually and pin a scan to a specific one, so two sites that share the same subnet and gateway stay separate instead of merging. Pro adds a PDF report per network you can hand off. And because nothing ever leaves your phone, scanning a client network doesn't send their data anywhere.`

**Suggested replacement:**
> **Can I use it on a client's network?**
> `Yes — and it's a good fit for it, because nothing ever leaves your phone. There's no account, no cloud, and no analytics, so scanning a client's network doesn't send their data anywhere. Pro adds a PDF report you can hand off after the visit.`

---

### H-4 · NetScanPro · `netscanpro.html` line 848 · PLATFORM (iPad not shipped)

**Offending text:**
> `Tap "Restore Purchase" inside NetScanPro on the new device. Your Pro entitlement follows your Apple ID, so any iPhone or iPad signed in gets Pro features automatically.`

iPad support is on the not-shipped list; this sentence tells a buyer their purchase works on iPad.

**Suggested replacement:**
> `Tap "Restore Purchase" inside NetScanPro on the new device. Your Pro entitlement follows your Apple ID, so any iPhone signed into the same account unlocks Pro automatically.`

---

### H-5 · NetScanPro · `best-private-network-scanner-iphone.html` lines 84, 378, 421, 488 · UNSHIPPED

The multi-network claim has leaked into the comparison article in four places, including inside the FAQPage structured data — so it is being served to Google as a rich result.

| Line | Offending text |
|---|---|
| 378 | Comparison table, "Multi-network" row, NetScanPro column: `Yes.` + `Separate history, pin scans per site` |
| 421 | `"It also keeps a separate device list and history per network, which suits anyone documenting more than one site."` |
| 84 | **JSON-LD FAQPage answer:** `"Yes. NetScanPro keeps a separate device list and scan history for each network and lets you pin a scan to a specific network, so two sites that share the same subnet and gateway do not merge into one record. That makes it useful for field techs and MSPs documenting multiple client networks."` |
| 488 | Visible FAQ answer, same question: `"NetScanPro keeps a separate device list and scan history per network and lets you pin a scan to a specific network, so two sites with the same subnet and gateway stay separate. That is useful for field techs and MSPs."` |

**Suggested action:** drop the "Multi-network" row from the comparison table entirely (it is a differentiator you don't have), delete the claim from line 421, and replace the FAQ question in **both** line 84 and line 488 with a question you can answer honestly — e.g. *"Does it work without an account or a subscription?"* → *"Yes. NetScanPro has no account, no server, and no subscription. Pro is a one-time $6.99 unlock, and everything — scanning, identification, port checks — happens on the phone."*

> Note: the JSON-LD answer and the visible answer must be changed together. Leaving one behind creates exactly the schema/copy mismatch flagged in section 5.

---

### H-6 · Referra · `referra.html` line 767 · OVERCLAIM (never shipped)

**Offending text:** Pro Annual pricing card, feature list item:
> `<li>Data export</li>`

Export was never shipped. Advertised inside a paid tier, this is a refund/App-Store-review risk.

**Suggested replacement:** delete the line. To keep the Pro card at five bullets, replace with `<li>Everything in Free, unlimited</li>`.

---

### H-7 · Referra · `referra.html` line 768 · OVERCLAIM (never shipped)

**Offending text:**
> `<li>Tags and filters</li>`

Never shipped.

**Suggested replacement:** delete the line. (Note: line 671 also says *"Tag by category and organization"* — that one is defensible if it refers to the shipped category picker, but it reads as the unshipped tagging feature. See M-5.)

---

### H-8 · Referra · `referra.html` line 766 · OVERCLAIM (feature is free, sold as Pro)

**Offending text:**
> `<li>Advanced analytics</li>`

Analytics ships in the **free** tier. Selling it as a Pro benefit misrepresents what the $39.99 buys.

**Suggested replacement:** delete the line and replace with `<li>No contact limit, ever</li>`.

---

### H-9 · Referra · `referra.html` line 753 · OVERCLAIM by implication

**Offending text:** Free tier feature list:
> `<li>Core analytics</li>`

"Core" implies a reduced analytics set on Free with more behind Pro. Free gets the full analytics.

**Suggested replacement:**
> `<li>Full analytics and leaderboard</li>`

> **Together, H-6 through H-9 mean the Pro cards should say only one thing:** the free tier caps you at 25 contacts and Pro removes the cap. Everything else — analytics, interactions, reminders — is free. A corrected Pro Annual card would read: *Unlimited contacts · Everything in Free · No contact limit, ever · Save 33% vs. monthly · Cancel anytime.*

---

### H-10 · Referra · `referra.html` — whole file · MISSING PLATFORM CLAIM (iPad shipped in 1.6)

**The word "iPad" does not appear anywhere in `referra.html`.** Nor does it appear in `referra-privacy.html` or in Referra's card on `index.html` (lines 605–613).

There is no "iPhone only" or "designed for iPhone" text to correct — the problem is silence, not a wrong statement. Referra is the only one of your eight apps with full iPad support (adaptive analytics grid, NavigationSplitView contacts, an iPad screenshot set) and the site says nothing about it.

There is also **no `requirements` / `compatibility` block and no JSON-LD at all** on this page, so there is no `operatingSystem` field to correct either (see M-8).

**Suggested additions:**

- Hero eyebrow, line 635 — change `iOS App · Now Available` to `iPhone + iPad · Now Available`
- Meta description, line 16 — append: `Now on iPhone and iPad.`
- New feature card in the features grid (after line 702):
  > 📱 **Built for iPhone and iPad**
  > `Referra runs natively on both. On iPad the analytics grid expands to use the full screen and your contact list sits alongside the detail view, so reviewing your referral sources on a bigger screen actually feels like a bigger screen.`
- When you add JSON-LD (M-8), set `"operatingSystem": "iOS, iPadOS"`.

---

### H-11 · PrepPeriod · `prepperiod.html` line 804 · PLATFORM (iPad deliberately removed)

**Offending text:**
> `Everything PrepPeriod records lives on your own devices and syncs between your Apple Watch, iPhone, and iPad through your personal iCloud account. Fogarty Holdings never sees, stores, or transmits your students' data.`

iPad support was deliberately removed. A teacher reading this could buy expecting to write up incidents on a classroom iPad.

**Suggested replacement:**
> `Everything PrepPeriod records lives on your own devices and syncs between your Apple Watch and iPhone through your personal iCloud account. Fogarty Holdings never sees, stores, or transmits your students' data.`

This is the only iPad reference in `prepperiod.html`. `prepperiod-privacy.html` is clean.

---

### H-12 · Field Stub · `fieldstub.html` · 8 locations · PRICE MISMATCH

Ground truth says **$39 one-time**. Every price on the page says **$39.99**.

| Line | Offending text |
|---|---|
| 16 | meta description — `One-time $39.99 unlock for Pro features.` |
| 24 | og:description — same |
| 32 | twitter:description — same |
| 54 | JSON-LD — `"price": "39.99",` |
| 641 | `…for $39.99, once.` |
| 746 | `<div class="pricing-amount">$39.99</div>` |
| 773 | `<div class="feature-name">Why $39.99?</div>` |
| 774 | `Field Stub is a one-time $39.99 — no annual renewal, no per-seat fees…` |
| 820 | `Free for up to 5 clients. Unlock Pro for $39.99 — once, never again.` |

**Suggested action:** confirm the App Store price first. If the shipped price really is $39.00, change all nine occurrences (the JSON-LD value becomes `"39.00"`). If your ground-truth note was shorthand for $39.99, no change is needed — but this is the kind of discrepancy that should not be left ambiguous, since the page states a specific dollar figure nine times. I have not assumed either way.

The free-tier cap (5 clients, line 734) and the three Pro gates — unlimited clients (749), PDF reports (750), on-device voice transcription (751) — all match ground truth exactly. No iPad claim anywhere in the file.

---

### H-13 · CertFlash · site-wide · MISSING CySA+ EXAM-VERSION CLAIM

I checked every mention of CySA+ across all 22 HTML files, `sitemap.xml`, and all JSON-LD blocks.

**Result: there is no stale reference to correct — and no exam-version claim at all.** CySA+ appears in exactly eight places, and every one of them is a bare deck-name mention:

| File | Line | Text |
|---|---|---|
| `certflash.html` | 16 | meta description — `Study for CompTIA A+, Network+, Security+, CySA+, Linux+…` |
| `certflash.html` | 24 | og:description — same list |
| `certflash.html` | 32 | twitter:description — same list |
| `certflash.html` | 46 | JSON-LD description — `Seven decks — A+, Network+, Security+, CySA+, Linux+, Cloud+, and PenTest+` |
| `certflash.html` | 688 | hero description — same list |
| `certflash.html` | 737 | deck tile — `<div class="deck-name">CySA+</div>` |
| `certflash.html` | 858 | Pro pricing bullet — `Unlocks Network+, Security+, CySA+, Linux+, Cloud+, PenTest+` |
| `certflash.html` | 888 | FAQ answer — `A one-time $5.99 purchase unlocks the Network+, Security+, CySA+…` |
| `index.html` | 649 | venture card — `…master all seven CompTIA exams: A+, Network+, Security+, CySA+, Linux+, Cloud+, and PenTest+.` |

No exam code (`CS0-002`, `CS0-003`, or any other) appears anywhere in the repo. No objectives revision, no "updated for", no exam-version wording of any kind — for CySA+ or for any other deck.

**This is the missed selling point you flagged.** Your freshest, most defensible claim is invisible. Recommended additions (all net-new copy — nothing to remove):

- **Deck tile, line 737** — add a badge under the deck name:
  ```
  <div class="deck-name">CySA+</div>
  <div class="deck-meta">150 cards</div>
  <span class="deck-tag">Updated for the current exam</span>
  ```
- **New FAQ card** in the FAQ grid (after line 899):
  > **Are the decks current with the latest exam objectives?**
  > `Yes. The CySA+ deck has been rewritten for the current version of the CompTIA CySA+ exam — not the previous revision. Deck updates are included free with Pro, so when CompTIA revises an exam you get the updated cards without paying again.`
- **Meta description, line 16** — append: `CySA+ updated for the current exam.`
- **JSON-LD description, line 46** — append the same sentence so structured data and copy agree.
- **Hero description, line 688** — worth a clause: `…with 1,050 cards, spaced repetition, and scenario-based questions that mirror the real exams — including a CySA+ deck rewritten for the current exam version.`

> Fill in the specific exam code yourself before publishing — I have deliberately written the copy above without one, since your ground truth didn't specify it and guessing a code would recreate exactly the staleness problem you're auditing for.

---

## 3. Per-app findings — MEDIUM (stale / missing features) and LOW (wording, consistency, links)

### CertFlash — `certflash.html`

**MEDIUM**

- **M-1 · `1,050+` overstates the card count · 9 occurrences** (lines 16, 24, 32, 46, 688, 714, 801, 859, 888, plus `index.html` line 649). Ground truth is exactly **1,050** cards — 7 decks × 150. The deck tiles themselves (lines 720, 726, 732, 738, 744, 750, 756) each say `150 cards`, which multiplies to precisely 1,050, so the page contradicts its own arithmetic every time it says "1,050+". Change all occurrences to `1,050` (dropping the plus sign is also the more confident claim — a specific number reads as verified, "1,050+" reads as marketing).
- **M-2 · No FAQPage structured data.** The page has a 6-question FAQ (lines 880–910) but the only JSON-LD is `SoftwareApplication`. `best-private-network-scanner-iphone.html` does implement FAQPage, so the pattern exists in the codebase. Adding it here is free rich-result eligibility.

**LOW**

- **L-1 · Line 801 — iPad mention in an iPhone-framed app.** `"the cards you flag on your iPhone are waiting on your iPad"`. You didn't give me CertFlash's platform ground truth, so I'm not calling this wrong — but note the JSON-LD at line 48 says `"operatingSystem": "iOS"` and the rest of the page is iPhone-framed. If CertFlash is iPhone-only, this line needs the same treatment as H-4. Listed under "Claims to verify manually" below.

**Verified correct:** all seven deck names match ground truth exactly (A+, Network+, Security+, CySA+, Linux+, Cloud+, PenTest+, lines 719–756). $5.99 appears consistently in meta (16), og (24), twitter (32), JSON-LD (54), pricing card (855), FAQ (888), and final CTA (919). **No surviving `$3.99` anywhere in the repo** — I grepped all 22 files; the reprice is fully propagated.

---

### Referra — `referra.html`

**MEDIUM**

- **M-3 · Three shipped features are missing from the page entirely.** Ground truth lists dual-role contacts, contact detail, and privacy-first onboarding as shipped; none appear. Dual-role contacts in particular (a contact who both refers to you and receives referrals from you) is a real differentiator for a referral CRM and is worth its own feature card.
- **M-4 · The analytics feature card doesn't mention the donut chart** (line 695 describes the leaderboard, velocity, category breakdown, and weekly summaries). The donut chart is a shipped, visually distinctive feature and is the kind of thing a screenshot caption sells.
- **M-5 · Four feature claims aren't on your shipped list — verify or cut.** These aren't in the "never shipped" bucket, but they also aren't in the shipped list you gave me, so I can't confirm them:
  - Line 689 — `"Batch-log multiple referrals from a single workshop or screening event in one tap."`
  - Line 695 — `"referral velocity over time"` and `"weekly summaries"`
  - Line 701 — `"Outreach Cadence shows you who you've nurtured recently — and flags referrers you haven't talked to in 90+ days."` (named feature, specific 90-day threshold)
  - Line 671 — `"Tag by category and organization"` — defensible if it means the shipped category picker, but with H-7 removed from the pricing card this is the only surviving "tag" language on the page and reads as the unshipped tagging feature. Suggest `"Sort by category and organization"`.
- **M-6 · No `SoftwareApplication` JSON-LD.** Referra is the **only** app page in the repo with no structured data at all — CertFlash, NetScanPro, PrepPeriod, Field Stub, Remindfully, Bar Barons, Digital Legacy Vault, and TagSweep all have it. It's also the page where structured data matters most right now, because `operatingSystem` is where the iPad claim (H-10) belongs. Suggested block, matching the sibling pages' shape:
  ```json
  {
    "@context": "https://schema.org",
    "@type": "SoftwareApplication",
    "name": "Referra",
    "applicationCategory": "BusinessApplication",
    "operatingSystem": "iOS, iPadOS",
    "downloadUrl": "https://apps.apple.com/us/app/referra-referral-tracker/id6763003248",
    "offers": { "@type": "Offer", "price": "0", "priceCurrency": "USD" }
  }
  ```

**LOW**

- **L-2 · Line 653 — wording drift on the collaborator description.** `"Referra was built in collaboration with a cash-based physical therapist running their own practice"`. Your standing instruction is **"a cash-pay physical therapist."** No individual is named anywhere in the file — that rule is being followed correctly. This is purely "cash-based" → "cash-pay" for consistency with how you describe the market everywhere else.
- **L-3 · No `<link rel="canonical">`.** Referra and `index.html` are the only two pages missing one.

**Verified correct:** App Store ID `6763003248` on all four links (640, 756, 771, 785) and on `index.html` (611) and `services.html` (827) — correct everywhere. Free tier 25 contacts (750, 818). Pro Monthly $4.99 (790), Annual $39.99 (762), Lifetime $79.99 (776) — all match. "Save 33% vs. monthly" (769) is arithmetically right ($4.99 × 12 = $59.88; $39.99 is 33.2% off). The "Track relationships, not patients" positioning (799) and the explicit not-a-medical-record disclaimer (805) are both present and well-stated.

---

### NetScanPro — `netscanpro.html`

**MEDIUM**

- **M-7 · Line 833 — vague version reference will go stale.** `"the latest release improves detection of smart-home and Matter devices."` "Latest release" is undated and unversioned; in six months it means nothing. Suggest naming the capability instead of its recency: `"NetScanPro detects smart-home and Matter devices that broadcast on standard mDNS. Some legacy proprietary devices (like older Amazon Echoes) don't broadcast at all, so they may appear unlabeled."`
- **M-8 · 1.11.2 is bug-fix only — no page changes needed.** Recorded here so it's explicit: I found nothing on the page that 1.11.2 makes stale.
- **M-9 · `best-private-network-scanner-iphone.html` lines 2–8 — a publish checklist is still live in production markup.** An HTML comment reading `PUBLISH CHECKLIST` with unchecked items including `Verify current competitor prices on the App Store before publishing` and `Update datePublished / dateModified in JSON-LD`. Relatedly, lines 54–55 have `"datePublished": "2026-06-26"` and `"dateModified": "2026-06-26"` — identical, so `dateModified` was indeed never updated. Remove the comment; set `dateModified` to the date you next edit the file. Also note the competitor prices in that comparison table were, by the checklist's own admission, never verified — worth a pass before you leave it up.

**LOW**

- **L-4 · Device rename is never mentioned on `best-private-network-scanner-iphone.html`.** It's a real Pro feature (`netscanpro.html` line 791 lists it) and a reasonable differentiator against competitors, but the comparison page's Pro feature summary (line 500) omits it.

**Verified correct — including the one I most expected to find broken:** **MAC address wording is correctly hedged everywhere.** Every instance qualifies the claim — line 704 `"vendor and device details where available"`, line 710 `"from what each device broadcasts (Bonjour, UPnP, and MAC vendor prefixes)"`, line 772 `"Hostname, IP, and vendor info where available"`, line 838 `"its MAC vendor prefix where that's available"`, and JSON-LD line 46 `"on a best-effort basis from what each device broadcasts"`. There is **no unqualified claim that the app shows MAC addresses for all devices** anywhere in the repo. The confidence-level framing (lines 710, 838) and the explicit "devices that share nothing stay honestly unlabeled rather than mislabeled" (710) are exactly right.

Also verified clean: **no $19.99 second tier**, **no per-device notes**, **no per-device history**, **no catalogue export/import**, **no Wake-on-LAN**, **no subnet calculator** anywhere in the repo attributed to NetScanPro. (Wake-on-LAN and subnet-calculator language does appear on `best-private-network-scanner-iphone.html` lines 422, 428, 462 — but attributed to competitors iNet and IP Scanner, which is fine.) Pricing is consistent: $6.99 and 3 scans/day in meta (16), og (24), JSON-LD (54), pricing cards (771, 782), FAQ (813), CTA (859). Port count "25 ports" matches at lines 722 and 787.

---

### PrepPeriod — `prepperiod.html`

**MEDIUM**

- **M-10 · The $9.99 price is never stated anywhere.** Line 839 says `"Single purchase — price shown in the app"` and line 838 shows the amount as the word `Once`. Every other paid app on the site names its price on the page. Hiding it costs you the "one-time $9.99, no subscription" contrast that is this app's strongest pitch against subscription-based classroom tools. Suggest `<div class="pricing-amount">$9.99</div>` and `<div class="pricing-period">One-time payment</div>`.
- **M-11 · JSON-LD `offers.price` is `"0"` (line 53) while the page sells a paid unlock.** Schema/copy mismatch. Once M-10 lands, either add a second `Offer` for $9.99 or use an `AggregateOffer` with `lowPrice: "0"` / `highPrice: "9.99"`.
- **M-12 · User-configurable marking periods are missing from the page.** The string "marking period" does not appear in the file. This is a shipped feature and a real differentiator — teachers on trimesters, quarters, or six-week cycles are exactly the audience that gets burned by tools that assume semesters. The attendance card (line 776) mentions rotating and A/B schedules but not marking periods.
- **M-13 · The "Data Not Collected" App Store privacy label is never cited.** The privacy section (lines 800–807) makes the right argument but doesn't invoke Apple's own label, which is the single most credible third-party proof point available for a teacher evaluating whether student data is safe. Suggest adding to line 805: `PrepPeriod carries Apple's "Data Not Collected" privacy label on the App Store — the strongest disclosure Apple offers.`

**LOW**

- **L-5 · JSON-LD `operatingSystem` is `"iOS"` (line 48), omitting watchOS.** For a watch-first app whose hero eyebrow says `iOS + Apple Watch` (line 680), the structured data undersells the platform. Suggest `"iOS, watchOS"`.

**Verified correct:** App Store ID `6788439492` on all five links (685, 832, 847, 914) and on `index.html` (581). Tagline "Get your prep period back." present as the H1 (681). All core features present and accurately described: behavior incidents and positive notes (763–764), hall passes with the exact bathroom/water/nurse triad (757–758, 871), parent-contact log with outcomes (769–770), attendance (775–776), rotating and A/B day schedules (776, 891), per-student PDF reports (781–782). Free tier correctly includes iCloud sync (line 830, `Private iCloud sync across your devices`). PDF export is correctly the *only* thing behind the unlock (841–845). The 1.1.0 rating prompt and Settings row are correctly **absent** — no action needed.

---

### Field Stub — `fieldstub.html`

**MEDIUM**

- **M-14 · CSV export is claimed as a free feature but isn't in your ground truth.** Line 686 (`"Export your visits to CSV across any date range — free for all users"`), line 738 (free-tier bullet `CSV export`), and line 804 (FAQ, `"CSV export across any date range is included in Free"`). Your gate list names only unlimited clients, PDF export, and voice transcription for Pro — it says nothing about CSV either way. If CSV export doesn't ship, this is a HIGH overclaim in the free tier; if it ships free, no change. Flagged for you to confirm.

**LOW**

- **L-6 · Lines 779 and 794 make roadmap promises.** `"iCloud sync is on the roadmap for a future update — when it ships, your existing data will migrate automatically and stay private to your iCloud account"` and `"In v1, you'd want to wait for iCloud sync (planned) before relying on phone migration."` Forward-looking commitments in FAQ copy age badly and set an expectation you may not want to be held to. Suggest softening to `"Field Stub is single-device today. If iCloud sync ships, your existing data will migrate automatically."`

**Verified correct:** 5-client free cap (734, 784, 820). Pro gates match exactly — unlimited clients (749), PDF reports (750, 680), on-device voice transcription (751, 674). No iPad claim anywhere in the file; the page consistently says "your iPhone" (623, 698, 789). The on-device transcription explanation (799) is unusually specific and defensible.

---

### Remindfully — `remindfully.html`

No MEDIUM findings. Per your instruction I have not judged this page's feature list against anything — every claim it makes is extracted in section 4.

**LOW**

- **L-7 · No `<link rel="canonical">` issue — it has one (line 18). No findings on links or platform.** The page is internally consistent: $4.99 appears in meta (16), JSON-LD (53), narrative (643), pricing card (736), FAQ (793, 798), and CTA (809), with no contradictions. `operatingSystem` is `"iOS"` (line 48) and the page makes no iPad, Mac, or Watch claim, so schema and copy agree.
- **L-8 · The Live Activity card (line 676) is described as a lock-screen feature only.** Live Activities also appear in the Dynamic Island on supported iPhones — if that's true of your implementation it's a free upgrade to the claim. Verify before adding.

---

### Bar Barons — `bar-barons.html`

**MEDIUM** — three shipped v1.5.1 features are entirely absent from the page:

- **M-15 · Invite URLs with rich iMessage previews.** Not mentioned anywhere on `bar-barons.html`, despite `barbarons/invite/index.html` existing in the repo as the deep-link landing page and `.well-known/apple-app-site-association` being configured for it. For a social app whose whole growth loop is friends inviting friends, this is the most marketable feature you have and the page doesn't mention it. Suggested feature card:
  > 📨 **Invite friends with one link**
  > `Send an invite link in iMessage and it arrives as a rich preview, not a naked URL. Tap it and you land straight in the app — already connected.`
- **M-16 · Friend-Baron map indicators.** Not mentioned. The "Friend feed" card (line 628) describes a shared activity feed but not the map indicators showing which bars your friends hold. That's the more visual, more competitive framing.
- **M-17 · Suggest-a-bar.** Not mentioned. Worth adding directly beneath the coverage section (line 682), because the natural objection to "1,900+ bars" is "but is *my* bar in it?" — and the answer is that users can submit it.

**LOW**

- **L-9 · Line 634 vs the coverage detail.** The feature card says `"1,900+ bars covered"` and describes `"a growing roster on Long Island, in Westchester County, and 100 bars across Boston"`, but the Long Island region strip (lines 719–722) lists exactly one neighborhood chip (Rockville Centre) against three for Westchester and ten for Boston. Not wrong, but "a growing roster on Long Island" backed by one town undersells or oversells depending on the reader. Consider either adding chips or dropping Long Island from the headline list.

**Verified correct — the important one:** **emoji reactions do not appear anywhere in the repo.** I grepped all 22 files for "reaction", "emoji", and "react" — the unshipped v1.6.0 feature has not leaked into any page, meta tag, or JSON-LD. The single "emoji" hit is unrelated: `bar-barons-privacy.html` line 336, `"Display name and avatar emoji — chosen by you. Visible to your friends and on public leaderboards."` That's the avatar picker, not reactions, and it's fine — though it's worth confirming the avatar-emoji feature itself still ships, since `bar-barons.html` never mentions it. Tier ladder (Newcomer → Patron → Regular) is consistent between the feature card (622) and the tier strip (654–666). App Store ID `6770139105` matches on both links (594, 761) and on `index.html` (621).

---

### Digital Legacy Vault — `digital-legacy-vault.html`

Per your instruction, this page has not been judged against any feature or pricing baseline. Full claim extraction is in section 4.

**MEDIUM**

- **M-18 · The JSON-LD block (lines 42–57) has no `offers` field at all.** Every other app page in the repo declares one. The page also never states a price, a tier, or whether the app is free — so a visitor cannot tell what it costs without going to the App Store. Whatever the pricing model is, it should appear both in the copy and in the structured data.

**LOW**

- **L-10 · No FAQPage structured data** despite a 6-question FAQ (lines 815–845). Same gap as CertFlash (M-2).

---

## 4. Claims to verify manually

These are extracted, not judged. Each is a claim the site currently makes that I could not check against ground truth.

### 4.1 CertFlash — every feature claim currently on the page

Your note said not to treat the page's feature list as verified-complete for 1.6.0. Here is the full inventory, so you can diff it against the shipped build:

| # | Claim | Line(s) |
|---|---|---|
| 1 | SM-2 spaced repetition, with Again / Hard / Good / Easy rating buttons | 703, 777, 893 |
| 2 | Scenario-based **and performance-style** questions in **every** deck | 704, 762, 898 |
| 3 | Quick Quiz mode with random sets of **10, 20, or 50** questions, separate from the SRS schedule | 783 |
| 4 | Per-deck progress tracking **and a "Due for Review" count** | 789 |
| 5 | Study streaks with **milestone celebrations** | 795 |
| 6 | **Daily reminder notifications** | 795 |
| 7 | **Confetti burst** on completing a session | 795 |
| 8 | Favorites / bookmarking | 800–801 |
| 9 | Search across all cards at once | 801 |
| 10 | Favorites sync **via iCloud** across devices | 801, 860, 903, 908 |
| 11 | Fully offline after download | 688, 705, 807, 903 |
| 12 | Dark mode | 705, 807, 847 |
| 13 | Swipe navigation between cards | 807 |
| 14 | Built natively with SwiftUI | 705, 807 |
| 15 | No account required for the free tier | 841, 883 |
| 16 | **All future decks included free with Pro** | 861, 888 |
| 17 | Restore Purchase across Apple ID devices | 869, 908 |
| 18 | A+ deck free; other six behind Pro | 721–758, 843, 858 |
| 19 | Exactly 150 cards in each of the seven decks | 720, 726, 732, 738, 744, 750, 756 |
| 20 | Favorites available on iPad (see L-1) | 801 |

Highest-risk items if unverified: **#16** (a forward commitment to ship future decks free — hard to walk back), **#3** (specific quiz sizes), **#7** (confetti), and **#20** (iPad).

### 4.2 Remindfully — every feature and price claim currently on the page

| # | Claim | Line(s) |
|---|---|---|
| 1 | Single **morning** digest notification, once daily, of everything due today | 624, 658, 773 |
| 2 | Time-specific reminders still interrupt at their time | 664, 726, 778 |
| 3 | **High Priority**-flagged reminders always break through | 664, 727, 778 |
| 4 | Missed **daily** reminders roll forward to today | 670, 783 |
| 5 | Missed **weekly/monthly** reminders stay flagged rather than disappearing | 670, 783 |
| 6 | Live Activity lock-screen "Today card" showing progress and what's next — **PRO** | 676, 739 |
| 7 | **Check items off from the lock screen without unlocking** | 676 |
| 8 | Per-list interruption rules — **PRO** | 682, 740 |
| 9 | Reads existing Apple Reminders; writes completions back | 688, 728, 763 |
| 10 | "Read-only reminder mirror" — free tier | 729, 793 |
| 11 | Siri, Apple Watch, and Reminders app keep working unchanged | 642, 688, 763 |
| 12 | Nothing to migrate; reminders never move | 624, 642, 768 |
| 13 | On-device only; no accounts, no servers, no tracking, no ads | 704–705, 788 |
| 14 | **Price: one-time $4.99 Pro, no subscription** | 16, 53, 643, 736, 793, 798 |
| 15 | Free forever: digest, time-based + high-priority interruptions, completion sync, reminder mirror | 724–729, 793 |
| 16 | Restore Purchase across Apple ID devices | 749, 798 |

Item **6** is the one you named as known-shipped. Items **7** and **10** are the ones I'd check first — interactive Live Activity controls have specific iOS version requirements, and "read-only reminder mirror" is unusual phrasing that may describe something that has since changed.

Also worth noting: `v1.4.1` is never stated on the page, so nothing there can be version-stale.

### 4.3 Digital Legacy Vault — every feature, price, tier, and platform claim

| # | Claim | Line(s) |
|---|---|---|
| 1 | **100% offline — zero network calls**, app has no technical ability to transmit data | 683, 698, 761, 818 |
| 2 | No accounts, no sign-up, no cloud | 683, 698, 843 |
| 3 | Encrypted at rest via **iOS Complete File Protection** | 699, 761, 838 |
| 4 | Face ID / Touch ID lock; **auto-locks every time you leave the app** | 699, 755, 838 |
| 5 | **Excluded from iCloud backup by design** | 699, 767, 838 |
| 6 | Stores: emergency contacts **sorted by priority** | 714–715, 823 |
| 7 | Stores: final wishes / arrangements & directives | 718–720, 823 |
| 8 | Stores: medications, **doses & allergies** | 723–725, 823 |
| 9 | Stores: document locations ("map to the paperwork") | 728–730, 823 |
| 10 | Stores: letter to executor | 733–735, 823 |
| 11 | PDF export with **name, date, document ID, executor letter, and confidentiality footer** | 779, 828 |
| 12 | **Apple Legacy Contact guidance** when designating a primary emergency contact | 773, 833 |
| 13 | **Home screen widget** showing only the last-updated date, never the data | 785 |
| 14 | Built natively for iOS | 700 |
| 15 | **Platform: `operatingSystem: "iOS"`** in JSON-LD; no iPad/Mac/Watch claim anywhere | 48 |
| 16 | **Price / tier: none stated anywhere on the page.** JSON-LD has **no `offers` block** | 42–57 |
| 17 | App Store ID `6762098328` | 51, 685, 856 |

Highest-risk items: **#1** (an absolute technical claim — "zero network calls" is falsifiable by anyone with a proxy, and the page repeats it four times), **#5** (iCloud-backup exclusion is a specific implementation detail), **#12** (Legacy Contact guidance), and **#13** (the widget). And **#16** — the pricing silence — is worth resolving regardless of what the answer is.

---

## 5. Cross-page issues

### 5.1 Pricing consistency

| App | Copy | Pricing table | FAQ | JSON-LD `offers` | Verdict |
|---|---|---|---|---|---|
| CertFlash | $5.99 (16, 24, 32, 688, 919) | $5.99 (855) | $5.99 (888) | `"5.99"` (54) | ✅ consistent |
| Referra | $4.99/mo (790) | $39.99 yr (762) · $79.99 life (776) | — (no FAQ) | **absent** | ⚠️ prices consistent; no schema (M-6) |
| NetScanPro | $6.99 (16, 24, 859) | $6.99 (782) | $6.99 (813) | `"6.99"` (54) | ✅ consistent |
| NetScanPro (field-techs) | $6.99 (16, 24, 558) | $6.99 (547) | — | `"6.99"` (54) | ✅ consistent (page itself unpublishable — H-1) |
| PrepPeriod | unpriced (702, 912) | **"price shown in the app"** (839) | unpriced (901) | **`"0"`** (53) | ❌ price never stated; schema says free (M-10, M-11) |
| Field Stub | $39.99 (16, 24, 32, 641, 820) | $39.99 (746) | $39.99 (773–774) | `"39.99"` (54) | ⚠️ internally consistent, but ground truth says $39 (H-12) |
| Remindfully | $4.99 (16, 643, 809) | $4.99 (736) | $4.99 (793, 798) | `"4.99"` (53) | ✅ consistent |
| Bar Barons | free (759) | — | — | `"0"` (53) | ✅ consistent |
| Digital Legacy Vault | **none** | — | — | **absent** | ❌ no price stated anywhere (M-18) |
| TagSweep | $29.99 (16) | $0 / $29.99 (895–925) | $29.99 (974, 984) | `"0"` only (58) | ⚠️ copy consistent; schema omits the paid tier |

**No surviving `$3.99` anywhere in the repo.** **No `$19.99` anywhere in the repo.** Both confirmed clean by grep across all 22 files.

### 5.2 App Store links

| App | ID in repo | Your ground truth | Match |
|---|---|---|---|
| Referra | `6763003248` | `6763003248` | ✅ |
| NetScanPro | `6762677151` | `6762677151` | ✅ |
| PrepPeriod | `6788439492` | `6788439492` | ✅ |
| CertFlash | `6761732835` | not supplied | — |
| Field Stub | `6767348984` | not supplied | — |
| Bar Barons | `6770139105` | not supplied | — |
| Remindfully | `6779375899` | not supplied | — |
| Digital Legacy Vault | `6762098328` | not supplied | — |
| TagSweep | none — placeholder badge | not supplied | — (pre-launch) |

All three IDs you supplied are correct, and each is used consistently across every page that links to that app (app page, `index.html`, and `services.html` where applicable). No mismatched or cross-wired IDs found.

### 5.3 Platform claims

| App | Ground truth | Site says | Verdict |
|---|---|---|---|
| Referra | iPhone **+ iPad** (1.6) | iPad never mentioned | ❌ H-10 |
| PrepPeriod | iPhone + Watch, **no iPad** | claims iPad sync (804) | ❌ H-11 |
| NetScanPro | iPhone, **no iPad** | implies iPad (848) | ❌ H-4 |
| Field Stub | **no iPad** | iPhone only throughout | ✅ |
| Bar Barons | iOS 17+ | `iOS 17+` (48, 759) | ✅ |
| CertFlash | not supplied | mentions iPad once (801) | ⚠️ L-1 |
| Remindfully | not supplied | iOS only | — |
| Digital Legacy Vault | not supplied | iOS only | — |

### 5.4 Structured data vs. visible copy

| Page | Schema types | Mismatches |
|---|---|---|
| `certflash.html` | SoftwareApplication | none; **FAQPage missing** despite 6-question FAQ (M-2) |
| `referra.html` | **none** | whole block absent (M-6) |
| `netscanpro.html` | SoftwareApplication | none |
| `netscanpro-field-techs.html` | SoftwareApplication | description sells unshipped multi-network (H-1) |
| `best-private-network-scanner-iphone.html` | Article + FAQPage | **FAQPage answer at line 84 asserts unshipped multi-network** (H-5); `dateModified` never updated (M-9) |
| `prepperiod.html` | SoftwareApplication | `offers.price: "0"` vs. a real paid unlock (M-11); `operatingSystem` omits watchOS (L-5) |
| `fieldstub.html` | SoftwareApplication | none internally |
| `remindfully.html` | SoftwareApplication | none |
| `bar-barons.html` | SoftwareApplication | none |
| `digital-legacy-vault.html` | SoftwareApplication | **no `offers` block at all** (M-18); FAQPage missing (L-10) |
| `tagsweep.html` | SoftwareApplication | `offers.price: "0"` while the description text itself names a "$29.99 in-app purchase" |
| `index.html`, `services.html` | **none** | no `Organization` schema on the company homepage (L-13) |

### 5.5 "Our apps" index — do all eight appear?

**Yes.** `index.html` lists all eight in both the hero button row (lines 546–554) and the ventures grid, plus TagSweep and IT Study Hub:

TagSweep (566) · PrepPeriod (575) · Remindfully (585) · Field Stub (595) · Referra (605) · Bar Barons (615) · NetScanPro (625) · Digital Legacy Vault (635) · CertFlash (645) · IT Study Hub (655)

**M-19 · But the homepage social preview names only six of them.** `index.html` lines 21 and 29:
> `iOS apps and educational tools — Bar Barons, Referra, CertFlash, Digital Legacy Vault, NetScanPro, and IT Study Hub.`

Missing: **Field Stub, Remindfully, PrepPeriod, TagSweep.** This is the text that appears whenever anyone shares fogartyholdings.com in iMessage, Slack, or on social. Suggest: `iOS apps and educational tools — PrepPeriod, TagSweep, Remindfully, Field Stub, Referra, NetScanPro, CertFlash, Bar Barons, Digital Legacy Vault, and IT Study Hub.` (or a category-level description that won't need updating with every launch).

**M-20 · `services.html` links to no product page at all.** Its portfolio strip links straight out to the App Store for Referra (line 827), CertFlash (832), Digital Legacy Vault (837), and NetScanPro (842) — and contains **zero** links to any local `.html` product page. Two problems: anyone landing on Services has no path into the rest of the site except back to the homepage, and the strip covers only four of your nine apps (no Field Stub, Remindfully, PrepPeriod, Bar Barons, or TagSweep). Suggest pointing each cell at its own product page and adding the five missing apps.

### 5.6 Broken internal links, orphan pages

**No broken internal links.** Every local `href` in the repo resolves to a file that exists — 26 distinct link targets checked against the 22 files present, plus `favicon.svg`, `favicon.ico`, and `apple-touch-icon.png`.

- **L-11 · `favicon.ico` and `apple-touch-icon.png` are referenced by all 22 pages but neither file exists in the repo.** Only `favicon.svg` is present. Modern browsers will fall back to the SVG, but iOS home-screen bookmarks will show a generic icon.
- **L-12 · No orphan pages.** Every file appears in `sitemap.xml` except `index.html`'s own alias (the sitemap uses the bare domain, which is correct) and `barbarons/invite/index.html` (correctly excluded — it's a deep-link handler, and `.well-known/apple-app-site-association` is configured for it).
- **`netscanpro-field-techs.html` and `best-private-network-scanner-iphone.html` are reachable only from `netscanpro.html`**, not from the homepage. That's a reasonable SEO structure, not a defect — but note that once H-1 is actioned, the sitemap entry at line 154 has to go with it.

### 5.7 TODO / placeholder / leftover text

- **`best-private-network-scanner-iphone.html` lines 2–8** — live `PUBLISH CHECKLIST` HTML comment with unchecked items (M-9). This is the only genuine leftover in a published page.
- **`tagsweep.html` lines 745–750, 907–911, 926–930, 997–1001** — four `APP STORE BADGE PLACEHOLDER` comments and "Coming to the App Store" badges. Expected for a pre-launch page, but the page **is** in `sitemap.xml` (line 86) and linked from the homepage, so it's publicly crawlable in placeholder state. Worth deciding deliberately.
- No lorem ipsum, no `TODO`, no `FIXME`, no dummy content anywhere else in the repo.

### 5.8 Screenshot images

**There are no product screenshots on the site.** Every `<img>` in the repo points to an app icon, `homepagelogo.jpg`, `textpreview.jpg` (the og-image), or IT Study Hub's remote favicon. Full inventory: `CertFlashAppIcon.png`, `DigitalLegacyVaultAppIcon.png`, `Fieldstub-appicon.png`, `PrepPeriodAppIcon.png`, `Referra-Icon-1024.png`, `RemindfullyAppIcon.png`, `TagSweepAppIcon.png`, `barbarons-appicon.png`, `netscanpro-icon.png`, `homepagelogo.jpg`, `textpreview.jpg`.

So **there is no screenshot depicting a stale UI or a contradicting price** — the risk you asked me to check for doesn't exist yet. The flip side: nine app pages sell software without showing it. Referra's new iPad layout (H-10) is a good candidate for the first screenshot set, since the adaptive analytics grid is the kind of thing that's much easier to show than describe.

### 5.9 One thing outside the audit scope

**TagSweep is a ninth app** with a full marketing page (`tagsweep.html`, 1,032 lines), a privacy page, an icon, a sitemap entry, and top billing in the homepage ventures grid — but it wasn't in the eight-app ground truth. I have not judged any of its claims. Its full claim inventory is in the subagent extraction (feature set, $0 free tier up to 100 devices, $29.99 unlimited unlock, iOS 17+); if you want it audited properly, send me the ground truth and I'll run the same pass.

---

## Appendix — files reviewed

`index.html` · `services.html` · `certflash.html` · `referra.html` · `netscanpro.html` · `netscanpro-field-techs.html` · `best-private-network-scanner-iphone.html` · `prepperiod.html` · `fieldstub.html` · `remindfully.html` · `bar-barons.html` · `digital-legacy-vault.html` · `tagsweep.html` · `certflash-privacy.html` · `referra-privacy.html` · `netscanpro-privacy.html` · `prepperiod-privacy.html` · `fieldstub-privacy.html` · `remindfully-privacy.html` · `bar-barons-privacy.html` · `digital-legacy-vault-privacy.html` · `tagsweep-privacy.html` · `barbarons/invite/index.html` · `sitemap.xml` · `.well-known/apple-app-site-association`

All line numbers refer to the files as they stood at audit time (`index.html` mtime 2026-07-26; `netscanpro.html` 2026-07-15; `prepperiod.html` 2026-07-17; `fieldstub.html` 2026-07-22; all others 2026-07-04 or earlier).
