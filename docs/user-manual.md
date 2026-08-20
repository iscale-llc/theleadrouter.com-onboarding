# Lead Routing System - User Manual

A comprehensive guide to managing leads, buyers, partners, and campaigns.

> **A note on currency:** money amounts in this manual's examples are shown in USD ($). If your account is configured with a different currency (e.g. GBP or EUR), every amount in the app — prices, balances, caps, reports, and exports — is displayed in your account's currency instead.

---

## Table of Contents

1. [Getting Started](#getting-started)
    - [Global Search (⌘K)](#global-search)
2. [Dashboard](#dashboard)
3. [Leads](#leads)
    - [Adjusting sales](#adjusting-sales)
    - [Demographics](#lead-demographics)
4. [Buyers](#buyers)
5. [Contracts](#contracts)
6. [Partners](#partners)
7. [Campaigns](#campaigns)
8. [Offers](#offers)
    - [Offer Visibility](#offer-visibility)
    - [Partner Marketplace](#partner-marketplace)
    - [Cascade Status Changes](#offer-cascade-status)
    - [DQ Contracts](#dq-contracts)
9. [Verticals](#verticals)
10. [Lead Scoring](#lead-scoring)
11. [Reports](#reports)
    - [CSV Export](#csv-export)
11. [Settings](#settings)
    - [Notifications](#notifications)
12. [Users](#users)
13. [Contacts](#contacts)
14. [Block List](#block-list)
15. [Suppression List](#suppression-list)
16. [System Lists](#system-lists)
17. [Webhooks](#webhooks)
18. [Integrations](#integrations)
    - [Custom HTTP Prescreening](#custom-http-prescreening)
    - [Lead Enrichment Integrations](#lead-enrichment-integrations)
19. [Data Deletion](#data-deletion)
20. [Buyer Dashboard](#buyer-dashboard)
21. [Partner Portal](#partner-portal)
    - [Time of Revenue & Time of Lead Reports](#partner-reports)
22. [Intake Centers](#intake-centers)
23. [Disposition Webhook](#disposition-webhook)
24. [People Hub](#people-hub)
25. [Call Tracking](#call-tracking)
    - [Number Pool Management](#number-pool-management)
    - [Twilio Call Tracking Setup](#twilio-calls-setup)
    - [Telnyx Call Tracking Setup](#telnyx-calls-setup)
26. [Messaging](#messaging)
    - [Dashboard](#messaging-dashboard)
    - [Contacts](#messaging-contacts)
    - [Segments](#messaging-segments)
    - [Templates](#messaging-templates)
    - [Flows](#messaging-flows)
    - [Campaigns](#messaging-campaigns)
    - [Analytics](#messaging-analytics)
    - [Deliverability Monitoring](#deliverability-monitoring)
    - [Messaging Settings](#messaging-settings)
    - [Amazon SES Setup](#ses-setup)
    - [SendGrid Setup](#sendgrid-setup)
    - [SES vs SendGrid Comparison](#ses-vs-sendgrid)
    - [DNS Records for Email Deliverability](#email-dns)
    - [Twilio SMS Setup](#twilio-sms-setup)
    - [Bandwidth SMS Setup](#bandwidth-sms-setup)
    - [Bandwidth Call Tracking Setup](#bandwidth-calls-setup)
    - [SignalWire Call Tracking Setup](#signalwire-calls-setup)
27. [Consent Tracking](#consent-tracking)
28. [Multi-Tenant Data Isolation](#multi-tenant-data-isolation)
30. [Superadmin](#superadmin)
    - [Per-Tenant Feature Flags](#per-tenant-feature-flags)
31. [Tenant Signup](#tenant-signup)
32. [Glossary](#glossary)
34. [Audit Trail / Activity Log](#audit-trail--activity-log)
35. [Postman Collection](#postman-collection)
36. [Posting Log](#posting-log)
37. [Default Lead Fields](#default-lead-fields)
38. [Use Cases & Configuration Guide](#use-cases) — 100 real-world scenarios with recommended settings
39. [AI-Powered Features](#ai-powered-features)
40. [AI Agent Skills](#ai-agent-skills)
41. [AI Assistant](#ai-assistant)
    - [Voice Input](#voice-input)
    - [Voice Output](#voice-output)
    - [Interactive Walkthroughs](#interactive-walkthroughs)
    - [Action Cards](#action-cards)
    - [Spotlight Highlights](#spotlight-highlights)
    - [Agent Sessions](#agent-sessions)
42. [Portal Signup](#portal-signup)
43. [Deliver-First Pipeline](#deliver-first-pipeline)
44. [Products](#products)
45. [Billing & Lead SKUs](#billing-and-lead-skus)
    - [Platform Services](#platform-services)
    - [For Admins](#skus-for-admins)
    - [For Buyers](#skus-for-buyers)
    - [Landing Page Flow](#skus-landing-page-flow)
    - [Subscription vs One-Time](#skus-subscription-vs-one-time)
    - [SKU Gating Behavior](#skus-gating-behavior)
    - [SKU / Product System (Tenant Feature Flag)](#skus-tenant-feature)
    - [Pricing Models & Use Cases](#skus-pricing-models)
    - [Custom Funnel Integration (API)](#skus-custom-funnel)
    - [Stripe Connect Setup Checklist](#skus-connect-setup)
    - [Troubleshooting](#skus-troubleshooting)
46. [Need Help?](#need-help)

---

## Getting Started

### System Overview

The Lead Routing System connects **Partners** (lead sources) with **Buyers** (lead purchasers) through a sophisticated routing engine. Here's how data flows:

<!-- react-flow:system-overview -->

**Key Entities:**
- **Verticals**: Categories of leads (e.g., Auto Insurance, Home Services)
- **Partners**: Organizations who generate leads
- **Campaigns**: Partner programs that source leads
- **Offers**: Distribution rules linking campaigns to contracts
- **Buyers**: Companies purchasing leads
- **Contracts**: Buyer agreements defining pricing, caps, and delivery

### How Lead Routing Works

The following diagram shows the end-to-end flow when a partner submits a lead through the system.

<!-- react-flow:lead-routing -->

For a detailed view of every check the routing engine runs (dedup, integrations, filters, geo, caps, schedule, balance, etc.) with a printable/PDF-exportable format, visit [Lead Routing Flow (detailed, printable)](/admin/help/routing-flow).

### Entity Relationships

This diagram shows how the core entities in the system relate to each other.

<!-- react-flow:entity-relationship -->

### Authentication

- **Admin Portal**: Full system access at [/admin](/admin)
- **Buyer Portal**: Buyer-specific access at [/buyer](/buyer)
- Sign in at [/login](/login) with an email magic link, Google, or passkey

### Password Reset {#password-reset}

Password login and password reset are deprecated. If you cannot sign in:

1. Go to [/login](/login)
2. Request a fresh magic link for your email, or use Google/passkey if enabled
3. Check your email and follow the one-time link before it expires

**Notes:**
- Magic links are single-use and time-limited
- Works for both admin and buyer accounts
- If you don't receive an email, check your spam folder or contact your administrator

### Profile & Sign Out {#profile-sign-out}

Your avatar (initials) appears in the top-right corner of the header. Click it to open a dropdown showing your name, email, and current role. Use the **Sign Out** option in this dropdown to log out. Sign-out has been removed from the sidebar — the profile dropdown is now the only place to access it.

### Magic Link IP Mode {#security-magic-link-ip-mode}

**Location:** [/admin/settings](/admin/settings) → Security tab → Magic Link IP Binding

<!-- react-flow:magic-link-ip-flow -->

Magic links (used for passwordless login and Stripe checkout welcome) are bound to the IP address that requested them. When a link is redeemed from a different IP, this setting controls what happens:

- **Step-up to 2FA on mismatch** (default, recommended) — the user is forced through a fresh 2FA challenge before the session is issued. If they have no 2FA enrolled, they are sent through the 2FA setup flow first. This is the right choice for most tenants because real users frequently click links from mobile devices that switch networks (Wi-Fi to cellular) between request and click.
- **Subnet match** — the user is allowed through if the new IP shares a `/24` IPv4 (or `/64` IPv6) subnet with the requesting IP. Useful for organizations whose users move within a single corporate network or carrier subnet, but tighter than step-up for cross-network use.
- **Strict** — any IP change rejects the redemption with a `AUTH_IP_MISMATCH` error and the user has to request a new link. Highest security; may lock out legitimate users on mobile networks. Only pick this if your users always sign in from a stable corporate IP range.

Mismatches are recorded in the audit log with both IPs and the mode in effect, so you can review whether your choice is producing false positives before tightening further.

---

## Global Search (⌘K) {#global-search}

Press **⌘K** (Mac) or **Ctrl-K** (Windows/Linux) from anywhere in the admin dashboard to open the global search palette. You can also click the search icon in the top bar. Global search is available to admin and superadmin users.

**Searching.** Start typing (at least 2 characters) to search across buyers, partners, contracts, campaigns, offers, verticals, people, and intake centers by name. Numeric queries also match a record's display ID. Results are grouped by entity type with a colored type chip; press Enter or click a result to jump straight to its detail page. Use the arrow keys to move through results and Escape to close.

**Finding leads and people by contact info.** Leads are found only by exact display ID, email, or phone number — never by partial name — to protect PII. People are found by display ID, exact email, or exact phone. Looking a record up by email or phone requires PII access; without it, those lookups return nothing and any names shown are masked (e.g. `M*** Z***`). Results never expose raw email or phone numbers in the list.

**Empty state.** With no query typed, the palette shows your **Recently Visited** records (buyers, partners, contracts, campaigns, offers you opened this session), quick **Go To** navigation shortcuts, and **Create** commands for new buyers, partners, campaigns, offers, and contracts. Recently-visited history is scoped to your account and current organization and resets when you switch organizations or sign out.

Results are tenant-scoped — you only ever see records belonging to your current organization.

---

## Dashboard

**Location:** [/admin](/admin)

The dashboard provides a real-time overview of system activity.

### Metrics Cards

| Metric | Description |
|--------|-------------|
| Leads Today | Total leads received today |
| Sold Leads Today | Leads delivered and not rejected today |
| Revenue Today | Total revenue generated today |
| Revenue per Sold Lead | Revenue today divided by sold leads today |
| Revenue per Raw Lead | Revenue today divided by all leads received today |
| New Buyers Today | Buyer accounts created today |
| Order Revenue Today | Dollar total of SKU purchases today (new orders + subscription renewals) |
| Orders Today | Count of SKU purchases today (new orders + subscription renewals) |
| Active Partners | Partners with active status |
| Active Contracts | Contracts currently accepting leads |
| Total Contracts | All contracts ever created |
| Total Buyers | All buyer accounts in the system |

Time-based cards show a trend arrow comparing today (so far) against all of yesterday, with yesterday's value in the subtext. All day boundaries use the tenant timezone.

A **This Month** section below the daily cards tracks month-to-date performance. Trends compare against last month at the same point in the month (same elapsed time since the 1st), so a mid-month view is apples-to-apples. All month boundaries use the tenant timezone.

| Metric | Description |
|--------|-------------|
| Leads MTD | Total leads received this month |
| Sold Leads MTD | Leads delivered and not rejected this month |
| Revenue MTD | Total revenue generated this month |
| Projected Revenue | Revenue MTD extrapolated to a full-month run rate; trend compares against last month's final total |

### Quick Actions

- **View Leads**: Jump to lead management
- **Add Buyer**: Create new buyer account
- **Contracts**: Manage buyer contracts
- **Reports**: Access analytics

### Recent Leads

Displays the last 10 leads with:
- Lead ID and contact info
- Status (Pending, Matched, Sold, etc.)
- Revenue generated
- Creation timestamp

---

## Leads {#leads}

**Location:** [/admin/leads](/admin/leads)

### Lead List {#lead-list}

Browse, search, and manage all leads in the system.

#### Sorting

Click any column header to sort the list by that column. Click again to toggle between ascending and descending order. A small arrow indicator shows the current sort direction. Your sort preference is saved automatically and remembered when you return to the page.

Default sort: Created date, newest first.

This sorting feature is available on all list views: Leads, People, Buyers, Contracts, Partners, Campaigns, and Offers.

#### Column Picker (Leads)

Click the column picker icon to show/hide columns. The leads table supports 30+ columns including all lead fields, vertical, campaign, buyer, status, pricing, and timestamps. Columns can also be resized by dragging the column border.

#### Filtering Options

| Filter | Options |
|--------|---------|
| Search | Name, email, or phone |
| Status | All, Pending, Matched, Sold, Returned, Rejected, Scrubbed, Trashed |
| Vertical | Filter by lead category |

#### Advanced Filters

Click the **Advanced** button in the filter bar to open the Advanced Filters panel. A badge shows the number of active advanced filters.

**Entity Filters** (dropdowns):

| Filter | Description |
|--------|-------------|
| Partner | Filter by lead source partner |
| Campaign | Filter by source campaign |
| Vertical | Filter by lead category (also in top bar) |
| Offer | Filter by distribution offer |
| Buyer | Filter by buyer who purchased the lead |
| Contract | Filter by buyer contract |

**Status Filters** (tri-state toggles — All / Yes / No):

| Filter | Description |
|--------|-------------|
| Sold | Show only sold or unsold leads |
| Paid | Show only paid or unpaid leads |
| Scrubbed | Show only scrubbed or non-scrubbed leads |
| Trashed | Show only trashed or non-trashed leads |
| Test | Show only test or non-test leads |

**Date Range**: Filter leads by creation date (From / To).

Click **Apply Filters** to apply. Click **Reset** to clear all advanced filters (top-bar filters are preserved). Advanced filters are preserved when using the top-bar search/status/vertical filters.

#### Bulk Actions {#bulk-actions}

Select multiple leads to perform:
- **Cherry Pick**: Manually assign to a buyer
- **Adjust Payout**: Modify partner payout
- **Scrub**: Remove from distribution
- **Trash**: Mark as invalid
- **Toggle Test**: Mark/unmark as test lead
- **Toggle Paid**: Mark/unmark partner payment

### Lead Detail {#lead-detail}

**Location:** [Leads](/admin/leads) > Lead Detail

Comprehensive view of a single lead. A stat strip at the top (Total Revenue, Total Payout, Sales, Delivered) summarizes the lead at a glance, with the action buttons (Cherry Pick, Distribute, Force Charge, Scrub, Trash, Adjust Revenue/Payout) in the header above it. Below the strip, a tab bar switches between:

#### Home Tab
- **Contact Info**: Name, email, phone, address
- **Lead Details**: Vertical, partner, campaign, offer, cost, revenue — a collapsible section that summarizes total revenue and payout when collapsed
- **TCPA Consent**: Consent status and details
- **Raw Data**: Every non-system field from the partner POST, collapsed by default
- **Sales**: All buyers who purchased this lead (shows "No sales yet" when empty)
- **Notes**: Internal notes about the lead

#### Lead Info Tab
All lead fields displayed in table format, including custom fields.

#### Tracking Tab
- Partner and campaign attribution
- Sub IDs (Sub ID 1-5)
- Request ID for tracking
- System response JSON

#### Raw Fields Tab
Every non-system field received in the partner POST body, stored in `lead.custom`. Use this when a partner reports they sent a value but you don't see it on the structured fields above — if it's anywhere, it's here.

#### Validations Tab
- TCPA consent details
- Consent timestamp
- IP address
- Consent text/language

#### Dispositions Tab {#dispositions-tab}
Timeline of status changes showing:
- Disposition type
- Reason for change
- Date/time

#### Distributions Tab
The distribution waterfall showing every contract evaluation:
- Contract name and buyer
- Disposition (accepted, rejected, skipped)
- **Decided By** (`System` or `Buyer`) — see below
- Rejection reason
- Price offered
- Post timestamp

The disposition reason for non-winning contracts reflects the offer's distribution rule, not a blanket "Outbid" label. Price-based offers show `Outbid: $X vs winner $Y` so you can see the bid gap. Priority, weight, and round-robin offers show `Lower priority — not selected`, `Lower weight — not selected`, and `Not selected in rotation` respectively — price isn't the selection criterion on those offers, so calling a contract "outbid" would be misleading. Historical waterfall rows are preserved as-written.

On multisell and hybrid offers, every contract that successfully posts to its buyer is shown as `delivered` in the waterfall — not just rank 1. Previously only the highest-ranked winner was marked delivered and the rest stayed as outbid/not_selected even after they successfully posted, which made the waterfall under-count deliveries. Now the state transitions correctly as each rank delivers. Over-cap candidates (a contract that delivered successfully but was dropped because `maxSales` was already reached) keep their original waterfall disposition rather than being incorrectly marked failed.

##### Decided By: System vs Buyer {#distribution-decided-by}

Every waterfall row answers: *whose logic produced this outcome?*

- **System** — our routing engine made the call pre-post, with no HTTP round-trip to the buyer. Includes internal dedup hits, filter mismatches (geo/demographic/custom), cap exceeded, insufficient balance, schedule gate, HTTP timeout / network failure (buyer never actually responded), and admin manual overrides (cherry-pick, bulk cherry-pick, redistribute eval rows). The buyer's response-mapping rules were never consulted.
- **Buyer** — we POSTed to the buyer's endpoint, they returned a response, and our response-mapping rules classified it. Includes successful deliveries as well as rejections the buyer declared (returning `"status": "ERROR"`, `"DUPLICATE"`, etc. mapped through the contract's rules). Even a `200 OK` with an unparseable body or no matching rule counts as buyer — the buyer owns their response shape.

**Why it matters:** a row that reads `rejected / Duplicate Lead` is very different if it was our dedup blocking pre-post vs. the buyer's own system saying duplicate on their end. The `Decided By` column resolves the ambiguity at a glance. Filter by `?decidedBy=buyer` on the lead list to see only leads where a buyer actively made a call; filter by `system` to audit what our engine rejected before any external call went out.

### Lead Actions {#lead-actions}

| Action | Description |
|--------|-------------|
| Adjust CPL Payout | Change partner CPL cost for this lead |
| Adjust CPL Revenue | Change buyer CPL charge for this lead |
| Adjust CPA Revenue | Change buyer CPA charge (on a converted CPA sale) |
| Adjust CPA Payout | Change partner CPA payout (on a converted CPA sale) |
| Cherry Pick | Manually sell to a specific buyer |
| Distribute | Re-run distribution routing |

#### Cherry Pick options {#cherry-pick-options}

The Cherry Pick modal exposes six toggles that control which routing-engine guards run on the manual sale:

| Toggle | What it bypasses |
|--------|------------------|
| Skip Caps | Don't increment the contract's cap counter for this sale |
| Skip Dupes | Bypass the buyer-level dedup check (and the cross-partner dedup check) |
| Skip Filters | Reserved for filter bypass on bulk cherry pick |
| Skip Balance Check | Allow the sale even if the buyer's balance would go negative |
| **Skip Delivery** | Don't post the lead to the buyer's delivery endpoint. The `leadSale` (with a real `saleId`), `leadDistribution`, balance deduction, cap increment, and partner accrual all still run — only the outbound HTTP delivery is suppressed. Use this when the lead is being handed to the buyer out-of-band (manual hand-off, email, phone) but you still need a `saleId` so postbacks can attribute conversions. The sale's `dispositionReason` is stamped *"Cherry Pick: delivery skipped (manual postback)"* and `decidedBy` is `system` (no buyer endpoint was consulted). |
| **Fire Postbacks** | When OFF, the `lead.created` and sale webhooks do NOT fire for this cherry pick. Default ON. Use when manually re-keying a sale that was already posted out-of-band (and the partner's CRM has already been notified) — prevents double-firing partner postbacks. |

Skip Delivery and Fire Postbacks are independent — you can skip delivery while still firing postbacks (typical: out-of-band handoff that should still notify the partner), or you can fire delivery but suppress postbacks (typical: re-keying an already-notified sale).

##### Buyer Lead ID (Skip Delivery only) {#cherry-pick-buyer-lead-id}

When **Skip Delivery** is checked, an optional **Buyer Lead ID** text field appears below the toggles. This is for the case where the buyer already has the lead — typically because someone moved it manually at the intake center — and you want to record their identifier for it on the sale we're about to create.

**Why it matters:** the postback handler at `/api/v1/postback` matches sales by `buyerLeadId` when the `saleId` query param isn't a UUID. So if the buyer's CRM later fires `?saleId=EXT-12345&disposition=sold`, it'll find the leadSale you cherry-picked here and attribute the conversion correctly.

**Rules:**

- Only allowed when **Skip Delivery** is on. A real delivery would have its own buyer-returned ID (via response-mapping rules), and we don't want a hand-typed value to overwrite it.
- Single-lead only. Hidden in bulk cherry-pick because one ID can't fan out to N leads.
- Must be unique per buyer across non-`returned` sales. If a buyer already has an active sale with the same Buyer Lead ID, the cherry pick is rejected with a `400 VALIDATION_ERROR` naming the conflicting `saleId` and `leadId` — postbacks would otherwise be ambiguous. Recycling an ID after the prior sale is `returned` is allowed.
- Up to 255 chars. Whitespace is trimmed; empty/whitespace-only values are rejected.

### Adjusting sales {#adjusting-sales}

The **Adjust** menu on a lead or call sale retroactively edits dollars on an already-delivered sale. Two top-level buttons — **Revenue** and **Payout** — open the same modal, which exposes a **CPL / CPA** segmented toggle at the top. Combined with the button, that gives the four fully-independent modes: **CPL Revenue**, **CPL Payout**, **CPA Revenue**, **CPA Payout**. The CPL and CPA ledger sides never couple automatically. The CPA toggle is disabled on per-sale rows where the sale has no CPA amount/payout yet; on bulk it's always enabled and the server guards each sale individually.

**When to use:**
- Buyer negotiates a lower (or higher) CPL rate after delivery → **Adjust CPL Revenue**
- Partner agrees to a make-good on bad leads → **Adjust CPL Payout**
- CPA conversion fired with the wrong buyer charge → **Adjust CPA Revenue** (buyer-side only, does not touch partner)
- CPA payout was renegotiated after conversion → **Adjust CPA Payout** (partner-side only, does not touch buyer)
- Twilio billing reconciliation differs from the charged duration on a call sale → **Adjust CPL Revenue** (call)
- Manual true-up after a dispute is resolved

**What it does:**
- Edits the sale row directly (`leadSale.price`, `leadSale.cpaAmount`, `leadSale.cpaPayout`, `callSale.totalPrice`, `callSale.payout`, etc.)
- Recalculates the lead/call aggregate (`lead.revenue`, `lead.cost`) from `SUM(sale)` so reports stay consistent
- Deltas the buyer balance (revenue/CPA-amount edits) and/or the partner balance (payout/CPA-payout edits)
- Writes a `transaction` row with `type = 'adjustment'` — old vs. new amount preserved via `balanceBefore` / `balanceAfter`
- Inserts a disposition row with the human reason you typed
- Fires `lead.sale_adjusted` or `call.sale_adjusted` webhook so subscribed buyers know the invoiced amount changed
- Logs an `adjust_billing` audit entry

**What it does NOT do:**
- **Cap counters are not rewound.** Caps are directional by design — if an adjustment drops a price to zero, the cap spend stays at the original amount. For voids or full reversals, use **Trash** or **Refund** instead.
- **Misc-cost rows are not recalculated.** Existing misc-cost rows from the original sale stay in place for forensics.
- **Partner revshare is not auto-synced when revenue changes.** If you edit CPL revenue from $40 → $30 on a revshare-paid sale, the payout stays the same until you edit payout too. The two ledger sides are intentionally independent and explicit.

**When to use Refund or Trash instead:** Adjustments are for *amount edits* on a sale that still stands. For full reversals (lead rejected, sale voided), use the refund flow or trash the lead. Don't adjust a sale to $0 as a substitute for a refund.

**Bulk:** Select N leads on the list page → **Adjust Revenue** or **Adjust Payout**, then use the in-modal CPL/CPA toggle to pick the side. Call-side adjustments stay per-sale to reduce blast radius on wrong blanket amounts.

**Overdraft:** If increasing revenue would push the buyer's balance negative, the adjustment still proceeds — it represents an already-occurred economic truth. The resulting transaction is stamped `creditStatus: 'overdraft'` and surfaced in the modal response so you can follow up with the buyer.

**Optimistic lock:** The modal shows the current amount before you submit. If another admin adjusts the same sale in parallel, the second submit returns a 409 Conflict with the new current value so you can re-review instead of double-applying the delta.

<!-- react-flow:adjustment-flow -->

### Refund Requests {#refund-requests}

**Location:** [/admin/refunds](/admin/refunds)

When a buyer requests a lead return from their portal, it lands here as a pending refund. The list is tabbed by status (Pending, Approved, Rejected); click a row to review the lead, buyer, reason, and the buyer's notes.

**Approving or rejecting:** Click **Approve** or **Reject** on a pending refund. A confirmation dialog opens with a **Note to buyer** field — whatever you write here is sent to the buyer with the decision (it's included in the decision email and shown highlighted in their portal until they dismiss it). The note is optional for approvals; for rejections, explain why the return didn't qualify so the buyer isn't left guessing. Maximum 2,000 characters.

On approval the system marks the sale returned, credits the buyer (wallet balance or prepaid SKU seat, depending on how the lead was funded), reverses the partner payout, and decrements contract caps. Rejection changes nothing financially — only the status and your note.

### Lead Lifecycle

A lead moves through the following states from submission to final disposition.

<!-- react-flow:lead-lifecycle -->

### Demographics {#lead-demographics}

Leads can carry **gender**, **ethnicity** and an **age band**. These support reporting
questions like "how many leads did we retain for each age range", broken down by
vertical or traffic source.

<!-- react-flow:lead-demographics -->

**Where the values come from.** They can arrive with the lead at submission, but more
often they arrive later, on a disposition postback from the intake partner — after the
lead has been qualified. A postback fills any value that is still empty and never
overwrites one that is already set, so a later correction does not replace a good
value. Sending demographics on a disposition the system has already recorded still
works, which is what makes a backfill of older leads possible.

**Age band, not date of birth.** Date of birth is stored encrypted, which means reports
cannot group by it. The age band is worked out from the date of birth and stored
alongside it so age *is* reportable. It is calculated as of the day the lead was
created — not the day the date of birth arrived — so a lead's band reflects the
person's age at the time they came in. Bands are:
`0-17 · 18-24 · 25-34 · 35-44 · 45-54 · 55-64 · 65+`.

If a date of birth is missing or unreadable, no band is stored. Nothing is guessed — a
wrong band would be indistinguishable from a real one in a report.

**Which fields your account has.** Gender is available to every account. Ethnicity and
age band appear only where they have been enabled for your organisation under
**Settings → Lead Fields**. If a field is not enabled, values sent for it are ignored
rather than stored.

**Who can see them.** On an individual lead, gender and ethnicity are hidden from staff
without PII access; the age band stays visible. In reports, gender is available to
everyone, while ethnicity and age band are offered only to staff with PII access — so
someone who cannot look up one person's ethnicity also cannot work it out from a
one-row breakdown.

**Reporting.** Use the **Demographics** report template for retained counts and retain
rate by gender, or add Gender, Ethnicity and Age Band as breakdowns to any Leads or
Sold Leads report. Where a campaign never collected a value, those leads are reported
as not collected rather than being folded into an "unknown" group.

Demographics are never included in the data sent onward to buyers.

---

## Buyers

**Location:** [/admin/buyers](/admin/buyers)

Buyers are companies that purchase leads through contracts.

### Buyer List

| Column | Description |
|--------|-------------|
| ID | System identifier |
| Name | Company name |
| Email | Primary contact email |
| Status | Active, Paused, Suspended, Inactive |
| Balance | Current account balance |
| Created | Account creation date |

### Creating a Buyer

**Location:** [/admin/buyers/new](/admin/buyers/new)

The create form is a slim identity form — everything else is configured on the buyer detail page after creation.

**Required Fields:**
- Company (also used as the buyer name)

**Optional Fields:**
- Email
- First Name, Last Name
- Phone
- Status (Active / Inactive)

Address, billing, deduplication, and brand settings live on the buyer detail page.

### Buyer Detail {#buyer-detail}

**Location:** [Buyers](/admin/buyers) > Buyer Detail

**Health strip** (visible on every tab): current **Balance**, **active Contracts**, **Leads today**, and **Spend today** (today = your tenant's timezone; returned sales and test leads excluded).

**One save bar:** every field edit on the Home tab feeds a single floating save bar that appears when something changed and sends only the changed fields. Cancel reverts to the last saved values. Invalid fields (flagged as you leave them) disable Save until fixed.

#### Home Tab

**Status:**
- Active: Receiving leads
- Paused: Temporarily stopped
- Suspended: Account issue
- Inactive: No longer active

Setting status to Inactive asks whether to also expire the buyer's contracts and (if any) pause its active Stripe subscriptions.

Pending buyers (self-signup) show Approve / Reject actions instead of the status selector.

**Contact Information:** ID, Company, Website

**Partner Response:** buyer name / ID / phone, redirect URL, and photo or video returned to partners (see below).

**Address, Legal & Tax, Management, Billing, Brand** sections cover the remaining profile fields (sales rep, account manager, timezone, billing cycle, payment terms, brand assets).

**Deduplication Settings:**
- **Dedup Enabled**: Toggle duplicate checking
- **Dedup Fields**: Select which fields to check (email, phone, firstName, lastName, address, zipCode)
- **Window**: Number of days to check for duplicates

**Financial Settings:**
- **Balance**: Current balance (read-only)
- **Credit Limit**: Maximum negative balance allowed
- **Auto Recharge**: Enable automatic balance top-up
- **Auto Recharge Amount**: Amount to add when triggered
- **Auto Recharge Threshold**: Balance level that triggers recharge

#### Contracts Tab
View and manage all contracts for this buyer. Click "Add Contract" to create a new contract pre-linked to this buyer. If the buyer is not active, a banner warns that routing skips all of its contracts.

#### Leads Tab
History of all leads sold to this buyer showing date, price, and delivery status.

#### Transactions Tab
Financial transaction history (shown when the Finance module is enabled):
- Type: Sale, Refund, Payment, Adjustment
- Amount and balance changes
- Reference notes

#### SKUs Tab
Shown when the SKU/Product system is enabled (or the buyer has historical packs). Pack inventory, funding history, and money actions — refunds, refund-to-credit, free units, reissue, and subscription pause/resume/upgrade — all confirm in their own dialogs and apply immediately (they are not part of the page save bar).

#### Users Tab
Portal members and pending invitations. Invite users, send login links, and bulk-activate/deactivate members.

#### Partner Response — Buyer Photo or Video

Optional buyer photo or short video shown to partners alongside the lead-submit response (e.g. a face shot or brand intro that helps the partner's contact center close the lead).

- **Where to upload:** Buyer Detail > **Partner Response** section > **Buyer Photo or Video**.
- **Supported formats:** JPG, PNG, or WebP photos up to 10 MB; MP4 or WebM videos up to 50 MB.
- **How partners receive it:** when a campaign's **Send Buyer Media** toggle is enabled, the partner's `POST /api/v1/leads/submit` response includes `buyerMediaUrl` and `buyerMediaType` (`'photo'` or `'video'`).
- **Toggle location:** set per-campaign on Campaign Detail, or as a default on the parent Offer. The toggle is **off by default** — partners will not see the URL until you turn it on for the campaign they post to.

The buyer's phone and redirect URL are set the same way (Buyer Detail > Partner Response, or by the buyer themselves — see [Lead Completion Experience](#buyer-lead-completion)) and are gated by the **Send Buyer Phone** / **Send Redirect URL** campaign toggles.

##### Redirect URL & Calendar Link Detection {#partner-response-calendar}

When **Send Redirect URL** is enabled, the sold-lead response includes `redirectUrl` plus a `redirectType` field:

- If the buyer's redirect URL is a recognized scheduling link (Calendly, Cal.com, or Google appointments), `redirectType` is `'calendar'` and the response also carries `calendarProvider` (`'calendly'`, `'cal.com'`, or `'google'`) and `calendarEmbedUrl` — an iframe-embeddable booking URL. Partner landers can render the buyer's calendar inline on the completion page instead of redirecting.
- For any other URL, `redirectType` is `'redirect'`, the calendar fields are omitted, and the partner redirects the consumer to `redirectUrl` as usual.

Detection runs on the **rendered** URL (after `{{leadId}}` / `{{partnerId}}` token substitution) at response-build time. The public posting spec page documents the embed snippet for partner developers.

---

## Contracts

**Location:** [/admin/buyers/contracts](/admin/buyers/contracts)

Contracts define how buyers receive and pay for leads.

### Contract List

| Column | Description |
|--------|-------------|
| Name | Contract identifier |
| Buyer | Associated buyer |
| Vertical | Lead category |
| Priority | Routing priority (higher = first) |
| Price | Cost per lead |
| Daily Cap | Maximum leads per day |
| Status | Active, Paused, Expired |

### Creating a Contract {#creating-a-contract}

**Location:** [/admin/buyers/contracts/new](/admin/buyers/contracts/new)

Creating a contract captures **identity only** — pricing, caps, filters, schedule, delivery, and calls are all configured on the [contract page](#contract-detail) after it's created.

**Step 1: Select Vertical**
You must select a vertical first. This determines which leads the contract can receive. If the vertical has a vertical group assigned, it will be displayed below the selection for reference.

**Step 2: Product (optional)**
Only shown for tenants with the SKU / Product system enabled. Selecting a product prefills the contract from the product (and locks the sell-type selector to the product's type).

**Step 3: Identity**
- **What does this contract sell?** (required) — **Leads / Calls / Both** chip selector. This sets the contract's capabilities (Lead Sales and/or Call Tracking), which decide which tabs the detail page shows. CPA Revenue is never asked at create — enable it later from the Details tab's Capabilities card.
- **Contract Name**: Descriptive identifier
- **Buyer**: Select the buyer (required)
- **Priority**: Higher numbers get leads first (0 = lowest)
- **Rule Type**: Exclusive or Multi-Sale
- **Notes**: Internal notes

**Starts in Setup:** a new contract is created in **Setup** status. Before it can be activated, the activation gate must be met — a lead-selling contract needs a delivery method, and a call-only contract needs a transfer phone number. The Details tab shows an inline hint explaining exactly what's missing (see [Contract Detail](#contract-detail)).

### Contract Detail {#contract-detail}

**Location:** [Contracts](/admin/buyers/contracts) > Contract Detail

Settings are grouped into eight workflow-ordered tabs — identity first, then economics, who gets the lead, how it's delivered, the call channel, the post-delivery lifecycle, funding & fees, and the audit trail:

- **Details** — the always-visible control surface: the **Capabilities** card, **Spend Caps** (the contract-level budget), basic info (ID, name, status, rule type, priority, buyer/vertical links, notes), Connected Offers, and a collapsed Legacy & CRM section.
- **Leads** — CPL economics: Lead Revenue (pricing model + price), Ping Configuration (Ping/Post contracts), and Lead Caps.
- **Routing** — who can win and when: geo state/zip filters, custom filters, dedup override, and the Schedule (timezone, hours grid, expiration). Replaces the old separate Targeting and Schedule tabs.
- **Delivery** — delivery methods (see [Delivery Tab](#delivery-settings)).
- **Calls** — phone numbers, call billing, whisper/recording, call caps (see [Call Settings on Contracts](#call-settings-on-contracts)). The Call Tracking on/off toggle itself lives on Details → Capabilities.
- **Conversions** — the post-delivery lifecycle: CPA revenue, postback URLs, and disposition mappings.
- **Finance** — funding in, fees out: the Contract Funding panel (SKU-funded contracts) followed by Misc Costs.
- **Activity** — audit log.

**Capability-conditional tabs.** Tabs hide when their functionality is off, driven by the contract's capability flags:

| Tab | Visible when |
|---|---|
| Details, Routing, Finance, Activity | always |
| Leads, Delivery | Lead Sales or CPA Revenue is on (any lead billing) |
| Calls | Call Tracking is on |
| Conversions | CPA Revenue or Call Tracking is on (dispositions serve call billing too; the CPA sections show an explainer when CPA is off) |

A call-only contract shows `Details | Routing | Calls | Conversions | Finance | Activity`; a plain lead contract shows `Details | Leads | Routing | Delivery | Finance | Activity`. Old bookmarked tab links keep working — legacy tab names redirect to their new home, and a link to a hidden tab falls back to Details.

**The Capabilities card** (top of Details) is where you turn each channel on or off:

- **Lead Sales** — the contract buys leads on acceptance (CPL).
- **Call Tracking** — the contract accepts inbound calls (moved here from the old Call Settings tab; turning it off still triggers the last-call-contract confirmation when applicable).
- **CPA Revenue** — the contract bills on CPA conversion. This is the **only** place CPA Revenue is turned on or off; the Conversions tab displays the state (fields when on, an "Enable CPA Revenue on Details →" link when off).
- **Allow $0 Conversions** — appears just under CPA Revenue once that is on. Off by default. Turn it on only for **cost-plus** contracts, where the buyer pays the intake center directly instead of paying us: the conversion is worth $0 to us but is still real. With it on, a $0 conversion is recorded normally and counts in **CPA Events** and **RR%**, but no money moves — no balance change and no ledger transaction is written. With it off (the normal case) a $0 CPA is rejected with "CPA amount is 0", because a zero rate almost always means the CPA amount is misconfigured.

  Note the two questions reports answer differently: retainer **counts** include $0 conversions, while **revenue** reports (Time of Revenue, revenue pivot/export/day) do not — a $0 retainer contributes no revenue, so it correctly adds nothing there.

  To stop a contract billing CPA entirely, turn **CPA Revenue** off (or end the contract) — do **not** set the CPA amount to `0` as a way of disabling it.

Flipping a capability on reveals its tab(s) immediately.

**The Save bar is shared across tabs:** it appears whenever there are unsaved edits, no matter which tab you're on, and saves exactly the fields you changed. Cancel restores the last saved values.

**SKU-funded contracts** render additional surfaces automatically (funding chip, Contract Funding panel on Finance, fill-headroom strips) and lock the fields the SKU manages — see [Field-locking on SKU-funded contracts](#skus-tenant-feature). Standalone contracts get the clean view with everything editable.

<!-- react-flow:contract-setup-flow -->

#### Details Tab {#basic-info-tab}

Identity and the contract-wide controls:

- **Capabilities** — see above.
- **Spend Caps** — daily / weekly / monthly **budget** for the whole contract, consumed by lead, call, and CPA revenue alike (that's why it lives on the always-visible Details tab — a call-only contract keeps its budget controls even though the Leads tab is hidden). When a period's spend reaches its cap, the contract stops winning new leads and calls until the period resets. Late CPA conversions still bill and count even if they push spend past the cap — earned revenue is never rejected; the exhausted budget blocks *future* deliveries instead.
- **Basic Information** — ID (read-only), Contract Name, Status, Rule Type (**Exclusive** or **Multi-Sale** — Multi-Sale contracts can participate in multisell offers), Priority, Buyer / Vertical links, Notes.
- **Activation hint** — when the contract can't be activated yet, an amber hint under Status explains the blocker with a link to fix it: a lead-selling contract needs a delivery method ("Configure delivery →"), a call-only contract needs a transfer phone number ("Add transfer number →"). The server enforces the same gate.
- **Pause confirmation** — changing Status to Paused on a contract with an active Stripe subscription opens a confirmation spelling out the cascade (subscription paused, open invoices voided) before saving.
- **Connected Offers** — read-only list of offers this contract is linked to.
- **Legacy & CRM** — collapsed section for legacy/CRM identifiers.

#### Leads Tab {#pricing-caps-tab}

CPL economics — visible when the contract has any lead billing:

- **Fill-headroom strip** (SKU-funded contracts only) — a compact strip at the top: **Fillable now** (the smaller of pack remaining and today's cap headroom), today's cap usage, pack remaining, and a link to the Finance tab.
- **Lead Revenue** — Charge on Lead Acceptance, pricing model (**Fixed** or **Ping/Post**), Price Per Lead (or Minimum Bid Floor for Ping/Post), exclusive multiplier.
- **Ping Configuration** — shown for Ping/Post contracts: ping URL, timeout, response field mapping, test ping, stored ping results.
- **Lead Caps** — daily / weekly / monthly lead **volume** caps. Caps are pacing; funding (the SKU pack or wallet balance) is total volume — both apply.

CPA revenue is configured on the **Conversions** tab (a pointer card on Leads links across). Spend Caps live on **Details**.

#### Conversions Tab {#conversions-tab}

Everything about what happens *after* delivery — visible when CPA Revenue or Call Tracking is on:

- **CPA Revenue** — the CPA amount and conversion window (the enable toggle lives in Capabilities on the Details tab — the card here shows a note pointing to it). CPA Revenue is *lead* CPA — it fires on postback for delivered leads and makes the contract a lead-delivery contract (a delivery method is required before it can activate). Call CPA is separate: configure it under the Calls tab (conversion type / call billing rules). A call-only contract (call enabled, no lead billing) activates once it has a transfer phone number instead of a delivery method. When CPA is off but the tab is visible for call dispositions, the CPA card shows an explainer with an "Enable CPA Revenue on Details →" link instead of the fields.
- **CPA Auction Pricing (RPL)** — revenue-per-lead lookback pricing for CPA auction contracts.
- **Postback URLs** — the buyer's conversion postback URLs, plus a collapsed parameter reference table.
- **Disposition Mappings** — map buyer-reported dispositions to system outcomes (previously its own Dispositions tab).

##### CPA Reversal on Return {#cpa-reversal-on-return}

When a postback fires a disposition with **Triggers Return = true**, the system can reverse the lead sale (CPL refund) and/or any prior CPA conversion on the same sale. The new **Return Scope** select on each disposition controls *what* gets reversed.

**Where to set it:** Admin → **Vertical Groups** → *(group)* → **Dispositions** tab → toggle **Return** on → pick **Return Scope**.

**Three values:**

| Return Scope | DB value | What reverses on this disposition |
|---|---|---|
| All (default) | `null` | Both the CPL lead-sale (refund row + `deliveryStatus='returned'`) AND any `cpaStatus='converted'` CPA conversion |
| CPL only | `'CPL'` | Only the lead sale (existing pre-2026-05-02 behavior) |
| CPA only | `'CPA'` | Only the CPA conversion — `leadSale.deliveryStatus` and the refund table are untouched |

**What the CPA reversal touches:**

- `leadSale.cpaStatus` flips from `'converted'` → `'reversed'`
- `cpaAmount` and `cpaPayout` columns are preserved on the row for audit (so you can still see what the conversion was worth)
- `lead.cpaRevenue` and `lead.cpaPayout` aggregates are decremented (allowed to go negative on edge cases — represents truth, not error)
- `buyer.balance` is credited back the full `cpaAmount`
- `partner.balance` is debited the full `cpaPayout` — **allowed to go negative** because it represents a partner-owed-back position; collect via the next payout cycle
- Two offsetting `transaction` rows are written with `type = 'cpa_reversal'` (one buyer-side credit, one partner-side debit) — both link to the original CPA `transaction` rows so the ledger reconciles
- Any `miscCostCharge` rows from the CPA conversion are reversed via `type = 'misc_cost_reversal'` and stamped with `reversedAt` + `reversalTransactionId` for audit
- The `cpa.reversed` webhook fires (plus the type-specific `lead.reversed` or `call.reversed` event) so subscribed buyers can update their CRM
- An audit-log entry with `action = 'cpa_reversal'` is written

**Re-conversion is allowed.** If a later disposition with **Triggers CPA = true** fires after a reversal on the same sale, the engine allows `'reversed' → 'converted'` re-fire — fresh `transaction` rows are written and the ledger updates again. This handles the legitimate "we reversed by mistake, let's re-convert" or "lead unsigned then re-signed" workflows.

**Caveat — caps are NOT decremented.** CPA reversal mirrors the existing rule that CPA *fires* don't increment caps: reversals likewise don't decrement them. Caps track delivery, not conversion outcome.

**Cannot mix CPA + Return on the same disposition.** A DB CHECK constraint enforces `NOT (triggersCpa AND triggersReturn)` — a single disposition can't both fire CPA and reverse it in one shot. Use two separate dispositions for that flow.

**Behavioral change — first postback after deploy.** Existing dispositions with `triggersReturn=true` ship with `returnScope = NULL`, which means **both CPL and CPA reverse** on the next postback. If you want to keep the prior "CPL-only" behavior on a specific disposition, edit it in the admin UI and set Return Scope to `CPL only` before reposting.

#### Delivery Tab {#delivery-settings}
Configure how leads are delivered to buyers. A contract can have multiple delivery actions (e.g., HTTP POST + email notification), each configured independently.

##### Delivery Action Types

###### HTTP POST {#delivery-http-post}

- URL endpoint
- Format: JSON, XML, or Form
- Timeout and retry settings
- Authentication: None, Basic Auth, Bearer Token, API Key
- Custom headers
- Field mapping (map your fields to buyer's expected fields)
- Response mapping (define success/error conditions)

###### Email {#delivery-email}

Sends lead data as a formatted HTML email. Configure:
- **To**: One or more recipient email addresses
- **CC**: Optional CC addresses
- **Subject**: Subject line (supports token placeholders)
- **Body**: Rich text editor for composing the HTML email body

Token placeholders like `{firstName}`, `{lastName}`, `{email}` are replaced with actual lead field values at delivery time. Any lead field, vertical field, or contract custom field can be referenced as a token. Email delivery is available in both contract and intake center delivery settings.

###### Google Sheets (OAuth) {#google-sheets-oauth}

Delivers each lead as a new row in a Google Sheet that lives in the buyer's own Google Drive. Uses per-delivery-method OAuth — every Google Sheets delivery action gets its own connection, so different sheets can live in different Google accounts on the same contract.

<!-- react-flow:google-sheets-oauth-flow -->

**Prerequisites:**
- Admin role (the Connect button is hidden from buyer/partner users)
- A Google account with access to the destination Drive (personal or Workspace, both work)
- Pop-ups allowed for `theleadrouter.com` — we open the consent screen in a popup; if blocked we fall back to a full-page redirect

**Connecting a sheet:**
1. Open the contract detail page → **Delivery** tab → **Add Delivery Action** → **Type: Google Sheets**
2. Click **Connect Google** — a Google sign-in popup opens
3. Pick the Google account you want the sheet to live under, then **Allow** the requested permissions
4. The popup closes automatically. The card now shows **Connected as `<email>`**
5. Click **Pick Sheet** — the Google Picker opens
6. Either pick an existing spreadsheet or click **New** to create one. The sheet is created in the granting user's Drive
7. (Optional) Edit **Sheet/Tab Name** — defaults to `Leads`
8. Click **Test connection** to write a `[test row]` and verify the connection works
9. Save the contract

**What gets written to the sheet:**
- The header row is auto-created on the first append if the sheet is empty
- Each lead becomes one row, in the order defined by the field mapping editor
- Field transformers (date format, phone format, casing, etc.) work the same as HTTP POST delivery

**Field mapping order — drag to reorder, locks on save:**
- Each mapping row has a drag handle on the left. Drag rows up/down to set the column order before you save the contract.
- The first time you save the contract with a non-empty mapping, the saved rows **lock**: the column-name input goes read-only, the remove button is replaced by a lock icon, and you can no longer drag those rows. This prevents shifting columns in a sheet that's already collecting live data — existing rows would mis-align if columns moved.
- You can still:
  - Add new columns at the end (they remain freely reorderable among themselves until the next save commits them).
  - Change the **lead field** (right-side dropdown) on a locked row — re-aiming which lead value flows into a saved column is allowed since it doesn't change column position.
  - Edit transformers (the **fx** button) on a locked row.
- You cannot:
  - Reorder, rename, or remove a saved row.
  - Use **Match to sheet** or **Clear and start over** once any row is locked (both are hidden — they would change the saved order).
- If you need to change a saved column's name or position, work around it: add a new column at the end with the desired name, leave the old column in place (it'll continue receiving data), and migrate consumers of the sheet over to the new column.

**If the connection breaks:**
- **Token revoked** (user removed app access at `myaccount.google.com/permissions`) → next delivery sets the connection to `revoked`, the card shows **Reconnect** banner, and no further deliveries are attempted until the admin reconnects
- **Sheet deleted** in Drive → next delivery sets `lastErrorMessage="Sheet deleted"`, card shows an error banner, admin clicks **Pick Sheet** to choose a new one
- **Token expired** mid-delivery → automatically refreshed using the stored refresh token, no admin action required

**Disconnect button:**
- Revokes the refresh token at Google (the user's `myaccount.google.com` no longer shows our app)
- Marks the connection `revoked` locally
- The delivery method stays configured (spreadsheet ID, sheet name, field mapping preserved) — reconnecting reuses the same row
- Audit log entry: `google_sheets.disconnect`

**Security notes:**
- Sheets live in the granting user's own Drive, not in any service account
- We request the `drive.file` scope — we can only see files our app created or that the user explicitly picked via the Picker. We cannot list or read other files in the user's Drive.
- Refresh and access tokens are encrypted at rest with AES-256-GCM (`CREDENTIALS_ENCRYPTION_KEY`)
- Every connect, disconnect, refresh failure, and sheet pick is recorded in the audit log; tokens never appear in any audit row
- Both refresh and access tokens are wiped on disconnect
- A nightly cron (`/api/cron/orphan-google-sheets-credentials`) revokes connections whose delivery method has been removed from the contract

**Default recipients (tenant-wide):**

Set on the Google Sheets provider card under [Integrations](#integrations) → **Default Recipients**. These email addresses automatically get **viewer (read-only)** access to every Google Sheet created for the tenant, on top of the buyer, who gets editor access. Use it for internal oversight staff — account managers who need visibility into all buyer sheets. Up to **20 addresses**.

Removing an address here does **not** revoke access to sheets that already exist; revoke those from Google Drive directly.

**Legacy service-account flow (deprecated, removed 2026-05-25):**
The previous Google Sheets integration used a shared service-account JSON file with `share-with-email` access. Any delivery method still on that flow shows a yellow **Legacy Google Sheets connection** banner with a **Reconnect with OAuth** button. The legacy code path keeps delivering until the removal date — connect via OAuth before then to avoid an interruption.

**Sheet mapping templates — reusable column layouts per vertical:**

If most contracts in a vertical share the same Google Sheets column layout, save it once as a template and load it into every new contract instead of re-building the mapping by hand. Templates are scoped to the contract's vertical — a "Solar buyer columns" template saved under the Solar vertical only appears for other Solar contracts.

**Saving a template:**
1. On the contract's Google Sheets delivery config, build the field mapping the way you want it (column names, lead-field bindings, order)
2. Click **Templates ▾** (next to **Use all vertical fields** / **Use default columns**) → **Save as template…**
3. Give it a name (e.g. `Standard Solar Mapping`) and save. The template captures the current column → lead-field rows in their saved order.

**Loading a template onto another contract:**
1. On a different contract in the same vertical, open the same Google Sheets delivery config
2. Click **Templates ▾** — saved templates for the vertical appear in the dropdown
3. Click a template name to load it. The current mapping is replaced with the template's columns, in the template's order. There is no live binding — editing the template later does not push changes to contracts that already loaded it.

**Renaming or deleting templates:** Click **Templates ▾** → **Manage…**. Rename inline, or delete with confirmation. Deleting a template does not affect contracts that previously loaded it (the mapping was copied in, not referenced).

**Pre-applying a template via the onboarding wizard:** When you build an onboarding wizard that includes a **Delivery Picker** step with Google Sheets as an allowed channel, you can optionally bind a sheet template to that step. When a buyer completes the wizard with Google Sheets selected, the chosen template's mapping is copied into the new contract's delivery action automatically. Wizards aren't vertical-scoped, so the wizard builder lists every template flat, grouped by vertical name in the option label. If the bound template is deleted before the buyer completes the wizard, the contract is created with an empty mapping and a warning logged — the wizard completes successfully either way.

###### FTP {#delivery-ftp}

- Host and path
- Credentials

###### SMS (Transactional) {#sms-delivery}

Sends a short plain-text message to a phone number via a shared system Twilio sender. Intended for **buyer notifications and internal alerts** — not consumer outreach. This is separate from the Messaging app's consumer SMS (`/admin/messaging/*`), which supports per-tenant Twilio/Bandwidth senders, campaigns, opt-out tracking, and inbound replies. Transactional SMS is one system-level Twilio account, no inbound webhook, and every tenant's contract deliveries flow through it.

**Configuring the system Twilio account** (superadmin, one-time):
1. Navigate to `/superadmin/telephony` → Twilio card → **Configure**
2. Enter **Account SID**, **Auth Token**, **From Number** (E.164, e.g. `+15551234567`), optional **Messaging Service SID** (sender pool — overrides From Number at send time if set)
3. Save. Voice and transactional SMS share this one credential.

**Managing SMS templates** (superadmin): `/superadmin/sms`
- Create, edit, delete reusable templates with a unique **slug**, a **body**, and optional **description**
- Body supports the same token palette as email: `{{firstName}}`, `{{lastName}}`, `{{leadId}}`, `{{offerName}}`, plus any lead field, vertical field, or contract custom field
- Unknown tokens render literally (not blank) so missing data is visible, not silent

**Adding SMS to a contract:**
1. Open the contract detail page → **Delivery** tab
2. Click **Add Delivery Action** → **Type: SMS**
3. Fill:
   - **Send to** — radio selector (added 2026-05-06):
     - **Specific number** (default) — uses the **Phone Number** field below. Pick this for buyer CRM, ops alerts, internal routing.
     - **Lead's phone** — sends directly to the matched lead's `phone` field at delivery time. Pick this for consumer-facing notifications. The Phone Number field is hidden in this mode. Lead phones are normalized to E.164 before send, so common US formats like `7044957507`, `17044957507`, or `(704) 495-7507` become `+17044957507`. Sends are skipped (logged with `PHONE_INVALID`) when `lead.phone` is missing or cannot be normalized. **Consent is enforced upstream at intake** (TCPA gate at lead capture / posting) — there is no per-send TCPA check in the SMS dispatcher.
   - **Phone Number** (E.164, required when **Send to = Specific number**)
   - **Message Body** (required, up to 1,600 chars — char counter + SMS segment count shown inline; ≤160 chars = 1 segment, else `ceil(length/153)`). Type `{{` to surface inline token suggestions (see below).
   - **Template** (optional — picks a saved template; its body becomes the message body)
   - **From Number Override** (optional, E.164 — overrides the system From Number / Messaging Service SID for this contract only)
4. Save. Primary/secondary semantics work the same as email — SMS can be primary.

**Provider resolution:** When **Send to = Lead's phone**, the dispatcher routes through the unified `sms_messaging` resolver (whichever provider the tenant has set as default for `sms_messaging` in `/admin/system/providers`). The legacy fixed-number path uses `sms_delivery`. Both fall through to system Twilio when no tenant-specific assignment is configured.

**Inline token autocomplete:** Type `{{` anywhere in the **Message Body** to surface a dropdown of available merge tokens (lead fields, vertical fields, contract custom fields). Use ArrowUp / ArrowDown to navigate, Enter or Tab to insert, Esc to dismiss. Same pattern as the email subject autocomplete — replaces the older click-a-chip workflow.

**10DLC registration (pre-ship requirement):** US carriers heavily filter unregistered A2P traffic. Before enabling transactional SMS in production, register an **A2P brand + campaign** in Twilio Trust Hub. Without registration, messages may silently drop at the carrier with no error returned to the system. Brand approval typically takes 1–3 weeks; start the process in parallel with rollout.

**Retry behavior:** Same as HTTP POST delivery — 3 attempts with exponential backoff on Twilio 5xx / network errors. 4xx responses (bad number, unregistered sender, opted-out recipient) are terminal and do not retry. Every attempt lands in `postingLog`; the final outcome updates `leadDistribution` and the lead sale row.

##### GSM-7 Normalization (automatic) {#sms-gsm7-normalization}

Carriers bill SMS by segment, and the character set decides how big a segment is. A message made entirely of GSM-7 characters gets **160 characters per segment**; a single character outside that set (one curly quote pasted from a word processor, one emoji) flips the **whole message** to UCS-2 at **70 characters per segment** — roughly 4x the cost for the same text.

Every SMS body on the platform is therefore normalized to GSM-7 automatically. You do not need to do anything.

**What changes:**
- Curly quotes become straight quotes, en/em dashes become hyphens, an ellipsis becomes three dots
- Non-breaking and zero-width spaces become ordinary spaces or are removed
- Accented characters that are *not* in the GSM-7 set fold to their base letter (`María` → `Maria`)
- Emoji, CJK and other characters with no GSM-7 representation are dropped

**What does NOT change:** accents that *are* GSM-7 characters are kept exactly as typed — `é ñ ö ü à Ä Ö Ñ Ü Ç É £ €` all survive. `José` stays `José`.

**Where it applies:** SMS templates (tenant and global), contract SMS delivery actions, the buyer portal's self-service message editor, onboarding wizard message overrides, provider test sends and SKU share messages. Normalization happens twice: once when you save, and again at send time **after** merge tokens are filled in — that second pass is what handles an accented lead name arriving in `{{firstName}}`.

The character counter under every SMS body shows the segment count for the normalized text, so the number you see is the number you are billed for. If what you typed will be changed, the counter says so.

**Length cap:** the final message — after tokens are merged — is capped at 1,600 characters (10 segments). Anything past that is truncated rather than sent as an unbounded message.

**Media/MMS:** the SMS path never sends media. Adding MMS is a deliberate product decision, not something a message body or an integration can trigger implicitly.

##### SMS Opt-Out & Suppression {#sms-opt-out}

Broker (contract) SMS is TCPA-suppressed end to end. Three mechanisms keep opted-out numbers from being messaged:

**1. Recipient replies STOP.** An inbound webhook (`/api/v1/sms/webhooks/inbound`, Twilio + SignalWire) receives the reply, validates the provider signature, and writes a tenant-scoped `suppressionEntry` (hashed phone, `reason: opt_out`). Keywords: **STOP / STOPALL / UNSUBSCRIBE / CANCEL / END / QUIT** opt out; **START / UNSTOP / YES** resubscribe (deletes the opt_out rows). The carrier sends its own confirmation reply — we return empty TwiML, never a second message.

**2. Provider reports error 21610.** Twilio/SignalWire refuse to deliver to a STOP'd number and return code **21610** on the status callback (`/api/v1/sms/webhooks/status`). We update the `deliveryLog` row to `failed` and automatically write a suppression entry for that tenant — no reply needed. This catches numbers that opted out before we ever tracked them.

**3. Pre-send gate.** Before every broker SMS send, `isPhoneSuppressed` checks the recipient against suppression entries for the sending tenant (plus any platform-wide `tenantId = null` rows). A hit is logged to the delivery log as `RECIPIENT_OPTED_OUT` — no send, no retry.

Suppression is **reason-agnostic at send time**: an entry created by any means — a STOP reply (`opt_out`), a manual DNC add (`dnc_request`), or a compliance import (`manual`) — blocks the send. Entries are **tenant-scoped** (a STOP to tenant A does not silence tenant B unless the entry is platform-wide), stored as a hash plus a masked display value (e.g. `+1619***2898`), never the raw number. They surface in the existing [Suppression List](#suppression-list) admin view alongside every other suppressed contact.

The messaging app (consumer SMS) mirrors this: a 21610 on its Twilio webhook sets the contact to `opted_out` and adds a suppression-list row.

<!-- react-flow:sms-opt-out-flow -->

##### Primary & Secondary Actions {#primary-secondary-delivery}

When a contract has multiple delivery actions, exactly one is the **primary action**. The primary action determines the lead sale outcome — its delivery result (success, reject, duplicate, etc.) is what the system records as the official result for that buyer.

**Secondary actions** only fire after the primary succeeds. If the primary action fails or is rejected, secondary actions are skipped entirely. This ensures side-effect deliveries (email notifications, CRM pushes) only happen when the buyer actually accepted the lead.

**How primary is assigned:**
- The first delivery action added to a contract is automatically set as primary.
- If you delete the primary action, the next action is auto-promoted to primary.
- The primary action is indicated by a **blue "Primary" badge** in the delivery actions list.
- To change which action is primary, click the **"Set Primary"** button on any secondary action.

**Email as primary:** An email delivery action can be primary. Since email has no response payload, the delivery is always treated as successful upon send. No response field extraction is needed.

##### Importing Delivery from a Posting Spec URL {#import-delivery-spec}

On the contract delivery tab, you can automatically configure a delivery method by pasting a posting spec URL from any lead platform (LeadProsper, LeadSpedia, Boberdoo, etc.).

1. Open the contract detail page and go to the **Delivery** tab
2. In the **Import from Posting Spec URL** section, paste the spec page URL
3. Click **Analyze Spec** — AI reads the page and extracts the posting configuration (10-15 seconds)
4. Review the extracted spec: posting URL, format, field count, auth fields, and any notes
5. Click **Add as Delivery Method** to create a pre-configured delivery action
6. The delivery action is fully editable — adjust field mappings, body template, or response rules as needed before saving

The AI automatically:
- Maps their field names to your standard lead fields (e.g., `first_name` → `firstName`)
- Embeds auth credentials (API keys, campaign IDs) as literal values in the body template
- Sets up response mapping rules to interpret success, rejection, and duplicate responses
- Extracts response token patterns (lead ID, price, message) from the spec

> **Billing:** Analyze Spec — like the template editor's **AI Token Mapping** helper — is billed **from credits based on the actual size of the call** (typically a few credits; a disclosure note appears next to each action). At a zero credit balance the action is blocked until you add credits. See [AI usage billing](#credits-billing).

##### Field Transformers {#delivery-field-transformers}

Field transformers modify individual field values before they are sent to the buyer. Use them to reformat dates, strip phone numbers, change text casing, encode values, map states to abbreviations, and more.

Transformers are **chainable** -- apply multiple in sequence. Each transformer receives the output of the previous one.

###### Using Transformers in Field Mapping Mode

Each field mapping row has an **fx** button. Click it to open the transformer picker:

1. Click **fx** on any field mapping row
2. Click **+ Add Transformer**
3. Browse or search the categorized transformer list
4. Select a transformer -- if it has parameters, configure them inline
5. Add more transformers to build a chain (drag to reorder)
6. The **fx** button shows a count badge (e.g., `fx (2)`) when transformers are configured

###### Using Transformers in Template Mode

In body templates (JSON/XML), use the `||` syntax inside token placeholders:

```
{{fieldName||transformer1||transformer2:param}}
```

Examples:
- `{{firstName||trim||title_case}}` -- trim whitespace, then title case
- `{{phone||phone:10}}` -- strip to last 10 digits
- `{{phone||phone:e164}}` -- format as +1XXXXXXXXXX
- `{{state||us_state}}` -- "Florida" becomes "FL"
- `{{custom.debt_amount||number:2}}` -- strip to number with 2 decimals
- `{{firstName||trim||lower_case||append:@example.com}}` -- build an email
- `{{score||if_else:300-600=poor:601-700=fair:701-900=good:*=unknown}}` -- map ranges to labels

Type `||` inside a `{{ }}` block to trigger the transformer autocomplete dropdown.

###### Transformer Reference {#transformer-reference}

**Text Case** -- change capitalization

| Transformer | Syntax | Example |
|---|---|---|
| Lowercase | `lower_case` | "Hello World" -> "hello world" |
| Uppercase | `upper_case` | "hello" -> "HELLO" |
| Title Case | `title_case` | "hello world" -> "Hello World" |
| camelCase | `camel_case` | "hello world" -> "helloWorld" |
| snake_case | `snake_case` | "Hello World" -> "hello_world" |
| kebab-case | `kebab_case` | "Hello World" -> "hello-world" |
| PascalCase | `pascal_case` | "hello world" -> "HelloWorld" |
| URL Slug | `slug` | "Hello World!" -> "hello-world" |
| Capitalize First | `upper_first` | "hello" -> "Hello" |
| Lowercase First | `lower_first` | "Hello" -> "hello" |

**Numeric** -- format numbers and phone numbers

| Transformer | Syntax | Example |
|---|---|---|
| Extract Number | `number` or `number:DECIMALS` | "$12,345.67" -> "12345.67" |
| Phone Format | `phone:FORMAT` or `phone:LENGTH` | See below |
| Add | `math_add:AMOUNT` | "10" + 5 -> "15" |
| Subtract | `math_sub:AMOUNT` | "10" - 3 -> "7" |
| Multiply | `math_multiply:AMOUNT` | "10" x 2 -> "20" |
| Divide | `math_divide:AMOUNT` | "10" / 4 -> "2.5" |
| Round Up | `math_ceil` | "24.3" -> "25" |
| Round Down | `math_floor` | "24.7" -> "24" |
| Modulo | `math_modulo:DIVISOR` | "65" % 12 -> "5" |
| Output as Number | `output_number` | JSON: `42` instead of `"42"` |

**Phone format options:** `phone:digits10` (10 digits), `phone:digits11` (11 digits), `phone:e164` (+1XXXXXXXXXX), `phone:formatted` ((XXX) XXX-XXXX), `phone:10` (last 10 digits), `phone:11` (last 11 digits)

**Date** -- reformat and calculate dates

| Transformer | Syntax | Example |
|---|---|---|
| Format Date | `date:OUTPUT_FORMAT` | "2015/07/21" with `yyyy-MM-dd` -> "2015-07-21" |
| Calculate Age | `age` or `age:DECIMALS` | "1965/07/22" -> "60" |
| Add Time | `date_add:AMOUNT:UNIT` | Add 1 month to a date |
| Subtract Time | `date_sub:AMOUNT:UNIT` | Subtract 7 days |
| Date Difference | `date_diff:UNIT` | Days/months/years since date |
| Convert Timezone | `timezone:TO_ZONE` | UTC to "America/New_York" |

Date units: `days`, `months`, `years`, `hours`, `minutes`

**String** -- manipulate text

| Transformer | Syntax | Example |
|---|---|---|
| Trim Whitespace | `trim` | "  hello  " -> "hello" |
| Append | `append:SUFFIX` | "John" + " Doe" -> "John Doe" |
| Prepend | `prepend:PREFIX` | "Doe" -> "Mr. Doe" |
| Substring | `chars:START:LENGTH` | "1234code" chars:0:4 -> "1234" |
| First N Chars | `first:LENGTH` | "hello world" first:5 -> "hello" |
| Last N Chars | `last:LENGTH` | "hello world" last:5 -> "world" |
| Find & Replace | `replace:SEARCH:REPLACEMENT` | "hello_world" -> "hello-world" |
| Remove Characters | `remove_chars:CHARS` | "hello" remove mn -> "hello" |
| Pad Left | `pad_left:LENGTH:CHAR` | "1" pad_left:3:0 -> "001" |
| Pad Right | `pad_right:LENGTH:CHAR` | "hi" pad_right:5:. -> "hi..." |
| Pad Both | `pad_both:LENGTH:CHAR` | Center-pad to length |
| Escape Quotes | `escape` | Escapes quotes and backslashes |
| Mask Value | `mask` | Any value -> "***" |

**Encoding** -- encode and decode values

| Transformer | Syntax | Example |
|---|---|---|
| Encode | `encode:ALGORITHM` | base64, md5, sha1, sha256, sha512, url |
| Decode | `decode:ALGORITHM` | base64, url |

**Conditional** -- map values based on conditions

| Transformer | Syntax | Example |
|---|---|---|
| Value Map | `if_else:MATCH=OUTPUT:*=DEFAULT` | `if_else:FL=Florida:CA=California:*=Other` |
| Range Map | `if_else_between:MIN-MAX=OUTPUT` | `if_else_between:0-600=poor:601-over=good` |
| Default Value | `fallback:VALUE` | Empty field -> "N/A" |

**Location** -- US state conversions

| Transformer | Syntax | Example |
|---|---|---|
| State Abbreviation | `us_state` | "Florida" -> "FL" |
| Full State Name | `us_state_long` | "FL" -> "Florida" |

**Boolean** -- convert boolean representations

| Transformer | Syntax | Example |
|---|---|---|
| To Boolean | `to_bool` | "0" -> "false", "1" -> "true" |
| From Boolean | `from_bool` | "true" -> "1", "false" -> "0" |
| Output as Boolean | `output_bool` | JSON: `true` instead of `"true"` |

**Advanced** -- combine and restructure

| Transformer | Syntax | Example |
|---|---|---|
| Merge Fields | `merge_fields:FIELD1,FIELD2:SEPARATOR` | Combine state + zip -> "CA_92010" |
| CSV to JSON Array | `csv_to_json_array` | "a,b,c" -> `["a","b","c"]` |

###### Error Handling

If a transformer receives invalid input (e.g., a date transformer on a non-date string), it **passes the value through unchanged**. No delivery failures are caused by transformer errors -- the lead still delivers with the original value.

###### Type Coercion (JSON only)

Three transformers affect how values are serialized in JSON payloads:
- `output_number` -- emits as a JSON number: `42` instead of `"42"`
- `output_bool` -- emits as a JSON boolean: `true` instead of `"true"`
- `csv_to_json_array` -- emits as a JSON array: `["a","b"]` instead of `"a,b"`

These only affect JSON format. Form-encoded and XML always output strings.

##### Response Field Mapping {#response-field-mapping}

After a buyer accepts a lead via HTTP POST, their JSON response often contains values the system needs — a buyer-side lead ID, an adjusted price, a redirect URL, etc. Response field mapping lets you extract these values automatically.

**Adding a mapping:**
1. On a delivery action's configuration, find the **Response Field Mapping** section.
2. Click **Add Mapping**.
3. Enter the **Response Path** — a dot-notation path into the buyer's JSON response (e.g., `data.leadId`, `result.price`, `redirect_url`).
4. Select the **System Field** — which system field to populate with the extracted value.

**Available system fields:**

| System Field | Description |
|-------------|-------------|
| `buyerLeadId` | Buyer's internal ID for the lead — stored on the lead sale record |
| `bidPrice` | Buyer's bid price from the response |
| `responseMessage` | A message or note from the buyer's response |
| `redirectUrl` | URL to redirect the lead to (e.g., for click-to-call flows) |
| `adjustedPrice` | Final price after buyer-side adjustments |
| `pingToken` | Token for ping/post flows — used in the subsequent post request |
| `brand` | Brand or product name the buyer matched the lead to |

**Example:** If the buyer's response is:
```json
{
  "status": "accepted",
  "data": {
    "id": "BUY-98765",
    "offer_price": 42.50,
    "redirect": "https://buyer.com/lead/98765"
  }
}
```
You would map:
- `data.id` → `buyerLeadId`
- `data.offer_price` → `adjustedPrice`
- `data.redirect` → `redirectUrl`

**Legacy response tokens:** If a contract has older response token configuration (from before response field mapping was introduced), a migration notice is displayed with the existing token paths. These continue to work but new mappings should use the response field mapping UI.

> **Note:** Response field mapping only applies to HTTP POST delivery actions. Email and FTP actions have no response payload to extract from.

#### Routing Tab {#targeting-tab}

Who can win the lead, and when — the old Targeting and Schedule tabs merged into one surface:

**Geographic Filters:**
- **States**: Include or exclude specific US states
- **Zip Codes**: Target specific zip codes (collapsed when empty)

**Custom Filters:**
Build rules using:
- Field (any lead field)
- Operator (equals, not_equals, contains, in, not_in, greater_than, less_than)
- Value

Example: `state equals "CA"` or `age greater_than "25"`

Date fields use a date-specific filter control: choose `Older than` or `Within last`, then choose `1 month`, `3 months`, `6 months`, `1 year`, or `2 years`. `Older than 1 year` accepts dates before the same calendar day one year ago; `Within last 1 year` accepts dates after that cutoff. Use `Age (posted or from DOB)` with `Over age` or `Under age` for age rules backed by a posted age or computed from date of birth.

**Age filters and date of birth:** filters on the field `age` (lowercase) evaluate the lead's posted `age` value when one exists — a posted value always wins. If the lead posted no `age` but has a valid date of birth, the engine computes the age in whole years (same rules as the `{Lead_Age}` webhook token: invalid, future, or 120+ year DOBs resolve to nothing and the filter fails). Filtering on `Lead_Age` always uses the computed-from-DOB value. The routing trace shows the actual value the filter evaluated, including computed ages.

**Dedup Override:**
Inherit the buyer's dedup setting, force it on, or bypass it for this contract (collapsed when inheriting).

**Schedule** (one card — timezone + hours + expiration):

- **Timezone**: Contract's operating timezone
- **Schedule Hours**: Define active hours per day
  - Select day of week
  - Set start and end time
  - Enable/disable
- **Quick Template**: Add Mon-Fri 8am-5pm schedule
- **Expiration**: Date/time when contract automatically expires (collapsed sub-row when unset)

#### Calls Tab {#contract-calls-tab}

The call channel — visible when Call Tracking is on. Phone numbers (transfer number, AI Dialer), call billing settings, call caps, and collapsed Whisper/Recording sections. Full reference: [Call Settings on Contracts](#call-settings-on-contracts). SKU-funded contracts show the same fill-headroom strip as the Leads tab, call-cap flavored. The Call Tracking on/off toggle lives on Details → Capabilities.

#### Finance Tab {#contract-finance-tab}

Funding in, fees out:

- **Contract Funding** — for SKU-funded contracts, the funding panel (pack remaining, fillable now, funding history) renders here first. Standalone contracts see a "Not SKU-funded — misc costs only" line instead.
- **Misc Costs** — per-lead/per-call fees charged alongside the sale price (previously its own Misc Costs tab).

**Fires On** decides when a misc cost books:

| Fires On | Charges when |
|---|---|
| Per CPA Conversion | the lead converts |
| Per Sold Lead | the lead is delivered to this contract |
| **Per Disposition** | one specific disposition arrives |

**Per Disposition** covers costs incurred partway through the lead's life, before any conversion —
an appointment booked, an estimate scheduled, a hand-off to a third party. Without it a cost can only
book at sale or at conversion, so anything in between has nowhere to land.

Pick **Per Disposition** under *Fires On*, then choose the disposition in **Fires On Disposition** —
the cost books when exactly that disposition arrives, once per lead. The list shows only dispositions
belonging to this contract's vertical group; one from another group could never resolve, so the cost
would silently never fire.

Two things worth knowing:

- **The disposition is mapped globally; the money is not.** Mapping a status at the intake center
  makes it arrive for every contract using that centre, but only contracts carrying a Per Disposition
  misc cost charge anything. Adding the mapping alone is safe.
- **A lead is charged once.** If the same disposition arrives twice, or the lead has more than one
  sale on the contract, only the first charge is kept. A later conversion books revenue and payout
  without charging the cost again.

If the trigger shows *"trigger not resolved — this cost cannot fire"*, the disposition it points at
no longer belongs to this contract's vertical group, so the cost is silently inactive. Raise it with
the platform team rather than editing around it.

#### Activity Tab {#contract-activity-tab}

The contract's audit trail — every change with who, what, and when.

### Contract Evaluation Flow

When a lead enters routing, each contract is evaluated through the following filter chain. A contract must pass every check to be eligible.

<!-- react-flow:contract-evaluation -->

---

## Partners

**Location:** [/admin/partners](/admin/partners)

Partners are organizations who source leads for the system.

### Partner List

| Column | Description |
|--------|-------------|
| Name | Partner/company name |
| Email | Contact email |
| Company | Company name |
| Status | Active, Paused, Inactive |
| Created | Account creation date |

### Creating a Partner {#creating-a-partner}

**Location:** [/admin/partners/new](/admin/partners/new)

**Required Fields:**
- Name
- Email

**Optional Fields:**
- First Name, Last Name
- Phone, Company
- Address fields
- Status

### Partner Detail {#partner-detail}

**Location:** [Partners](/admin/partners) > Partner Detail

**Health strip.** Partner vitals are pinned above the tabs on every tab: active campaign count, leads today, leads over the last 7 days (both in your tenant timezone), and **Payout owed** (the partner's available balance — total balance minus anything locked in open payouts).

**One save bar.** All field edits across the page accumulate into a single floating save bar that appears only when something changed. Save sends just the changed fields; Cancel reverts to the last saved values. Setting a partner to Inactive first asks whether their campaigns should be deactivated too. Invalid fields (flagged as you leave them) disable the save until fixed.

#### Home Tab
- Status management — pending partners show Approve / Reject actions; approved ones a status selector
- Contact information, Address, Management (partner manager, timezone), Payment Configuration (cycle, threshold, balance readouts)
- Legal & Tax and Legacy & CRM are collapsed sections with a live summary of what's set

#### Campaigns Tab
View and manage partner's campaigns. Click "Add Campaign" to create one.

#### Leads Tab
All leads sourced by this partner.

#### Transactions Tab
Partner payout history (shown when finance features are enabled for the tenant).

#### API Keys Tab
The posting key (lead submission), portal API key creation (with a read-only toggle), and the key list — create/revoke apply immediately, independent of the save bar.

#### Users / Notifications / Webhooks / Activity
Portal members and invitations, per-user notification preferences (edited on behalf of a partner user, saved with one Save All action), partner-scoped webhooks, and the audit trail.

---

### Partner Payouts {#partner-payouts}

Partner earnings accrue into a perpetual `Total Balance`. The balance is only debited when an admin marks a payout as `paid`. The payout cron creates pending payout records on each partner's payment cycle; an admin then reviews and approves them.

**Three balance concepts on the partner detail Payment Configuration section:**

- **Total Balance** — All unpaid earnings ever accrued by this partner. Goes up when leads are sold, only goes down when a payout is marked `paid`.
- **Locked in Open Payouts** — The sum of any `pending`, `processing`, or `failed` payout rows for this partner. This amount is "reserved" — it's still inside Total Balance but has already been snapshotted into a payout row awaiting approval.
- **Available Balance** = Total Balance − Locked in Open Payouts. This is the number the cron uses to decide whether to create a new payout on the next cycle.

**Why the split matters:** Before this model, the cron would snapshot `Total Balance` directly. If an admin hadn't approved the previous payout by the next cycle, the cron would snapshot the same dollars again — approving both would double-pay the partner. Now:

1. **At most one open payout per partner** is enforced by a DB-level partial unique index (`payout_one_open_per_partner_idx`). A second insert attempt fails with a unique-constraint violation.
2. **The cron uses Available Balance**, not Total Balance, for the threshold check — so if $500 is already locked in a pending payout, the cron won't re-snapshot those dollars.
3. **`nextPayoutDate` always advances** on every cron run — even when the partner was skipped because a previous payout is still open. Keeps the calendar rhythm intact regardless of approval lag.
4. **Cancelled payouts release the reservation naturally** — balance was never debited, so cancelling simply frees Available Balance back up.
5. **Failed payouts are treated as open** — they still block new payout creation until an admin either retries them back to `pending` or cancels them.

**Payout status taxonomy:**

| Status | Class | Effect on Available Balance | Effect on Total Balance |
|---|---|---|---|
| `pending` | Open | Locked | Unchanged |
| `processing` | Open | Locked | Unchanged |
| `failed` | Open | Locked (until admin retries or cancels) | Unchanged |
| `paid` | Closed | Released | Debited by payout amount |
| `cancelled` | Closed | Released | Unchanged |

**Stuck payout alert:** A daily cron (`/api/cron/stuck-payout-check`, 14:00 UTC) scans for any open payout older than 7 days and logs a Sentry warning + audit entry (`payout_stuck_detected`). The payouts list page also renders a red "Stuck >7d" badge on any such row so admins can spot them at a glance.

---

## Campaigns

**Location:** [/admin/partners/campaigns](/admin/partners/campaigns)

Campaigns are partner programs that source leads.

### Campaign List

| Column | Description |
|--------|-------------|
| Name | Campaign identifier |
| Partner | Associated partner |
| Pricing Model | Fixed or RevShare |
| Cost/Lead | Amount paid to partner |
| Daily Cap | Maximum leads per day |
| Status | Active, Paused |

### Creating a Campaign {#creating-a-campaign}

**Location:** [/admin/partners/campaigns/new](/admin/partners/campaigns/new)

Creating a campaign only links a **partner** to an **offer** — nothing else. Pricing, caps, call tracking, and conversions are all configured on the [campaign page](#campaign-detail) after it's created.

**Fields:**
- **Vertical** (required) — pick the vertical first; the Offer list filters to offers in that vertical.
- **Partner** (required) — the lead source.
- **Name** (required) — leave blank to auto-generate as `Partner - Vertical`.
- **Offer** (required) — only offers in the selected vertical are shown.
- **Notes** (optional) — internal notes.

You can also start a campaign already scoped to an offer from the offer's **Campaigns** tab → **Add Campaign** (which prefills and locks the vertical), or scoped to a partner via `?partnerId=`.

**Starts in Testing:** a new campaign is created in **Testing** status with no payout type set. Before it can be activated, it needs at least one payout type — **CPL**, **CPA**, or **Calls** — configured on the campaign page. Attempting to activate a campaign that has no payout type (from the campaign page, a status change, or a bulk status action) is rejected with a validation error: *"Campaign needs at least one payout type (CPL, CPA, or Calls) before it can be activated."* Existing active campaigns are unaffected — the gate only applies when transitioning **into** Active.

### Campaign Detail {#campaign-detail}

**Location:** [Campaigns](/admin/partners/campaigns) > Campaign Detail

View and edit campaign settings, see linked offers, and track lead volume and costs. Settings are grouped into tabs so each channel and concern lives on its own surface:

- **Details** — identity: basic info (ID, name, status, partner/offer/vertical links, notes) and a collapsed Legacy & CRM section.
- **Leads** — what the partner earns and how many leads they may send: CPL payout (Pricing). With CPL enabled, the tab also shows the [Lead Routing Readiness check](#lead-routing-readiness), lead volume Caps (daily/weekly/monthly — leads beyond a cap are rejected at intake), and a collapsed Partner Response section.
- **Calls** — inbound call tracking: Call Tracking (enable, assigned number, DNI settings), the Call Readiness check, and Call Billing rules. Deep link `?tab=call` also lands here.
- **Conversions** — see [Campaign Conversions](#campaign-conversions).
- **Fields** — campaign-specific and inherited field configuration (see [Three-Tier Field Architecture](#campaign-fields)).
- **Intake Filters** — per-partner quality gate (see [Campaign Intake Filters](#campaign-intake-filters)).
- **Posting** — integration surfaces: the merged **Integrate with AI** card (copy an AI lander-builder prompt, or generate a single-use AI setup link), public spec link, post URL, authentication, field reference, and sample requests.
- **Webhooks** — event subscriptions for this campaign.
- **Activity** — audit log.

The Save bar is shared across tabs: it appears whenever there are unsaved edits, no matter which tab you're on, and saves the same campaign fields regardless of where you changed them.

#### Lead Routing Readiness Check {#lead-routing-readiness}

Campaigns with **CPL enabled** show a **Lead Routing Readiness** card at the top of the **Leads** tab of the campaign detail page (like the Call Readiness check, which appears on the Calls tab only when calls are enabled). It answers one question: *would a lead posted to this campaign right now route and sell end-to-end?* The card runs the same gates the routing engine uses, so a **Ready** verdict means a lead would actually place a sale. It reports one of three states — **Ready** (green), **N warnings** (yellow, leads route but something needs attention), or **Blocked** (red, a lead cannot sell) — and expands to a checklist explaining each gate:

- **Campaign active** — the campaign is active (warns in Testing mode — leads route but are flagged as test).
- **Partner posting key** — the campaign's partner has an active posting-scoped API key, so leads can authenticate.
- **Platform credits** — the tenant has enough platform credits to accept the post (exempt tenants always pass).
- **Campaign caps have headroom** — the campaign's own daily/weekly/monthly caps aren't exhausted (warning only — caps reset at rollover).
- **Active offer linked** — an active offer is linked (notes when the offer requires a ping before post).
- **Eligible contracts** — at least one linked contract is active, unexpired, and belongs to an active buyer (failures list the exact breakdown, including multi-sale mismatches on multisell offers).
- **Within schedule now** — at least one eligible contract is inside its configured hours right now (warning only — hours are temporary).
- **Funding available** — at least one eligible contract can pay for the lead: prepaid SKU inventory, CPA billing, or sufficient wallet balance/credit.
- **Pricing resolves revenue** — warns when an eligible contract would sell for $0 with no CPA or SKU pricing to explain it.
- **Delivery configured** — eligible contracts have a primary delivery action (a sale fails without one; warns if only some are missing it).
- **Contract caps have headroom** — not every eligible contract has hit its current-period lead/spend caps (warning only).

Use **Refresh** to re-run the check after making changes.

**What it can't check:** gates that depend on the lead itself — TCPA/consent, suppression lists, geo/demographic/custom filters, deduplication, and quality-score floors — can only be evaluated against a real lead, so a campaign can be **Ready** and still reject a specific lead on one of those. To see why an individual lead didn't route, use the per-lead routing trace (lead detail → **Distributions** waterfall).

#### Campaign Conversions {#campaign-conversions}

**Location:** Campaign edit page → **Conversions** tab.

Conversions covers everything about what happens *after* a lead or call is delivered — outcomes reported back and any partner payout tied to them.

- **Dispositions** (always on) — A disposition is the outcome reported after delivery (e.g. sale, no-answer, not-qualified). Partners and buyers post it to a single **unified postback URL** (`/api/v1/postback`); the ID parameter picks the type — `saleId` for a lead sale, `callSaleId` for a call sale (a generic `conversionId` alias is coming). Valid disposition codes come from the campaign's vertical group. No CPA is required to record dispositions.
- **Send dispositions to partner** — display-only status showing whether the partner is subscribed to the `lead.dispositioned` / `call.dispositioned` webhook events. This is a plain subscription (CPA not required); manage it on the **Webhooks** tab.
- **CPA (Cost Per Acquisition)** — campaign-level partner payout when a delivered lead or call converts. Toggle it on to set a pricing model (Fixed or RevShare, with an optional Max Payout cap on RevShare). These knobs are what the *partner* earns; buyer-side CPA billing (the amount charged and the conversion window) is separate and set per contract.

### Three-Tier Field Architecture {#campaign-fields}

The system validates lead data across three tiers of field definitions:

**Global Fields** — System-wide fields like firstName, lastName, email, phone. Always validated. Stored in dedicated lead columns.

**Vertical Fields** — Defined per vertical (e.g., "Solar" has utilityBill, homeOwner). Shared across all campaigns in that vertical. Stored in lead `custom` JSONB.

**Campaign Fields** — Defined per campaign for path-specific data (e.g., roofAge for "Solar Path A"). Only validated for leads submitted to that campaign. Stored in lead `custom` JSONB.

#### Managing Campaign Fields

1. Go to **Partners → Campaigns → [Campaign] → Fields tab**
2. Click **Load Fields** to see inherited vertical fields (read-only) and campaign fields
3. Add fields with a camelCase name, display label, type, and required flag
4. Field names cannot conflict with system fields or vertical fields
5. Supported API field types: text, number, boolean, date, list, email, phone, url, textarea. Use `list` with an `options` array for dropdown-style fields.

#### How Validation Works

When a lead is submitted:
1. Global fields are validated by the Zod schema (firstName, email, etc.)
2. Vertical fields for the campaign's vertical are checked — required fields must be present, types must match
3. Campaign fields are checked — same rules apply
4. Any missing required field or type mismatch returns a 400 error naming the specific field

#### Posting Spec

The posting spec (`GET /api/v1/campaigns/{id}/posting`) now returns:
- `requiredFields` — global system fields
- `verticalFields` — fields inherited from the vertical
- `campaignFields` — fields specific to this campaign
- `customFields` — merged list (backward compatible)
- `sampleBody` — includes sample values for all three tiers

##### TCPA / Legal Disclosure on Posting Spec Pages {#posting-spec-tcpa}

Set tenant-wide disclosure text at **Admin → Settings → General → Compliance & Legal**. When set, it renders in its own section on the public posting spec page (`/specs/[id]`) for every campaign under your tenant. Plain text only — line breaks preserved, no HTML/markdown. Leave blank to hide the section. Typical use: TCPA consent language, data-use disclosures, or legal copy you want partners to review before integrating.

### Campaign Intake Filters {#campaign-intake-filters}

**Location:** Campaign edit page → **Intake Filters** tab.

Per-partner quality gate at the campaign level. Contracts already filter for buyer fit; intake filters decide whether the **partner's** lead is acceptable in the first place. One campaign = one partner's quality bar, so two campaigns under the same offer can apply different rules without duplicating contracts.

The filter editor is the same `field` / `operator` / `value` editor used on contracts, and the field selector covers system, vertical, campaign-custom, and enrichment-derived fields — intake runs **after** validation, suppression, dedup, and enrichment, so anything available to contracts is available to intake.

<!-- react-flow:campaign-intake-filters -->

#### Failure Modes

When a lead fails an intake filter, the campaign's **Fail Mode** decides what happens next:

- **Reject** — Lead is DQed at intake. No contracts are evaluated, no `leadSale` rows created, the partner is charged $0. A synthetic `leadDistribution` row records the rejection for ops audit; the partner sees a generic rejection.
- **Accept without crediting partner** — Lead routes to buyers normally and buyers are charged as usual, but the partner's payout is forced to $0 (`intakePayoutSuppressed` on the lead). Buyer caps are consumed. The partner sees the same generic rejection as Reject mode.

When to use which: pick **Reject** when the lead is unsellable or you don't want enrichment cost wasted on it. Pick **Accept without crediting** when buyers will still pay for the lead but the partner's traffic doesn't meet your quality bar — you keep the buyer revenue without rewarding the partner.

#### Partner Visibility

Both modes render identically to the partner: a `result: failed` response with the generic message **"Lead does not meet quality criteria"**. The specific filter rule (e.g. `income < 50000`) is stored on the lead's internal reason column for ops debugging but never returned in any partner-facing response, webhook, or portal endpoint.

#### Known Limitation (v1)

If an admin **tightens** intake filters while a `/ping` is active (5–60 min ping window) and the matching `/post` arrives before the ping expires, the post will be re-evaluated against the new filters and partner cost may be forced to $0 even though the original ping promised a real amount. Inverse direction (loosening filters) honors the original ping correctly. Workaround: pause the campaign briefly, let active pings expire, then edit. A v1.1 fix will stamp the intake decision onto the ping row at ping time.

### Progressive Lead Capture (Sessions) {#progressive-lead-capture}

For multi-step funnels that collect data across multiple form pages, use the Session API instead of submitting all data at once.

**Why use sessions?**
- Prevents data loss when users drop off mid-funnel
- Each step is fire-and-forget (no validation until final submit)
- Sub-10ms PATCH responses for great UX
- Session key acts as bearer token — no extra auth needed per step

<!-- react-flow:lead-session-flow -->

**How it works:**
1. Create a session with your partner API key
2. PATCH data after each funnel step (fire-and-forget)
3. Submit the session on the final step — validation + routing happens here

**Example flow:**
```javascript
// Step 1: Create session
const res = await fetch('/api/v1/sessions', {
  method: 'POST',
  headers: { 'Authorization': 'Bearer lr_your_key' }
})
const { data: { sessionKey } } = await res.json()

// Steps 2-N: Send data per step (fire-and-forget)
fetch(`/api/v1/sessions/${sessionKey}`, {
  method: 'PATCH',
  headers: { 'Authorization': `Bearer ${sessionKey}`, 'Content-Type': 'application/json' },
  body: JSON.stringify({ data: { firstName: 'John', email: 'j@test.com' }, currentStep: 1 }),
  keepalive: true
})

// Final step: Submit
const result = await fetch(`/api/v1/sessions/${sessionKey}/submit`, {
  method: 'POST',
  headers: { 'Authorization': `Bearer ${sessionKey}` }
})
```

Sessions expire after 24 hours if not submitted.

#### Co-Registration (Coreg) {#coreg}

After a lead is submitted through a session, you can route the same contact data through additional campaigns — this is called **co-registration**. Common use case: a funnel asks "Interested in Solar?" and if yes, submits the same PII to a solar campaign.

**How it works:**
1. Submit the primary lead normally via `POST /sessions/{sk}/submit`
2. Session moves to `submitted` status (not `completed`)
3. For each coreg offer the user accepts, call `POST /sessions/{sk}/coreg` with `campaignId` in the request body
4. Each coreg creates a separate lead linked to the primary via `parentLeadId`
5. When done, close the session: `PATCH /sessions/{sk}` with `{ "status": "completed" }`

**JavaScript example:**
```javascript
// 1. Primary submit
const result = await fetch(`/api/v1/sessions/${sessionKey}/submit`, {
  method: 'POST',
  headers: { 'Authorization': `Bearer ${sessionKey}` }
})

// 2. Coreg offers
if (userSaidYesToSolar) {
  await fetch(`/api/v1/sessions/${sessionKey}/coreg`, {
    method: 'POST',
    headers: { 'Authorization': `Bearer ${sessionKey}`, 'Content-Type': 'application/json' },
    body: JSON.stringify({ campaignId: 'uuid-of-solar-campaign', data: { interestedInSolar: true } })
  })
}

// 3. Close session
await fetch(`/api/v1/sessions/${sessionKey}`, {
  method: 'PATCH',
  headers: { 'Authorization': `Bearer ${sessionKey}`, 'Content-Type': 'application/json' },
  body: JSON.stringify({ status: 'completed' })
})
```

Sessions with submitted leads auto-complete after 24 hours if not explicitly closed.

---

## Offers

**Location:** [/admin/offers](/admin/offers)

Offers connect campaigns (sources) to contracts (buyers) and define distribution rules.

### Offer List

| Column | Description |
|--------|-------------|
| Name | Offer identifier |
| Vertical | Lead category |
| Default Payout | Standard payout model/amount |
| Status | Active, Paused, Inactive |
| Campaigns | Number of linked campaigns |
| Contracts | Number of linked contracts |

### Creating an Offer {#creating-an-offer}

**Location:** [/admin/offers/new](/admin/offers/new)

The create form is deliberately slim — pick a vertical, name the offer, say what it sells, done:

- **Vertical**: Lead category (if the vertical has a vertical group, it appears below the selection). All other fields unlock after a vertical is chosen.
- **Name**: Descriptive identifier
- **What does this offer sell?**: **Leads**, **Calls**, or **Both**. This sets the offer's Lead Sales / Call Tracking capabilities and which tabs its detail page shows.
- **Notes**: Optional internal notes

Everything else — distribution, CPL/CPA pricing, channels, CPA Revenue, Ping Tree — is configured on the offer's detail page after creation (the **Capabilities** card on the Details tab is the control surface).

### Offer Detail {#offer-detail}

**Location:** [Offers](/admin/offers) > Offer Detail

The offer page has **10 workflow-ordered tabs**: `Overview | Campaigns | Contracts | Lead Routing | Acceptance & Dedup | Pricing | Call Routing | Fields | Integrations | Activity`. Two are capability-gated: **Lead Routing** requires Lead Sales (CPL) and **Call Routing** requires Call Tracking. **Contracts** is always visible — it's the one place to attach buyers regardless of channel. **Pricing** is always present and gates its Lead / CPA / Call-billing *sections* internally by capability. Old bookmarked `?tab=` links (basic, details, campaigns, acceptance, dedup, contracts, dq-contracts, scarcity-weights, leads, conversions, attribution, webhooks, onboarding) resolve to their new homes.

The mental model: **Contracts = who's attached (channel-neutral) · Lead Routing / Call Routing = how each channel picks winners · Pricing = all the money.**

A **hub health strip** sits above the tab bar on every tab: active campaigns, linked contracts with how many are **routable** right now (same eligibility checks the routing engine uses), leads today, fill % (sold ÷ accepted today), and — when Call Tracking is on — **Calls today** with the call-routable contract count (routable *and* call-ready).

All field edits across every tab save through **one Save bar** at the bottom of the page — it appears when anything is dirty and saves only what changed. Linking/unlinking campaigns and contracts applies immediately.

#### Overview Tab
The offer's identity and its single capability control surface:

- **Capabilities** — the single control surface for the offer's three tab-gating capability flags: **Lead Sales** (CPL), **Call Tracking**, and **CPA Revenue**. (Ping Tree is a lead-selling *mode*, not a capability — its toggle lives on the Pricing tab.) If a capability contradicts the linked entities (e.g. Call Tracking on with no call-enabled contracts), an amber hint appears under the toggle with a fix-it link.
- **Offer Details** — ID, name, vertical, **Marketing Tags** (formerly "Channels" — display-only labels like Email/SMS/Voice; they do **not** affect routing), status, visibility, description, notes.
- Collapsed advanced sections: **Balance & Billing**, **Legacy & CRM**.

#### Campaigns Tab
The offer's supply side. Link/unlink the campaigns that feed this offer (link/unlink applies immediately), plus the collapsed **Partner Response Defaults** that seed new campaigns created from this offer.

**Distribution settings** (configured on the Lead Routing tab):
- **Distribution Type**:
  - *Exclusive*: Sell to one buyer only
  - *Multisell*: Sell to all eligible buyers
  - *Hybrid*: Sell to N buyers (configurable)
- **Distribution Rule**:
  - *Highest Price*: Highest bidder wins
  - *Priority*: Highest priority contract wins
  - *Weighted Random*: Random based on weights
  - *Round Robin*: Rotate through contracts
  - *Scarcity Weighted (DWRR)*: Weights from geographic scarcity (configure weights on the Lead Routing tab)
- **Round Robin Order** (Round Robin only): Controls the rotation order over the eligible contract set on each lead. Eligibility (filters, caps, balance, dedup) is identical across all three modes — only the order of the surviving set changes.
  - *Random*: Fisher-Yates shuffle of the eligible contracts on every lead. No memory between leads — each lead gets an independent draw. Distribution is statistically uneven over small samples and drifts further when contracts have different caps (uncapped contracts stay eligible longer and win more).
  - *Oldest contract first* (recommended for fairness): Least-recent-win. The routing engine tracks the last time each contract won on this offer (`offerContract.lastWinAt`) and gives the next lead to whichever eligible contract hasn't won in the longest — never-won contracts go first. Tiebreak among same-`lastWinAt` (or all-never-won) contracts: oldest `offerContract.createdAt` first.
  - *Newest contract first*: Same least-recent-win fairness as oldest_first — the contract that hasn't won in the longest still wins next. Only the tiebreak direction flips: among contracts tied on `lastWinAt`, the most recently attached `offerContract` wins. Useful when ramping up a freshly added contract.

  Round Robin Order is **one field shared by both channels** — the same rotation order also drives call round-robin, so the control appears on both the Lead Routing and Call Routing tabs.
- **Max Sales**: For hybrid, number of buyers to sell to

#### Offer Description {#offer-description}
The **Description** field on the Overview tab is a partner-facing text shown in the partner portal. It helps partners understand what the offer is about.

#### Contracts Tab {#offer-contracts}
Always visible — the one place to attach buyers, regardless of channel:

- **Link/unlink** primary contracts (applies immediately), with inline-editable **weight** and **priority** per row (saved with the page Save bar). Weight/priority editing lives here — the routing tabs show read-only previews.
- **Channel badge** per row — **Leads**, **Calls**, or **Both**, derived server-side: a contract is *call-ready* when call routing is on and a transfer phone number is set; it's *lead-capable* unless it's call-only (call enabled with no lead billing).
- Rows that have calls enabled but no phone show a **"not call-ready — set phone number"** hint linking straight to that contract's Calls tab.
- The same **routability chip** as the routing previews (routable / caps full / schedule closed / paused).

#### Lead Routing Tab {#offer-routing}
Visible when **Lead Sales (CPL)** is on. Everything that decides **how a lead is split and who gets it**:

- **Distribution** — the type (Exclusive / Multisell / Hybrid) and rule (Highest Price / Priority / Weighted Random / Round Robin / Scarcity Weighted), Round Robin Order (when applicable — shared with Call Routing), Shadow DWRR toggle, and Max Sales (hybrid). Co-located with the win-order preview it drives.
- **Only route leads to call-capable buyers** (`requireCallContracts`) — a **lead** routing filter (moved here from the old Calls tab): when on, leads only route to contracts with call routing enabled and a transfer number set. Toggling Call Tracking off resets it — the confirm dialog calls this out.
- **Effective Lead Order** — read-only preview of the linked contracts as a pre-computed win order, ranked by the active distribution rule (link/unlink and weight/priority editing moved to the Contracts tab). Each row shows rank, price, and a **routability chip**: green `routable` (passes status/schedule/cap checks), amber `caps full` with the reset time, gray `schedule closed` with the next-open time, red `paused — skipped`. The first routable row is highlighted as "wins next lead". The preview is channel-aware: call-only contracts (no lead billing) are separated with a "call-only contract" note, and when the call-capable filter is on, non-call-ready rows show as "filtered — not call-ready". Lead-specific filters and dedup still apply at route time.
- **DQ Fallback** — the rejected-lead fallback path (was the DQ Contracts tab). Expanded when configured; collapsed with an "Add…" row when not. DQ distribution rule, Bill DQ deliveries, CPL/CPA payout, and the linked DQ contracts table. DQ is lead-only and stays whole on this tab.
- **Scarcity Weights** — renders the DWRR weight panel when the rule is Scarcity Weighted or Shadow DWRR is on; otherwise a permanent explainer row (the section no longer appears/disappears as a tab).

#### Acceptance & Dedup Tab
The read-heavy "who qualifies / what's a dupe" split out of Routing:

- **Acceptance Criteria** — read-only union of what the linked active contracts collectively accept (geo, schedule, filters).
- **Deduplication** — vertical dedup settings (read-only) plus the offer's Cross-Partner Dedup toggles, with a collapsed **Effective Dedup — Per Contract** reference table.

#### Pricing Tab
The one home for money. Sections gate internally by capability:

- **Lead Pricing (CPL)** + **Ping Configuration** (when **Lead Sales** is on) — default CPL payout (model, amount, max payout for revshare) that campaigns inherit, plus the Ping Tree toggle and Require Ping Before Post (per-field ping requirements live on the Fields tab).
- **CPA Pricing** (when **CPA Revenue** is on) — CPA payout model, amount, max payout. When CPA Revenue is off on a call-enabled offer, an explainer links back to Capabilities on the Overview tab — never a second toggle.
- **Call Billing** (when **Call Tracking** is on) — the offer-level call billing panel.

#### Call Routing Tab {#offer-call-routing}
Visible when **Call Tracking** is on. Call *behavior* only — the enable toggle lives in Capabilities on the Overview tab; call **billing** rules live on the Pricing tab:

- **Affinity** (lead/caller) and the call distribution rule.
- **Connection Mode** (formerly "Distribution Type" here) — *Sequential* tries contracts one at a time; *Non-Sequential* rings all eligible contracts together. Renamed to stop colliding with the lead-side Distribution Type (winner count). The field is hidden entirely when the call distribution rule is already **Simultaneous**.
- **Round Robin Order** — the same shared field as on Lead Routing; one rotation order drives both channels.
- **Effective Call Order** — preview of the **call-ready** contracts (call routing on + transfer number set) in the order the call rule dials them. Priority and round-robin show a concrete order; weighted and price rules are decided at route time, so those rows show unranked with their basis. With zero call-ready contracts, the preview shows a warning linking to the Contracts tab.
  - Each row carries a **routability** chip, the same engine-eligibility check the Lead Routing preview uses. Call-ready is not the same as routable: a contract that is paused, capped or off-schedule still appears in the order but is skipped at call time. The first genuinely routable row is marked **answers next call** — so this preview and the header's *call-routable* count agree.
- **Recording** (collapsed when at-default).

The "Only route leads to call-capable buyers" filter is **not** here anymore — it's a lead filter and lives on the Lead Routing tab. Turning Call Tracking off prompts a confirm dialog listing every field it resets, including that filter (flagged "affects lead routing").

#### Integrations Tab
External-system config merged from the old Attribution, Webhooks, Integrations, and Onboarding Wizard tabs: attribution policy overrides, offer-scoped webhooks, per-offer integration opt-ins (off by default — see [Per-Offer Opt-In](#lead-enrichment-integrations)), the **Messaging** opt-in (below), and the buyer onboarding wizard.

##### Messaging — "Add this offer's leads & calls to Messaging" {#offer-messaging-optin}

**Off by default, on every offer.** While it is off, nothing about this offer's leads or calls is written to the messaging contact database, and no messaging flow can enrol them. Turn it on only for offers whose leads consented to be contacted — enabling it is, in practice, that assertion.

- The card only appears when **Messaging is enabled for your tenant** (System > Features). With messaging off there is nothing to opt into, and the API rejects an attempt to set the flag.
- It saves with the page's single Save bar, like every other offer field.
- Turning it **off** stops new contacts from being created for this offer. Contacts already in messaging stay — remove them from Messaging > Contacts if you need them gone.
- Historical replays ([backfill](#messaging-contacts)) obey the same switch: leads on an offer that never opted in are not backfilled.

#### Fields Tab {#offers-field-settings-propagation}

The **Field Settings** tab controls which fields this offer's campaigns show as visible/required on their posting specs. Each campaign linked to the offer can also declare its own `fieldOverrides` — a narrower set of per-field visible/required flags that only affects that campaign.

**Precedence:** campaign overrides win. If an offer marks `zipCode` required and a campaign has `fieldOverrides.zipCode = { hidden: true }`, the campaign's posting spec hides `zipCode` regardless of the offer setting. This is by design — overrides are how individual campaigns diverge from their offer's defaults.

**Propagation modal.** When you edit offer Field Settings and click **Save**, the system diffs the old vs new settings, then queries every linked campaign (status `testing`, `active`, or `paused`) to see which ones have `fieldOverrides` that would shadow the changed fields. If any do, a modal appears listing those campaigns with per-campaign checkboxes. Your options:

- **Apply to Selected Campaigns** — save the offer AND strip the changed field keys from each checked campaign's overrides. Unrelated override keys on the same campaign are preserved. One `auditLog` row is written per campaign touched, plus one for the offer save. All writes run in a single `db.transaction()` — if any campaign update fails, every change rolls back.
- **Save Offer Only** — save the offer without touching any campaign overrides. The campaigns continue to shadow the new offer settings.
- **Cancel** — abort the save, leave the form dirty.

If no campaigns shadow the changed fields (or no fields actually changed), the modal is skipped and the offer saves directly.

**Review banner.** On page load, the system also checks whether any campaign already diverges from the current offer settings — even if you haven't edited anything yet. If so, a dismissible banner appears above the field table: "N campaigns override fields set here. [Review & Propagate]". Clicking the button opens the modal directly against the live settings, so you can always get back to propagation without re-editing.

<!-- react-flow:field-override-propagation -->

**Known limitation — lost-update race.** If another admin edits a campaign's `fieldOverrides` between the moment the impact check runs and the moment you click Save, your save will overwrite their change for the specific keys you're propagating. No data corruption occurs — only the "lost update" of the other admin's override edit. Optimistic locking via `updatedAt` is not used here because no other endpoint in the codebase uses it; introducing it only for this flow would be inconsistent. Tracked as a v2 follow-up.

**Custom-field name collisions.** Offer `fieldSettings` keys silently shadow `campaignField` rows with the same name — if an offer marks `state` hidden and a campaign has a custom field named `state`, that custom field disappears from the posting spec. This is enforced in two places:

- **Warn on offer save.** The impact modal now surfaces collision rows alongside override rows. Shadowed overrides are tagged red (propagation can clear them); custom-field collisions are tagged amber with a `custom:` prefix. Apply to Selected only clears overrides — amber collisions require manually renaming the field in the campaign's Fields tab.
- **Hard-block on custom-field create/rename.** Creating or renaming a `campaignField` whose name matches a key in the parent offer's `fieldSettings` returns `400 VALIDATION_ERROR`. Fix by renaming the field or clearing the offer setting first.

### Distribution Flow

After contracts are evaluated, eligible contracts enter the distribution flow based on the offer's distribution type and rule.

<!-- react-flow:distribution-flow -->

### Offer Visibility {#offer-visibility}

Offers have a **visibility** setting: **Public** or **Private**.

- **Public** offers appear in the Partner Portal offers page, allowing partners to discover them.
- **Private** offers are only visible to partners who have a campaign linked to them.
- Offers in **setup** status are never listed in the marketplace, regardless of visibility. They must be activated first.

Set visibility on the Offer Detail > Details tab.

### Partner Offers Page {#partner-marketplace}

**Location (Partner Portal):** [/partner/offers](/partner/offers)

Partners browse offers they have access to. The page shows:
- Offer name, vertical, distribution type, and payout info
- Campaign status badge (Active, Paused, etc.) for linked campaigns
- Cost per lead for campaigns they're linked to
- Only offers where the partner has a linked campaign are shown

Partners must have the **permOffers** contact permission enabled to access this page. Without it, the offers link is hidden from their navigation.

Partners can toggle between **list view** and **grid view** using the view switcher at the top of the page. List view displays a table with offer details, linked campaigns, and cost per lead. Grid view shows the same information as cards. The default is list view.

Partners are linked to offers through campaigns — admins create campaigns and link them to offers from the Offer Detail page.

### Partner Offer Detail {#partner-offer-detail}

**Location (Partner Portal):** /partner/offers/:id

Clicking an offer opens its detail page with tabs for Overview, Posting Specs, Leads, and Payouts (tabs vary based on whether a campaign is linked).

The **Overview tab** includes a **Stats card** showing real-time analytics for the partner's leads on that offer:

| Metric | Description |
|--------|-------------|
| Total Leads | All leads submitted through linked campaigns |
| Sold | Leads successfully sold to buyers |
| Rejected | Leads rejected by routing filters |
| Returned | Leads returned after initial sale |
| Pending | Leads awaiting routing or match |
| Total Revenue | Sum of revenue from sold leads |
| Total Cost | Sum of costs across all leads |
| Conversion Rate | Sold / Total (percentage) |
| Return Rate | Returned / Sold (percentage) |
| Avg Cost/Lead | Total cost divided by total leads |
| Today / This Week / This Month | Lead counts for each time period |

Stats exclude test leads. If no campaign is linked, the page shows a "No campaign linked" message instead of stats.

### Cascade Status Changes {#offer-cascade-status}

When an offer's status changes from **active** to **paused** or **inactive**, the system prompts the admin to cascade the change to all linked campaigns.

- **Cascade enabled:** All linked campaigns are automatically disabled. Each campaign records which offer disabled it via `autoDisabledByOfferId`.
- **Cascade skipped:** Linked campaigns remain in their current state.

When the offer is **reactivated**, the system identifies campaigns that were auto-disabled by that offer and offers to restore them. Only campaigns disabled by the specific offer are affected — manually paused campaigns are left alone.

### DQ Contracts (Fallback Distribution) {#dq-contracts}

DQ (disqualified) contracts are fallback buyers that receive leads when no normal contracts match during routing. This lets you monetize rejected leads instead of discarding them.

**Location:** Offer Detail > Lead Routing tab > **DQ Fallback** section

#### How It Works

1. A lead is submitted and the routing engine evaluates all normal contracts on the offer.
2. If no normal contracts accept the lead (all filtered out by geo, caps, schedule, dedup, etc.), the lead is marked **rejected**.
3. The engine then evaluates DQ contracts as a secondary distribution pass.
4. Qualifying DQ contracts receive the lead. The lead status stays **rejected**, but the `isDqLead` flag is set to `true` and DQ sales are recorded.

DQ delivery does **not** enforce contract caps (daily/weekly/monthly lead caps or spend caps). DQ exists to absorb whatever didn't sell on primary, so a "full" contract still accepts DQ overflow. Caps apply to the primary path only.

#### Configuration

**Distribution Rule:**
- **All Qualifying** — deliver to every eligible DQ contract
- **Round Robin** — rotate through one DQ contract at a time

**Billing:**
- **Bill DQ deliveries** — enabled by default. Turn off to keep DQ delivery and audit records while setting DQ sale price/cost, buyer charge, partner payout, and billable misc costs to zero.

**Filter Options:**
- **Apply Geo** — when enabled, the contract's geo filters (state, zip) are enforced for DQ leads. When disabled, geo filters are skipped.
- **Apply Dedup** — when enabled, the buyer's dedup rules are checked before DQ delivery. When disabled, the lead is delivered even if the buyer already has it.

**Payout Configuration (CPL and CPA):**

DQ sales have separate payout settings, configured independently for CPL and CPA:
- **Inherit** — use the campaign's existing payout configuration for DQ sales
- **Revshare** — pay the partner a custom percentage of DQ revenue. Set the percentage in the field that appears when Revshare is selected.

#### DQ Sales in Reporting

DQ revenue and payouts are tracked separately from normal sales:
- **Lead list/detail** — DQ leads show the `isDqLead` badge. DQ Revenue and DQ Payout columns display the DQ-specific amounts.
- **Custom reports** — DQ Revenue and DQ Payout are available as separate columns/metrics.
- **Webhooks** — a `lead.dq_distributed` event fires when a DQ delivery completes, containing the lead ID, DQ contract, and sale details.

#### Adding DQ Contracts

1. Go to **Offers** > select an offer > **DQ Contracts** tab
2. Select a contract from the dropdown and click **Add**
3. Configure distribution rule, filter options, and payout model
4. Save changes

Any active contract can be used as a DQ contract. The same contract can serve as both a normal and DQ contract on different offers.

### Ping/Post (Ping Tree) {#ping-post}

Ping/Post lets partners check buyer availability and pricing before submitting a full lead. Instead of posting blindly, the partner sends a lightweight "ping" first, sees which buyers match and at what price, then decides whether to send the full lead as a "post."

#### Offer Type

The offer's **Ping Tree** capability controls submission mode:
- **Ping Tree off** (default, "direct post") — partners submit the full lead immediately via `POST /api/v1/leads/submit`
- **Ping Tree on** — partners ping first, then post with a `pingId`

Toggle it in the **Capabilities** card on the **Offer Detail > Details** tab; the Require Ping Before Post setting lives on the **Leads** tab.

#### Configuring Ping Fields

When an offer's Ping Tree capability is on, the field table on the offer's **Fields** tab shows a **Ping Required** column.

1. Go to **Offers** > select the offer > **Fields** tab
2. Check **Ping Required** for each field the partner must include in their ping
3. Typical ping fields: `state`, `zipCode`, `email`, `phone`
4. Save — campaigns under this offer now display ping documentation in their posting specs

#### How Ping/Post Works

<!-- react-flow:ping-post-flow -->

1. **Ping** — Partner sends `POST /api/v1/leads/ping` with the campaign posting key and ping-required fields
2. **Evaluation** — System checks all contracts on the offer: buyer status, balance, schedule, geo/demographic filters, caps, dedup
3. **Response** — Returns a `pingId`, expiry time, match count, and matched buyers with pricing
4. **Decision** — Partner reviews matches and pricing, decides whether to proceed
5. **Post** — Partner sends `POST /api/v1/leads/submit` with the full lead data plus the `pingId` from step 3
6. **Routing** — System creates the lead, runs routing (re-evaluates contracts), and links the lead to the original ping

#### Ping API Request

```
POST /api/v1/leads/ping
Authorization: Bearer pk_xxx
Content-Type: application/json

{
  "campaignId": "camp_abc",
  "state": "CA",
  "zipCode": "90210",
  "email": "jane@example.com",
  "phone": "3105551234"
}
```

#### Ping API Response

```json
{
  "data": {
    "pingId": "8f14e45f-...",
    "expiresAt": "2026-04-07T12:05:00.000Z",
    "matchCount": 2,
    "matches": [
      { "price": "12.50", "rank": 1 },
      { "price": "10.00", "rank": 2 }
    ]
  }
}
```

#### Post with Ping ID

Include the `pingId` in the standard lead submission:

```
POST /api/v1/leads/submit
Authorization: Bearer pk_xxx
Content-Type: application/json

{
  "campaignId": "camp_abc",
  "pingId": "ping_abc123",
  "firstName": "Jane",
  "lastName": "Doe",
  "email": "jane@example.com",
  "phone": "3105551234",
  "state": "CA",
  "zipCode": "90210"
}
```

#### Key Details

| Rule | Detail |
|------|--------|
| Expiry | Ping results expire after 5 minutes by default (configurable 1-60 min in offer settings) |
| Optional ping | Partners can still submit without pinging — `pingId` is optional on post |
| Validation | Missing ping-required fields return a 400 error |
| Single use | Each `pingId` can only be used once (status changes to "converted") |
| Toggling off | Switching an offer back to Direct Post immediately blocks new pings |
| Posting specs | Campaign posting specs auto-update to show ping docs when the offer type is Ping Tree |

#### Ping Logs (Admin) {#ping-logs}

Two admin pages make the auction observable end-to-end:

- **Inbound Ping Log** — [/admin/ping-log](/admin/ping-log) — every partner ping we received and the match decisions it produced.
- **Outbound Ping Log** — [/admin/outbound-ping-log](/admin/outbound-ping-log) — the HTTP pings we in turn fired out to buyer contracts during the auction.

##### matchCount vs totalMatches

| Field | Meaning |
|------|--------|
| `matchCount` | Winners only — the number of contracts we returned to the partner. This is what went back on the wire. |
| `totalMatches` | Every contract evaluated, winners plus any that were **excluded** mid-auction (below floor, no bid, network error, etc.). |

On the Inbound Ping Log row the Matches column reads `{winners} / {total}` when an excluded contract exists — e.g. `2 / 3` means 2 winners returned and 1 was excluded. When everything won it's just the count.

##### Excluded matches

An **excluded** `pingMatch` row is a contract the auction evaluated but rejected before fanout. Excluded rows have:

- `kind = 'excluded'` (winners have `kind = 'winner'`)
- `price = NULL` and `rank = 0`
- A `dispositionReason` explaining why (e.g. "below floor price", "buyer returned no bid", "HTTP timeout")

The modal's **Match Decisions** section lists both winners and excluded rows with their reasons, so any gap between `matchCount` and `totalMatches` is self-explanatory.

##### Cross-navigation

- **Inbound row → Outbound matches**: expand the inbound ping row and click **View Outbound Matches** to see the HTTP pings the auction fired to buyer contracts.
- **Outbound row → Inbound ping**: click the `Ping` column to jump back to the partner ping that triggered the auction (auto-opens the detail modal).
- **Lead → Originating Ping**: on a ping/post lead, the **Tracking** tab shows an **Originating Ping** link back to the inbound ping row.

##### Routing Trace — why didn't my contract ping?

`pingMatch` only records contracts the auction actually HTTP-pinged (winners) and contracts we tried to HTTP-ping but failed/rejected (excluded). Contracts filtered **before** the HTTP stage — cap reached, outside schedule hours, geo mismatch, insufficient balance, contract expired, no active SKU, custom filter fail, etc. — don't produce a `pingMatch` row at all. Pre-April 2026 this meant admins saw "No matches" or "0 total evaluated" with zero diagnostic info.

Every inbound ping now persists a **Routing Trace** — one row per evaluated contract in the `pingDistribution` table — surfaced in the Inbound Ping Log expand row below the match-decisions section. Each row shows:

| Field | Meaning |
|------|--------|
| Rank | The rank assigned by the distribution rule (or `—` if the contract was skipped before ranking) |
| Contract | Link to the contract detail page |
| Buyer | Which buyer the contract belongs to |
| Disposition | `eligible` (made it to auction), `skipped` (pre-HTTP filter removed it), `rejected` (auction ran but HTTP ping failed) |
| Reason | The specific engine reason — e.g. "Cap Exceeded", "Outside Schedule Hours", "Buyer Balance Insufficient", "Contract Expired", "Failed Geo Filter" |
| Price | Contract's base price at evaluation time |

Rows whose filters were evaluated in detail (geo, demographic, custom field matches) are clickable and expand inline to show every filter: field, operator, expected value, actual value, pass/fail badge. Example: if an admin sets a contract filter to `roofType = 'Metal'` and the lead submits `roofType = 'Asphalt'`, the trace row's drill-down shows exactly that mismatch.

This is forward-only — pings created before this feature shipped show "No routing data captured for this ping." The auction still behaves identically; Routing Trace is pure observability.

---

## Verticals

**Location:** [/admin/verticals](/admin/verticals)

Verticals are lead categories (e.g., Auto Insurance, Mortgage, Solar).

### Vertical List

| Column | Description |
|--------|-------------|
| Name | Vertical name |
| Slug | URL-friendly identifier |
| Status | Active, Inactive |
| Fields | Number of standard fields |
| Created | Creation date |

### Vertical Groups {#vertical-groups}

Vertical groups organize verticals into logical categories (e.g., "Mass Tort", "Insurance"). Each group owns its own dedup scope, attribution scope, and attribution policy overrides. Manage groups at [/admin/vertical-groups](/admin/vertical-groups).

The group detail page has four tabs — **Details**, **Verticals**, **Attribution**, and **Dispositions** — with a single save bar at the bottom: edit fields on any tab and one **Save** persists everything that changed (name/scopes and the vertical assignment together). **Cancel** reverts to the last saved values.

- **Details**: Name, **Dedup Scope**, and **Attribution Scope** (offer / vertical / group).
- **Verticals**: Assign verticals to the group with the multi-select list. A vertical can only belong to one group; the section header shows a live "N of M assigned" count.
- **Attribution**: Per-field attribution policy overrides (Origination Window, Assist Calls, Assist Leads, CPA Credit, CPA Window, Match On). Each field has a toggle — off inherits the system default, on takes a custom value. The section collapses to a summary ("System defaults" or "N overrides") when no overrides exist.
- **Dispositions**: Group-scoped disposition codes (see below). Disposition changes save immediately — they do not wait for the save bar.

#### Group Dispositions {#vertical-group-dispositions}

Each vertical group defines its own set of disposition codes. Dispositions have a label, category (working, converted, rejected, returned), and optional triggers for CPA events or returns. **Triggers CPA** and **Triggers Return** are mutually exclusive — turning one on turns the other off. These scoped dispositions are used by intake center disposition mappings. Drag rows to reorder, or use **Clone from Group** to copy another group's full disposition set.

### Creating a Vertical {#creating-a-vertical}

**Location:** [/admin/verticals/new](/admin/verticals/new)

- **Name**: Display name
- **Slug**: URL identifier (auto-generated)
- **Vertical Group**: Select from the dropdown to assign this vertical to a group (e.g., "Insurance", "Home Services"). Groups are managed at [/admin/vertical-groups](/admin/vertical-groups). Each group owns its own set of disposition codes and scopes intake center disposition mappings, helping organize verticals across contracts and offers.
- **Profile Key**: Optional identifier
- **Notes**: Internal notes
- **Status**: Active/Inactive

### Vertical Detail

**Location:** [Verticals](/admin/verticals) > Vertical Detail

#### Home Tab
Basic vertical information and metadata.

#### Settings Tab {#settings-tab}

Vertical settings enforce behavior in the lead processing pipeline. Each setting is checked during lead intake — violations are rejected before the lead is inserted.

**Duplicate Settings** — Detect and reject duplicate leads before they enter the system.
- Enable/disable duplicate detection per vertical
- Choose which fields to match: First Name, Last Name, Email, Phone, Zip Code
- Match condition: AND (all fields must match) or OR (any field match = duplicate)
- Check against: All Leads or Sold Leads only
- Window: number of days to look back for duplicates
- Across: This Vertical, All Verticals, or Select Verticals (cross-vertical dedup)
- Duplicates are rejected before the lead is inserted — no orphan records

**DNC List** — Control suppression list enforcement.
- When enabled (default): both global and vertical-scoped suppression entries are enforced
- When disabled: only global suppression entries are enforced; vertical-scoped entries are skipped
- Global suppression (email/phone hash match) is always enforced regardless of this toggle

**Returns** — Control which return reasons buyers can use.
- Override Global: when enabled, only the selected return reasons are accepted for this vertical
- When disabled (default): all system-level return reasons are accepted
- Buyers submitting a return with a disallowed reason receive a 400 error

**TCPA Compliance** — Enforce compliance artifact requirements.
- Require Consent: reject leads without tcpaConsent=true (enabled by default for backwards compatibility)
- Require TrustedForm: reject leads without a TrustedForm certificate URL
- Require Jornaya: reject leads without a Jornaya Lead ID in custom fields
- Require iScale Consent: reject leads without an iScale consent certificate URL
- Multiple requirements can be enabled simultaneously — all must be satisfied

#### Offers Tab
View offers in this vertical.

#### Lead Fields Tab

Vertical fields define the lead payload for a vertical — what partners may post and what buyers receive. Each field carries:
- Label and field name
- Data type (text, number, boolean, date, select, list, email, phone, url, textarea)
- Required flag
- Sort order

**Select and List fields** support predefined value/label pairs. When a field has options defined:
- The accepted values appear on the partner posting spec page
- Lead submissions are validated against the allowed values — invalid values are rejected
- Options are defined as value/label pairs where "value" is what partners send in the API and "label" is the display name
- All field names and list values are **case-sensitive** — values must match exactly as defined

##### PII Protected {#vertical-fields}

The **PII Protected** checkbox on a field marks it as containing personally identifiable information. PII fields are **masked for users without the PII Access permission**, and the same flag drives redaction in the report drilldowns and the custom report builder. See [PII-Access API Keys](#pii-access-api-keys) for the equivalent control on programmatic access, and [Permissions](#permissions) for the user-level grant.

**PII floor — some fields cannot be unmarked.** Any field of type **email** or **phone**, and any field named like an SSN or a date of birth (`ssn`, `socialSecurity`, `social_security`, `dateOfBirth`, `dob`, `birthdate`, `birthDate`), is forced to PII regardless of the checkbox. On those the checkbox renders checked and disabled: it is a platform floor, not a tenant setting.

#### Documents Tab
Upload documents visible in buyer dashboard.

---

## Lead Scoring {#lead-scoring}

**Location:** [/admin/scoring-models](/admin/scoring-models)

Lead Scoring grades every incoming lead on a **1–10 quality scale** using a **scoring model** you configure. The score is written to the lead, exposed to routing, and can gate which buyers are eligible — without ever rejecting the lead outright. Models are authored point-and-click from the **Scoring Models** admin area; the same models can also be managed through the admin API (`/api/admin/scoring-models`) for scripted or bulk work.

### What a scoring model is

A scoring model is a named, versioned configuration made up of:

- **Factors** — the lead fields that influence the score. Each factor has a **role**:
  - **Weighted** — contributes a graded sub-score (e.g. a `creditBand` of "excellent" scores higher than "poor"). Weights are renormalized across the factors that actually produced a sub-score, so a missing field doesn't silently drag the average down.
  - **Gating** — like weighted, but a matched band can also impose a **gate cap**: an absolute ceiling on the final score. A lead that fails a fundability gate is capped at, say, 3 no matter how strong its other factors are.
  - **Modifier** — a flat +/- adjustment (e.g. "+0.5 if homeowner") applied to the weighted average. Modifiers are applied **before** gate caps, so a bump can never lift a capped lead over its ceiling.
- **Bands** — per factor, the value ranges (using the same match operators as routing filters — equals, greater_than, in, contains, etc.) that map a field value to a sub-score.
- **Routing bands** — non-overlapping integer ranges over 1–10 that map the final score to a machine-key **band label** (e.g. `call_immediately`, `standard`, `nurture`, `deprioritize`). The band is stored on the lead alongside the numeric score.
- **Collected-not-scored** — vertical intake fields you deliberately collect but exclude from scoring (documents that they're intentionally omitted, so the Field Mapping tab doesn't flag them as gaps).

### How a lead gets scored

At score time the engine resolves which model to use — **the offer's override if set, otherwise the vertical's default, otherwise the lead is left unscored** — then runs the model's **live** version: each factor matches the lead's value to a band sub-score, the sub-scores are combined into a renormalized weighted average, modifiers apply, any triggered gate caps clamp the result, and the score is rounded half-up and floored at 1. The final score, its routing band, and a PII-safe **breakdown** (which bands matched, effective weights, contributions, caps triggered — never raw lead field values) are saved to the lead. Routing then consults the score at each contract's quality gate.

<!-- react-flow:lead-scoring-flow -->

### Creating a model

On **Scoring Models** click **New Scoring Model**, give it a name (and optional description), and optionally pick an existing model under **Clone from** to copy its whole config as a starting point — otherwise you begin from a blank 1–10 config. Creating lands you on the model's detail page with an initial draft version (v1) ready to author. The list shows each model's status, live version number, total versions, and how many verticals + offers it's attached to.

### The detail page — five tabs

The detail page has one **Save** bar shared across every tab (edits to any tab collect into a single save), an **Identity** section for name/description, and five tabs:

- **Factors & Bands** — the heart of the model, and the closest thing to a spreadsheet **Lookups** sheet: each factor is a lookup table that maps a field value to a sub-score. Add a factor, pick the intake field (or type a custom field name), choose its role, and add **bands** — one row per value range, each with a match operator, value(s), and a sub-score. Gating factors also expose a per-band **gate cap**. Modifier factors instead take a single **adjustment** (operator, value, +/- amount) and carry no bands. A per-factor **missing-data policy** decides what happens when the field is absent (renormalize away, fall back to a named band, or skip). A sticky **validation strip** at the top counts errors/warnings live, validates the draft against a chosen attached vertical's real intake fields, and lets you jump straight to the offending factor. The **Collected, not scored** section at the bottom is where you declare fields you capture but intentionally don't score.
- **Weights** — shows every weighted/gating factor's weight next to its **auto-normalized percentage** (a live bar). Weights are relative: the engine renormalizes them to 100% across the factors that actually scored, so you never have to make them add up yourself. A weight of 0 drops a factor. Modifiers are listed separately as unweighted.
- **Routing Bands** — map score ranges to outcome **band labels**. A color strip previews coverage across the whole 1–10 scale; the tab flags **overlaps** (blocking) and **gaps** (uncovered scores) as you edit. Labels are machine keys (lowercase letters, digits, underscores) validated on blur.
- **Field Mapping** — reconciles each attached vertical's visible intake fields against the config. Every field must be either **scored** by a factor or explicitly **collected-not-scored**; anything else shows as **UNRESOLVED** (a red row) — collected on the lead but silently ignored by scoring. You can resolve an UNRESOLVED field in place by marking it collected-not-scored. Leaving UNRESOLVED fields is what surfaces as a validation error and **blocks promotion**, so this tab is your pre-flight checklist.
- **Versions** — the version history table (draft → live → retired), a side-by-side config **diff** between any two versions, the current attachments, and the **Promote**, **Import XLSX**, and **Export** actions.

### Versions and promotion

A model has a **version history**. Editing and saving keeps working the same **draft** version; the current in-use configuration is the **live** version. Only the live version scores leads — a model that has only ever had a draft does not score anything yet.

**Promoting** a draft makes it live and retires the previous live version. Promote from the Versions tab: the app first **validates the draft against every attached vertical** and shows the combined errors and warnings in a confirm modal. Blocking errors (e.g. an UNRESOLVED intake field, or a gating factor missing its fallback band) disable the **Promote to live** button until you fix them; warnings are informational. Only your last **saved** draft is promoted, so save before promoting if you have unsaved edits.

### XLSX export / import

Every version can be exported to an `.xlsx` workbook (three sheets: **Lookups**, **Config**, **Routing Bands**) from the Versions tab — handy for reviewing a model in a spreadsheet or handing it to an analyst. **Import** uploads an edited workbook back and creates a **new draft** from it (the live version is never touched). Export → edit → import is a lossless round-trip. A malformed workbook is rejected with row-addressed errors (which sheet and row failed) so you can fix it and re-upload.

### Attaching a model

- **Vertical default** — on a vertical's detail page, set its **Default Scoring Model**. Every lead in that vertical is scored by it unless an offer overrides.
- **Offer override** — on an offer's Overview tab, set a **Scoring Model** to override the vertical default for leads coming through that offer. Leaving it on **Inherit from vertical** shows the inherited model name as a hint, so you can see what the offer will actually use without setting anything.

When you set a non-null model, its configuration is **validated against the vertical's intake fields** first — a factor referencing a field the vertical doesn't collect (or a dangling intake field) blocks the save. At score time the resolved model is: **offer model if set, otherwise the vertical default, otherwise not scored.**

### The contract quality gate

A contract can set a **Minimum Quality Score** (`minQualityScore`, 1–10) on its Routing tab. During routing, a scored lead whose quality score is **below** the contract's minimum is rejected for that contract — it appears in the routing trace with the reason `quality_score_below_minimum`, exactly like a failed filter.

**Unscored leads always pass the gate.** If a lead has no quality score — because no model is attached, or scoring failed — the minimum-score check is skipped and the lead stays eligible. This is deliberate: a scoring outage can never zero out a buyer's deliveries.

### Score consumers (dialer priority + Quality Tier distribution)

Beyond the gate, the quality score feeds two ranking decisions. In both, an **unscored lead stays neutral** — never punished for the absence of a score.

- **AI Dialer call priority.** When the outbound dialer picks its next contact, it still respects each contact's due time (TCPA windows and cadence delays are never violated), but **among contacts that are already due** it calls the highest-scoring lead first. Order is `quality score DESC, then earliest-due`. An unscored contact ranks as a **neutral 5** — ahead of a 3, behind an 8. A low scorer is deprioritized but never starved: it is still called, just later. A contact that isn't due yet is never pulled forward.
- **Quality Tier distribution rule.** An offer whose **Distribution Rule** is set to **Quality Tier** ranks eligible contracts by `Minimum Quality Score DESC, then price DESC, then priority DESC`. Premium-gated contracts get first pick of the leads that clear their bar; ungated contracts (no minimum set) are the floor tier — even at a higher price. Because a lead's score is constant for that lead, it cannot order the contracts; the contracts' declared score appetite does. **Unscored leads** already pass every minimum-score gate, and the tier ordering is unchanged for them — the highest-gated eligible contract still ranks first.

### Seeing scores on leads

- **Leads list** — a **Quality** column shows each lead's 1–10 score and its routing band badge, plus a small **degraded** tag when the score carried one or more scoring flags. Filter the list by **Routing Band** or by a **Minimum Score**. (The separate **AI Score** column — see below — is a different number and is hidden by default.)
- **Lead detail** — the **Lead Quality Score** panel breaks the score down: the final score and band, a per-factor table (band matched, sub-score, effective weight, contribution), any modifiers, a callout when the score was **capped** by a gate, and any **flags**. Unscored leads show a simple "This lead has not been scored." The panel reads only scoring fields — never a raw contact value.

**Degraded flags** mark a score the engine still produced but with reduced confidence — worth a glance before trusting it. The common ones: `MISSING_WEIGHTED`/`MISSING_MODIFIER` (a scored field was absent), `UNBANDED_VALUE` (a value matched no band), `COARSE_MAPPING` (a band flagged as an approximate mapping), and **`GATING_FALLBACK`** (a gating factor's field was missing, so its configured fallback band was used to decide the cap instead of a real value). A fully unscorable lead carries `NO_SCORABLE_FACTORS` and stays unscored.

### Quality Score vs AI Score

These are two different numbers — don't conflate them:

- **Quality Score (1–10)** — the deterministic, operator-authored score from a scoring model described in this section. Rule-based, explainable, and gate-able via `minQualityScore`.
- **AI Score (0–100)** — the platform's built-in **AI conversion score** (see [AI-Powered Features](#ai-powered-features)), a heuristic likelihood-to-convert signal computed automatically at submission. It is informational and does not feed the contract quality gate.

### Compliance boundary (important)

Lead Scoring is a **triage tool, not an eligibility engine**. Two hard rules are built into the platform:

1. **Scoring never rejects a lead.** A low score still creates the lead and still routes it. Scoring only influences delivery through the explicit, buyer-configured contract quality gate — never by discarding leads.
2. **Scoring runs in its own phase and never fails intake.** Scoring happens after cap checks and before routing. If the engine errors, the lead is simply left unscored and routing proceeds normally. Scoring is never on the critical path for accepting a lead.

Do not use quality scores as a proxy for consent, compliance, or fundability decisions that belong to your buyers and their contracts.

---

## Reports {#reports}

**Location:** [/admin/reports](/admin/reports)

### Filtering Report Data {#report-filters}

Reports that support filtering show an **+ Add Filter** button next to the date range picker. Click it to pick a dimension (Buyer, Partner, Campaign, Vertical, Offer, Contract, Revenue Type, or Lead Type), then multi-select the values you want to include. Selected filters render as removable chips above the report; click the chip text to edit, click the X to remove, or click **Clear** to remove all filters at once.

Filters combine with AND semantics — adding Buyer=Acme AND Partner=Globex returns only leads that match BOTH. Within a single dimension, values are OR'd (Buyer=Acme,Globex matches either buyer). The **Revenue Type** and **Lead Type** dimensions behave as toggles that skip entire query branches (e.g. Lead Type=Call skips the lead-revenue queries entirely rather than adding a WHERE clause).

Filter state lives in the URL query params (`?buyerId=abc,def&partnerId=xyz`), so any filtered view is shareable — copy the URL and send it. The Revenue report supports all 8 filter dimensions.

### Leads Report

**Location:** [/admin/reports/leads](/admin/reports/leads)

Overview of lead statistics:
- Total leads
- Sold count
- Conversion rate
- Total revenue
- Total cost
- Profit

Filter by date range.

### Trending Report {#trending-report}

**Location:** [/admin/reports/trending](/admin/reports/trending)

Line-chart view of lead-count trends over a chosen date range. Default window is the last 14 days in your tenant timezone. All metrics count by lead submission date (`lead.createdAt`), excluding test and trashed leads.

Two selectors drive the view:
- **Metric** — `Leads` (all submitted), `Sold`, `Retainer` (CPA-converted sales), `Returned`, or `Rejected`.
- **Pivot By** — `Day` shows only the chart and summary cards; `Contract`, `Partner`, `Buyer`, or `Campaign` adds a pivot table with one row per dimension value and one column per day in the range.

Top cards show the total, daily average, and peak day. The chart plots a single series of daily counts. When a pivot is active, the table below sorts by total desc and rolls the 26th and later rows into an "Other" row. All filters (buyer, partner, campaign, vertical, offer, contract) apply — buyer and contract filters match via leadSale, so they include multi-sold leads.

### By Buyer Report

**Location:** [/admin/reports/by-buyer](/admin/reports/by-buyer)

Revenue and metrics grouped by buyer.

### Buyer Budgets Report {#buyer-budgets}

**Location:** [/admin/reports/buyers/budgets](/admin/reports/buyers/budgets)

A nightly-ops view of how many leads we need to deliver tomorrow, per product. The FB ops team checks this each evening and adjusts campaign budgets for the next day so we hit each buyer SKU's daily cap without overspending on unsellable inventory.

One row per active buyer SKU that has both a `dailyLeadCap > 0` and an active contract attached. Products without a daily cap or with no active contract are excluded — they have no defined target to size against.

**Columns:**

- **Buyer** — the buyer that owns this SKU.
- **Vertical** — the vertical of the contract feeding this SKU (`—` if not set).
- **Product** — `product.name` from the contract's linked product, or the linked SKU's name if no product is set. The `contractDisplayId` shows below as a small reference. We never fall back to `contract.name` — that field often embeds buyer names and would misrepresent the product.
- **States** — count of US states accepted by `contract.geoFilters.states`. Renders as a small **All** badge for contracts with no state restriction (nationwide). Sortable; the **All** rows always group at the end of the sort.
- **Daily Cap** — `contract.dailyLeadCap`. The maximum leads this buyer will accept per day for this product.
- **Leads Remaining** — `buyerSku.leadsRemaining`. Leads still unfulfilled from the buyer's original purchase quantity (the total purchase backlog).
- **Delivered Today** — leads delivered to this product so far today, in tenant timezone (midnight → now).
- **Projected Today** — end-of-day projection if delivery pace holds, capped at remaining inventory and remaining daily cap. Shows `—` until at least 10% of the day has elapsed so early-morning noise doesn't drive the number.
- **Shortfall** — actionable leads needed to fill today's cap, capped at remaining inventory and never below 0. Renders in red when partner action could fill more, and `—` when projection isn't available yet.
- **Tomorrow To Deliver** — `min(leadsRemaining, dailyLeadCap)`. This is the number to plug into tomorrow's ad budget: never deliver more than the daily cap, never deliver more than the backlog.

A bold **Totals** row at the bottom of the table sums every numeric column across the visible rows (Daily Cap, Leads Remaining, Delivered/Projected Today, Shortfall, Tomorrow To Deliver, and total States across non-nationwide contracts). Headers stick to the top of the viewport as you scroll, so the column meanings stay visible on long lists.

**Filters:** Buyer, Vertical, Product, and Offer. Each is a searchable dropdown; filters update the URL so a view is shareable. The totals card at the top sums `Tomorrow To Deliver` across the filtered rows so you can read off the FB campaign budget in one number.

**Workflow:** Each evening, the FB ops team opens this report, optionally filters to a buyer or vertical, sorts by `Tomorrow To Deliver` desc, and uses the per-product target — combined with the partner's typical CPL — to size the next day's spend.

#### View by State

A second view — **By State** — pivots the same data by US state instead of by contract. Toggle at the top of the page. Useful when sizing geo-targeted ad campaigns: instead of "buyer X needs 200 leads tomorrow", you get "Florida needs 70 leads across 4 contracts, Texas needs 45 across 2".

Each contract's `Tomorrow To Deliver` is split across its accepted states (`contract.geoFilters.states`) proportionally to its last-7-day delivered share per state. So a contract that delivered 70 FL / 30 TX over the last week, targeting 100 leads tomorrow, contributes 70 to FL and 30 to TX. With no delivery history, the split is equal across the contract's accepted states. Largest-remainder rounding keeps the per-state allocations summing exactly to the contract's target — no leakage.

Contracts with no state targeting (empty `states`, or `exclude: true` lists) bucket as **Nationwide (any state)** — they could deliver to any state, so we don't try to attribute them. The total target in the totals card is invariant across views since every contract's full number is distributed somewhere.

State view columns:

- **State** — the state name (Florida, Texas, …) or *Nationwide (any state)*.
- **Tomorrow To Deliver** — sum of allocated targets from every contract contributing to this state.
- **Delivered Today** — sum of leads delivered today across all contracts that include this state (tenant timezone, midnight → now).
- **# Contracts** — count of active contracts contributing to this state.

Click any state row to expand and see the contracts contributing to that state's target. Each contributing contract shows its buyer + product, its per-state allocated portion of tomorrow's target, and its delivered-today count attributed to that state (or its full delivered count for Nationwide rows).

### Buyers by State Report

**Location:** [/admin/reports/buyers-by-state](/admin/reports/buyers-by-state)

Geographic coverage view of every in-scope contract (status `active` or `paused`). One row per US state + DC, with **Active** and **Inactive** columns. Hover any non-zero count to fly out a list of the buyer-contracts in that bucket; each item links to the contract detail page.

- **Active** means the buyer is `active`, the contract is `active`, and — if the contract is SKU-funded — the SKU is `status='active'`, `routeabilityStatus='routeable'`, has `leadsRemaining > 0`, and `subscriptionPauseStatus='active'`.
- **Inactive** is everything else in scope: paused contracts, buyers in `inactive`/`pending`, SKUs that are out of budget, paused, canceled, disputed, or with a paused/canceled subscription.
- A national contract (no `geoFilters.states`) counts toward every state. A list with `exclude: true` counts everywhere except the listed states.
- Contracts in `setup`, `expired`, or `archived` are not shown.

### By Campaign Report

**Location:** [/admin/reports/by-campaign](/admin/reports/by-campaign)

Revenue and metrics grouped by campaign.

### Revenue Report

**Location:** [/admin/reports/revenue](/admin/reports/revenue)

Detailed revenue analysis.

### Call Reports {#call-reports}

**Location:** [/admin/reports/calls](/admin/reports/calls)

First-class landing for inbound call analysis, gated by the same `permReports` permission as Custom reports. It wraps the shared call drilldown engine, so every view respects the report date range, filters, and CSV export.

Quick-pick presets swap the pivot dimension without leaving the page:

| Preset | Pivots by |
|---|---|
| By Campaign | Campaign |
| By Buyer | Buyer |
| By State | Caller state |
| By Hour | Hour of day |
| Transfer Performance | Transferred vs not (then campaign) |
| By Hangup Cause | Carrier hangup cause |

**Call Drilldown** ([/admin/reports/calls/drilldown](/admin/reports/calls/drilldown)) — Ringba-style pivot report drilling from Campaign down to Call Buyer. Columns: Calls, In Progress (not yet completed/missed/failed), Unique (distinct caller numbers), Receivable / Receivable Duplicate / Payable (sale counts), Revenue, Payout, EPC, CPC, Cost (telco cost only), Profit. A picker-only **Service Fee** column sums the [platform's per-call charge to you](#call-service-fee) for the group — always USD, exported as **Service Fee (USD)**. The time basis defaults to **Time of Lead/Call** (calls bucketed by when they started) — switch the selector to Time of Revenue to window by sale/conversion date instead; calls with $0 sales and no CPA conversions only appear under the call basis. Click a row's **Total** count to open People → Calls filtered to exactly those calls (campaign/buyer drill filters plus the report date range).

**Hangup cause.** Every completed, missed, or failed call now records *why* the carrier leg ended. Provider-specific reasons (Twilio SIP codes, Telnyx `hangup_cause`, SignalWire FreeSWITCH strings, Bandwidth `cause`) are normalized into one set — `normal_clearing`, `busy`, `no_answer`, `rejected`, `timeout`, `canceled`, `failed` — with anything unrecognized preserved as `unknown:<raw>`. The normalized cause appears on the admin call detail Info tab, in the buyer portal call list, and as the **By Hangup Cause** report dimension. Capture is forward-only; calls that ended before this shipped have no cause.

**Native attribution fields.** Each call records its marketing attribution independently of the original DNI impression: source, medium, campaign, term, content, click IDs (`gclid`, `fbclid`, `msclkid`, `ttclid`, `gbraid`, and `wbraid`), plus `sub1` through `sub10`. These fields are available in call lists, call detail, custom-report dimensions, CSV exports, API responses, and call-event webhook tokens. Historical calls are backfilled only where a native value is missing; existing call values are never overwritten. Buyer-facing output continues to apply the account's PII policy.

**Scoped call webhooks.** The Webhooks sections on vertical, buyer, and contract detail pages can subscribe to call events for only that entity. A scoped webhook fires only when the call sale belongs to the selected vertical, buyer, or contract; campaign, offer, call-specific, and global scopes remain available in their existing locations.

### Contracts Reports {#contract-summary-report}

The Contracts report comes in three variants under **Reports → Contracts**:

| Variant | Path | Pivots | Time axis |
|---|---|---|---|
| Summary | `/admin/reports/contracts` | Contract (default), Buyer, Vertical, Offer, Campaign | Revenue date |
| Time of Revenue | `/admin/reports/contracts/time-of-revenue` | Day (default), Week, Month | Revenue date |
| Time of Lead | `/admin/reports/contracts/time-of-lead` | Day (default), Week, Month | Lead date |

All three render the same columns: Leads, CPA Events, CPA Ratio, Sales, Revenue, Avg Price, Accept Rate, Daily Cap and Cap Used Daily/Monthly (Summary + Contract pivot only), Refunds, Refund Amount, Net Revenue.

**Filters:** date range, vertical, buyer, offer, contract, campaign, and contract status (active, paused, expired, setup).

#### Summary

**Location:** [/admin/reports/contracts](/admin/reports/contracts)

A contract-pivoted view of sales, revenue, accept rate, refunds, and cap utilization. Use it to monitor contract health, compare buyers head-to-head on the same offers, and surface underperforming contracts before they bleed budget.

**Drill-down:** when the pivot is set to Contract, clicking a row jumps to `/admin/leads` pre-filtered to that contract and the report's date window. Other pivots (Buyer, Vertical, etc.) don't drill down — change the pivot to Contract first if you want the lead list.

#### Time of Revenue

**Location:** [/admin/reports/contracts/time-of-revenue](/admin/reports/contracts/time-of-revenue)

Same metrics as Summary but rolled up into time buckets (day/week/month). Windowed by the **revenue date**: `leadSale.createdAt` for sales/leads, `leadSale.cpaConvertedAt` for CPA events, `refund.processedAt` for refunds. A lead created last month but converted today shows in today's bucket.

#### Time of Lead

**Location:** [/admin/reports/contracts/time-of-lead](/admin/reports/contracts/time-of-lead)

Same shape as Time of Revenue but windowed by **lead date** (`lead.createdAt`) for every metric. A lead created today and converted three weeks from now will count in today's bucket, not the conversion-day bucket. Use this view when you want to see the downstream performance of leads originated in a given period.

#### Metric definitions

**Leads** = distinct leads sold to the contract (or pivot dim) in the window. Multisell counts as one lead per buyer.

**CPA Events** = count of CPA conversions attributed to this contract. On the Summary and Time of Revenue views, windowed by `leadSale.cpaConvertedAt` — a lead sold last week and converted today shows in today's window. On Time of Lead, windowed by `lead.createdAt` along with everything else.

**CPA Ratio** = CPA Events ÷ Leads. Blank when no leads in the window.

**Accept Rate** = sales ÷ distribution attempts in the window. A 50% accept rate means half of the leads sent to the contract were sold. Blank when there were no attempts (so empty contracts don't read as 0% by mistake).

**Daily Cap** is the configured `contract.dailyLeadCap` value for the contract. It is point-in-time contract setup data, not scoped to the report date range. Blank when no daily lead cap is set. Only rendered on the Summary view when the pivot is Contract.

**Cap Used Daily / Monthly** is the highest single-day fill ratio inside the date range. Daily uses `max(daily sales) ÷ contract.dailyLeadCap`; Monthly uses `max(monthly sales) ÷ contract.monthlyLeadCap`. Both columns blank when the corresponding cap is unset on the contract. Only rendered on the Summary view when the pivot is Contract.

**Net Revenue** = gross revenue − approved refunds processed in the window. Net can be negative when refunds clear revenue booked earlier (e.g. a backdated refund landing inside a tight reporting window).

Use **Export CSV** on any variant to download the same rows currently rendered, including filters, pivot, and date range. The CSV uses tenant-local dates and matches the on-screen totals exactly.

### Custom Report

**Location:** [/admin/reports/custom](/admin/reports/custom)

Build custom reports with flexible filters.

### Reports Library {#reports-library}

**Location:** [/admin/reports/library](/admin/reports/library) (Reports → Library in the admin sidebar)

The Report Library is the central place to browse pre-built report templates and saved reports your team has published. The page is split into two tabs:

- **Saved Reports** — reports your team has published to the library, organized into folders.
- **Templates** — pre-built starter reports (the 13 media-buying templates listed below).

**Folders.** Admins create folders on the Saved Reports tab to organize the library — common patterns are buckets like "Weekly Reports", "Buyer Performance", or "Partner Audits". Click **+ New Folder** to add one. Deleting a folder does not delete the reports inside it; they move to **Uncategorized** so nothing is ever lost.

**Saving and publishing.** Save a report from any drilldown report page using the **Save** button in the toolbar. Saved reports are private by default — only you see them under the **Saved (n)** dropdown on the Custom Report page. To make a report visible to your team, toggle **Visible to everyone** in the Save dialog and pick a folder to publish it into the library. You can also publish later: open the Saved dropdown next to a private saved report and click **Publish** to push it into a folder.

**Attribution.** Every report in the library shows **Published by {name}** alongside the publish date so the team knows who curated each one.

**Delete rules.**

- Anyone can delete their own private saved reports.
- The publisher can delete their own published reports.
- Admins can delete any published report in the library.
- Private saved reports are owner-only — even admins cannot delete another teammate's private draft.

**Templates tab.** A grid of 13 pre-built media-buying performance reports, each scoped to your tenant. Templates are starting points: clicking a card opens the Custom Report drilldown UI with the template's dimensions, metrics, filters, and sort already applied. From there, tweak any setting and use **Save** to keep your variant (private by default, or publish to the library as above) — the original template stays untouched.

**Lead reports**

| Template | Use it to |
|---|---|
| All Leads | Wide overview of every submitted lead with sold/retained counts and gross profit, grouped by day → vertical → campaign → partner → contract → S1 → status → disposition. |
| Buyer Performance | Compare buyers on retain rate and DQ rate. Rows tint amber when retain rate falls below 30% and red when DQ rate climbs above 20%, surfacing problem accounts at a glance. |
| Placement (S1) | Performance grouped by partner sub-id 1, the typical "placement" or media slot identifier. Useful for cutting underperforming placements without touching the campaign. |
| Vertical Overview | Roll up lead volume, revenue, and profit by vertical for the period. Best for high-level "where's the business coming from" reads. |
| S1 Campaign Overview | S1 broken down within campaign — same as Placement (S1) but adds the campaign dimension first so you see placements grouped by their campaign parent. |
| Hour of Day | Lead volume bucketed by `hour_of_day` × `day_of_week` in your tenant timezone. Identifies when leads come in and how performance differs by daypart. |

**Sold-lead reports**

| Template | Use it to |
|---|---|
| Sold Leads | Per-sale view with payout, revenue, margin, and CPA conversion. Grouped by day → buyer → contract → vertical. Use it as the source of truth for what was sold and to whom. |
| Contract Performance | Buyer and contract scorecard with retain rate, DQ rate, and cost-per-retained. Use it for renewal and rate-change conversations. |
| DQ Reason | Counts and rates of every distinct DQ reason returned by buyers, grouped by buyer and contract. Surfaces systemic data-quality issues. |
| Intake Disposition | Latest disposition per sale, grouped by intake center and disposition code. Gauges intake-center quality. |
| Creative (S4) | Performance grouped by partner sub-id 4, conventionally the creative/ad identifier. Pairs with Placement (S1) to separate creative effects from placement effects. |
| TRT (Time to Retain) | Average, min, and max days from sale delivery to CPA conversion. Outliers beyond 365 days are excluded. Use it to set realistic retainer-window expectations and tune attribution windows. |

**Call reports**

| Template | Use it to |
|---|---|
| Pay Per Call | Call volume, transfer rate, average IVR/transfer durations, and revenue per call grouped by campaign and partner. Source: call + callSale tables. |

**Filters.** Every template starts with a default date range (typically "This Month"), but the date picker, partner filter, vertical filter, and any other filter on the Custom Report page are fully editable after the template loads. The "Loaded from template" banner at the top includes a **Reset to default** link that returns the report to the template's pristine state.

**Saved variants.** Saved reports store the dimensions, metrics, filters, date range, and sort. Private saved reports are scoped to your user — other users in your tenant don't see them until you publish. See the Saving and publishing notes above for how to push one into the library.

**Permissions.** The Report Library and Custom Report drilldown require the **Reports** permission (Settings → Users → edit user → Reports toggle). Only admins can create, rename, or delete folders. Users with PII access disabled see masked values for `city`, `zipCode`, `state`, and `callerNumber` columns; all metrics and aggregates remain unmasked.

### CSV Export {#csv-export}

Export leads, calls, or people as CSV files from the admin list pages.

**Requirements:** Your user account must have the **Data Export** permission enabled (Settings → Users → edit user → Data Export toggle).

**How to export:**
1. Navigate to the Leads, Calls, or People list page
2. Click the **Export CSV** button in the filter bar
3. Choose **Export filtered** (applies your current search/filters) or **Export all** (ignores filters)
4. The CSV file downloads automatically

**What's included:**
- **Leads:** One row per sold lead-sale. Includes lead fields, vertical custom fields, offer details, buyer/contract info, sale pricing, CPA status, and delivery outcome. Multi-sell leads produce multiple rows.
- **Calls:** One row per sold call-sale. Includes call fields, tracking number, person info, buyer/contract details, pricing breakdown, the [Service Fee (USD)](#call-service-fee) you paid the platform, and disposition.
- **People:** One row per person. Includes contact info, lead/call counts, total revenue, tags, and activity dates.

**Notes:**
- Large exports stream in batches — no row limit within Vercel's 5-minute timeout
- Each export is logged in the audit trail with entity type, filters used, and row count
- CSV includes a UTF-8 BOM for Excel compatibility

#### Call Charges Feed {#call-charges-feed}

`GET /api/admin/export/call-charges` is an incremental feed of call-tracking charge events for reconciliation integrations — each poll returns only the charges you haven't pulled yet. Every row carries an opaque **cursor**; pass the last one back as `?since=` on the next poll to get strictly newer rows (it's also returned in the `X-Next-Cursor` response header). Output is CSV by default or JSON with `?format=json`. The feed covers both billing modes: per-call charges carry a `callId` and a `callerId` (the caller's phone number — masked as `***-***-1234` unless the key has PII access), daily-aggregate charges have a blank `callId` and `callerId` — and it includes premium-destination surcharge charges (`service` = `call_tracking_premium`). A read-only admin API key works (the endpoint is GET-only), gated by the same Data Export permission as the exports above. Full parameters, paging, and the response schema are in Help > API Reference.

---

## Settings {#settings}

**Location:** [/admin/settings](/admin/settings)

### Buyer Portal Menu {#buyer-portal-menu}

**Location:** Settings > Buyers > Menu

Controls which items each buyer sees in their portal sidebar. Hiding an item does three things
together: it disappears from the sidebar, its page redirects to the buyer dashboard, and its API
returns `403` — for portal sessions and buyer API keys alike. There is no "visible but read-only"
state, and no way to reach a hidden section by typing its URL.

**The Dashboard cannot be hidden.** It is where every blocked page sends the buyer, so hiding it
would make the portal unreachable. The Balance card stays on the dashboard even when Billing is
hidden.

**Settings > Buyers > Menu sets the tenant-wide DEFAULT.** Three levels can override it, each on
its own detail page:

| Level | Where | Beats |
|---|---|---|
| Vertical | Vertical detail > Settings > Buyer Menu | the tenant default |
| Offer | Offer detail > Overview > Buyer Portal Menu | the vertical |
| Buyer | Buyer detail > Home > Buyer Portal Menu | everything, in both directions |

Each row is a three-way choice — **Inherit**, **Shown**, **Hidden**. Inherit shows you what the
item currently resolves to, so you can see the effect without leaving the page. "Hidden" is a real
setting, not an absence: an explicit Hidden at offer level beats a Shown at vertical level.

**The merge rule is "any path that enables it wins."** A buyer resolves through *every* contract
they hold: for each contract the most specific level with an opinion applies
(offer, else vertical, else the tenant default), and the buyer sees the item if **any** of those
paths says shown. A buyer-level override skips all of that and applies directly. A buyer with no
contracts at all falls back to the tenant default.

Note that the offer page's "Inherit (currently …)" preview resolves through the *offer's* vertical,
while a real buyer resolves through their *contract's* vertical — these can differ. The buyer-level
preview is the authoritative one.

**This composes with the other two gates, it never replaces them.** An item is visible only when
the menu allows it AND the buyer's contract mix supports it AND the tenant feature is on. Showing
Calls in the menu does not give Calls to a buyer with no call contracts.

Changes take effect on the buyer's next full page load. Tenant-level changes can take up to a
minute to propagate across servers; vertical, offer and buyer overrides are immediate.

### Account team visibility {#buyer-portal-account-team}

**Location:** Settings > Buyers > Menu > Account Team

Two independent toggles decide whether a buyer sees the **account manager** and the **sales rep**
assigned on their buyer record. When on, a *Your Account Team* card appears on the buyer's dashboard
and at the top of their help centre.

**Both are ON by default**, including for tenants that have never opened this tab — the same
absent-row-means-shown convention the menu items use. Turn either off if you would rather buyers
came through a shared inbox.

The card is drawn from the assigned user's own profile (Settings > Users), and each field renders
only if it is filled in:

| Field | Source on the user record |
|---|---|
| Name | Full name, else first + last name, else the email address |
| Job title | Job Title |
| Photo | Profile picture, else the account avatar |
| Email | Email — rendered as a `mailto:` link |
| Phone | Work phone (with extension), else office phone, else mobile — rendered as a `tel:` link |

**Nothing appears when nobody is assigned.** A buyer with no account manager and no sales rep sees
no card at all, so turning these on cannot expose an empty or half-filled block. Assign the people
on the buyer's own detail page (Buyer detail > Home).

Two safety properties worth knowing. A stale assignment pointing at a user in another tenant, or at
a deleted user, publishes nothing — the lookup is tenant-scoped and re-checked, and `salesRepId`
carries no foreign key, so a dangling id there is a normal state of the data. A banned user is never
published either. And if one person holds both roles they appear twice, once under each label.

Changes take effect on the buyer's next full page load, and can take up to a minute to propagate.

### How the buyer portal narrows {#buyer-portal-gating}

A buyer sees a section only when all three narrowings agree: the **tenant feature flag**, the
**buyer's own contract mix** (with a fallback that always keeps history they paid for reachable),
and the **menu cascade** you control above. The buyer's help centre follows exactly the same rules —
its manual is filtered by the same three answers, so it can never document a page that is hidden
from that buyer's sidebar. The diagram below also traces where a field's "Learn more" link goes, and
when the account team card appears.

<!-- react-flow:buyer-help-gating -->

### System Settings {#system-settings}
General system configuration.

#### Attribution Settings {#attribution-settings}
Control how partner credit is assigned for leads and calls. Settings include origination window (days attribution lasts), assist behavior for leads and calls from different partners, CPA credit assignment (originating vs. most recent partner), CPA expiration window, and match-on fields for linking leads/calls to existing engagements. These defaults can be overridden at the vertical group or offer level.

#### Telephony Settings {#telephony-settings}
Telephony (voice, SMS, and email) is **managed by the platform** — SignalWire is the primary number provider, with Twilio and Telnyx available for existing numbers. The **System &rsaquo; Providers** page and nav entry are hidden entirely unless your tenant is BYO-enabled; if you don't run your own carrier account, telephony is fully platform-managed and there is nothing to configure. For BYO tenants, [System &rsaquo; Providers](/admin/system/providers) is a read-only status page: tenants no longer add or edit their own credentials there. Use the **System Providers** and **Defaults** tabs to pick which available provider is the default for each capability (voice, SMS, etc.). If a special deal requires your own (BYO) credentials, your account contact attaches a tenant-scoped credential for you; it then resolves ahead of the platform default automatically. Behind the scenes, credentials resolve as own (BYO) → system (inherited platform default); there is no environment-variable fallback.

#### Event Import {#event-import}
Bulk-import disposition events by pasting CSV data. Columns: type, identifier, contractId, status, disposition, payout, occurredAt. For leads, identifier is the soldLeadId (leadSale UUID). For calls, identifier is the callerPhone number. Useful for backfilling historical events or syncing data from external systems.

### Routing Settings {#routing-settings}

**Location:** [Settings](/admin/settings) → **Defaults** group → **Routing**

**Max Buyer Response Timeout (seconds)** — how long the system waits for a buyer HTTP response before timing out. A buyer that exceeds it is treated as a delivery failure: no charge is booked, and on an exclusive offer the waterfall moves to the next eligible buyer.

- **Default 30 seconds** when the setting has never been saved.
- **A contract overrides it.** The timeout on the contract's *primary* delivery action wins over this tenant default; the tenant value applies only when the primary action leaves its timeout unset.
- **Clamped by the remaining request budget.** Whichever value applies, a single buyer never gets more than the time left in the lead-processing run's ~280-second budget — late buyers in a long waterfall can be cut short before their own timeout elapses.

### Security Settings {#security-settings}

**Location:** [Settings](/admin/settings) → **Security** group → **Security**

Tenant-wide authentication and access controls.

**HIPAA mode.** A single master switch that enforces a compliance floor. Two rules are actually enforced when you save: **2FA is required for all three roles** (admins, buyers, partners — the save is rejected if you try to turn one off), and the **session idle timeout must be ≤ 30 minutes**. The three 2FA toggles are the only controls locked while HIPAA mode is on; idle timeout, session timeout, max concurrent sessions, lockout, and Google OAuth all stay editable. The summary panel under the switch quotes a stricter target (idle ≤ 15 minutes) than the save actually enforces — treat it as guidance, not a limit.

**Session idle timeout.** Minutes of inactivity before automatic logoff. Default is **1440** (24 hours). HIPAA requires automatic session termination — use 15–30 minutes for any tenant handling PHI.

**Two-factor requirements** — set independently per portal:

| Toggle | Enable it when |
|---|---|
| Require 2FA for Admins | Recommended for any tenant with access to PII or financial controls |
| Require 2FA for Buyers | Buyers can see lead PII or download exports |
| Require 2FA for Partners | Partners can manage posting keys or reach lead detail pages |

Superadmins always require 2FA regardless of these settings.

#### New User Defaults {#new-user-defaults}

Two toggles that decide what a user starts with when you invite them. Both ship **off**, and
both are also asked during the setup wizard's **Data Access** step.

| Toggle | Off (default) | On |
|---|---|---|
| New users can see PII | New users see leads, routing, and reporting with names, phones, and emails **masked** | Every user you invite starts with full customer contact details |
| New users can export data | New users work in the dashboard and cannot download CSVs | Every user you invite can export leads, calls, people, logs, and reports immediately |

**They apply at creation only.** Changing a toggle never alters access a user already has, and
never overrides a value you set by hand on an individual user's profile. To change an existing
user, edit them directly — the toggles are a starting point, not a policy that re-applies.

**A user who belongs to more than one tenant is defaulted once**, by the tenant that created
their account. Accepting a later invitation into a second tenant does not re-default them.

**Why they default closed.** Export is the higher-stakes of the two: a masked screen and an
unmasked CSV disclose the same information, but only the file leaves the building, and
revoking access never recalls one already downloaded. Granting access later is one click.

The four combinations map onto real roles:

| PII | Export | Fits |
|---|---|---|
| Hidden | Off | New hires, trials, contractors |
| Hidden | On | Analysts — full volume and revenue data, contact details masked |
| Visible | Off | Support and ops — can look someone up, cannot bulk-download |
| Visible | On | Owners and senior staff |

Account **owners and superadmins can always export**, regardless of the export toggle and of
their own profile setting.

**Your own 2FA.** Set up a TOTP authenticator (Google Authenticator, Authy, 1Password, etc.): scan the QR code, then confirm with a 6-digit code. Save the one-time backup codes shown during setup — each works once if you lose your authenticator.

#### Passkeys {#passkeys}

Also on Settings → **Security**. A passkey (Face ID, Touch ID, or a hardware security key) gives passwordless sign-in.

**Where a passkey counts as your second factor.** Only on the **magic-link sign-in** path: a user who holds at least one passkey is not pushed into TOTP setup and does not burn a grace login, even when 2FA is required for their role. It does **not** cover two other cases — **superadmins** are always sent to TOTP setup at login regardless of passkeys, and the "set up two-factor authentication" prompt on the profile/settings pages keys off the TOTP flag alone, so it keeps nagging a passkey-only user. Enroll TOTP as well if you want those to go quiet.

- **Maximum 3 per user.** This is a limit in the settings UI — the Add button disables once you hold three passkeys; delete one to add another.
- **Name** is a label so you can tell them apart later — "MacBook Touch ID", "iPhone Face ID". Rename any time from the list.
- **Device type** shows how the passkey is stored: **Synced** means it is backed up across your devices (e.g. iCloud Keychain), so losing one device doesn't lose the passkey. **Single device** means it exists only on that device — if the device is lost, delete the passkey and enroll a new one.

Passkey sign-ins appear in the [Audit Trail](#audit-trail--activity-log) alongside magic-link and Google logins.

**IP rules.** Allow-list or block-list IP addresses for the account:

- **Block** denies the IP regardless of allow rules. **Block always wins over Allow.**
- **Allow** permits only listed IPs. As soon as *any* allow rule exists, every IP that doesn't match one is denied — so add your own address before saving the first allow rule.
- **Address** takes a single IPv4 address (`203.0.113.7`) or a CIDR range (`10.0.0.0/24`, which covers every address in the block).
- **Expires** is optional auto-expiry; after that date the rule stops applying. Leave it empty for a permanent rule.

Related: [Magic Link IP Mode](#security-magic-link-ip-mode) controls what happens when a login link is clicked from a different IP than it was requested from.

### Compliance Settings {#compliance-settings}

**Location:** [Settings](/admin/settings) → **General** group → **Compliance & Legal**

**TCPA terms text** — optional TCPA and legal disclosure copy. When set, it appears on the public posting specification page for **every campaign under this tenant**. Leave it blank to hide the section. Per-page behavior is described under [TCPA / Legal Disclosure on Posting Spec Pages](#posting-spec-tcpa).

### Email (SMTP) Defaults {#system-email-defaults}

**Location:** [Settings](/admin/settings) → **Connections** group → **System E-Mails**

The From address and transport used for platform email (delivery actions, notifications, invitations). The Provider control offers **Resend** (API key) or **Legacy SMTP** (host/port/credentials); the SMTP fields below appear only when Legacy SMTP is selected.

| Field | Notes |
|---|---|
| From Email / From Name | The address and display name recipients see |
| Host | SMTP server hostname (e.g. `smtp.gmail.com`, `smtp.sendgrid.net`) |
| Username | SMTP authentication username — usually the same as the email address |
| Password | SMTP password or app-specific password. Encrypted at rest; leave blank when editing to keep the current value |
| Port | 587 (TLS), 465 (SSL), or 25 (unencrypted) |
| Auth Enabled | Enable SMTP authentication — required by most providers |
| SSL Enabled | Use SSL/TLS for the connection |

**Per-tenant inheritance.** A tenant with **inherit SMTP** enabled sends through the system default configuration; disable it to point that tenant at its own SMTP server. Resolution order is tenant-specific config → system default. Provider-specific setup walkthroughs are under Messaging: [Amazon SES Setup](#ses-setup) and [SendGrid Setup](#sendgrid-setup).

### Admin API Keys {#admin-api-keys}

**Location:** System → **API Keys** (`/admin/api-keys`)

API keys let integrations call the platform programmatically as `Authorization: Bearer lr_xxx`. Each key carries one or more **scopes** that decide which surface it can reach:

- **Admin (full dashboard)** — the key can do anything you can do in the admin dashboard for your tenant. It hits every `/api/admin/*` endpoint and resolves *your* live permissions: the key literally acts as its creator, so if your account is later demoted from admin or deactivated, the key stops working automatically. Treat an admin key like a full admin password. **A plain Admin key can never touch money** — see Billing below.
- **Admin — Billing** — a **separate, distinct** scope for money mutations (credits, coupons, subscriptions, payouts, funding, refunds). Billing is *not* a toggle on the Admin scope; it is its own grant, so a general Admin key is structurally incapable of moving money. Because a billing key still needs to reach the admin surface, the Billing scope **also requires Admin** — selecting it adds Admin automatically. Grant it only to an integration that must charge the card or change plans.
- **Routing** — the general external API: leads, buyers, partners, campaigns, offers, contracts and reports under `/api/v1/*`, plus the `/api/buyer/*` and `/api/partner/*` portal endpoints. The right scope for most integrations. It does *not* cover posting new leads — `/api/v1/leads/submit`, `/post` and `/ping` require the **posting** scope, which lives on a campaign posting key (see [Posting Keys vs Portal Keys](#posting-portal-keys)), not on a key you mint here.
- **Messaging** — the external messaging API at `/api/v1/messaging` (send SMS/email through your templates and flows from an outside system). The admin messaging pages in the dashboard run on `/api/admin` and need the **Admin** scope instead.
- **Calls** — call tracking at `/api/v1/calls`, plus the telephony, tracking-number and IVR config endpoints. Pushing call results in from an external dialer is a separate **call_data_write** scope on `/api/v1/call-data`.

Routing, Messaging and Calls do **not** grant access to the admin dashboard endpoints — Admin is a separate, explicit grant. Legacy keys created before scopes existed (empty scope = "all apps") are also blocked from the admin surface. **New keys must carry at least one scope** — there is no "leave it empty for everything" shortcut, because an empty scope list reads as "every non-admin app" at request time and made it possible to create a key named for admin access that then failed on every admin route.

**Billing is a separate key.** There is no "allow billing" checkbox on a normal Admin key. To hand an integration the ability to move money you mint a **dedicated billing-capable key** carrying both the Admin and Admin — Billing scopes. Any admin key can still *read* billing pages; only a key with the Billing scope can perform money mutations. Every other admin key is blocked from credits, coupons, subscriptions, payouts, and funding mutations. The **Billing** column of the key list shows Allowed (has the Billing scope) / Blocked (Admin only); non-admin keys show —.

**Automatic 90-day expiry.** Admin-scoped keys are full tenant-console credentials, so if you don't set an expiry they **expire automatically 90 days** after creation. Set an explicit expiry to make it shorter or longer; that value is always respected. Non-admin keys (Routing / Messaging / Calls) are unchanged — they never expire unless you give them a date.

**Always off-limits to keys.** Even an Admin key cannot impersonate another user or mint/revoke API keys — those stay session-only. Admin keys are also rate limited to **300 requests per minute** per key.

**Creating a key.** Click **Create Key**, name it, choose **Admin (full dashboard)** (plus any other scopes). For a money-capable key, also check **Allow billing operations** in the Admin warning box — that adds the separate Admin — Billing scope (and Admin if it isn't already selected). Set an expiry if you want one other than the 90-day default, and **Create**. The full key is shown **once** — copy it immediately; it is never displayed again. Revoke a key any time from the list; revocation takes effect immediately.

### Feature Modules {#settings-features}

**Location:** Settings → **Features**

Toggle feature modules for your tenant (Messaging, Call Tracking, AI Dialer, etc.). Chargeable modules show their **per-minute credit rate inline** next to the toggle — e.g. Calls at 10 credits/min — so the cost is visible before you commit. Flipping a chargeable module **on** opens a confirmation modal restating the rate; nothing is enabled (or billed) until you confirm. A payment method or credit balance is still required — enabling without one returns a billing prompt. Rates and credit mechanics: [Tenant Plan, Credits & Auto-Recharge](#tenant-plan-credits).

### Buyer Settings
Configure buyer dashboard:
- Dashboard access controls
- Feature visibility
- Custom branding

### Partner Settings
Configure partner portal:
- Partner portal access controls
- Feature visibility
- Custom branding

### Notifications {#notifications}

**Location:** [/admin/settings/notifications](/admin/settings/notifications)

Subscribe to event-driven notifications on a per-trigger basis. Each trigger represents a specific system event (e.g., a test lead being submitted). For each trigger you can independently enable **In-App** notifications (bell icon in the top bar) and **Email** notifications.

**Managing your subscriptions.** The notifications settings page lists every available trigger grouped by category. Toggles show your effective state — if you haven't set a personal preference, the row shows the role-level default (labeled "Default"). Flipping a toggle creates a personal override. Click **Reset to default** to remove your override and fall back to whatever your admin configured for your role.

**Role defaults** (admin only). At [/admin/settings/notifications/role-defaults](/admin/settings/notifications/role-defaults), admins configure the baseline notification channels for each role (admin, buyer, partner). These defaults apply to every user in that role who hasn't set a personal override. Changes cascade immediately — users who relied on the old default see the new one on their next page load.

**Current triggers:**

| Trigger | Fires when | Default subscribers | Default channels |
|---|---|---|---|
| `lead.test_submitted` | A partner posts a test lead (name contains "test", `isTest=true` API flag, or campaign in `testing` status) | Members of the posting partner | In-app |
| `lead.validation_failed` | A non-test lead post is rejected with `outcome=validation_error` or `outcome=auth_error` (and the API key is valid, so a partner is known) — covers Zod failures, missing TCPA consent, bad TrustedForm/Jornaya, suppressed email/phone, no-scope and read-only API keys | Members of the posting partner | Email + in-app |

Additional triggers (delivery failures, low balance, DSR submitted, etc.) will reuse this same infrastructure as they're added.

**Partner self-serve.** Partner-portal users can manage their own subscriptions at [/partner/settings/notifications](/partner/settings/notifications) — accessed via the **Notifications** link on their main settings page. They see only the partner-scoped triggers above. Admins can manage subscriptions on behalf of any partner-portal user from the Admin → Partner detail → **Notifications** tab; the route enforces that the target user is a member of that partner before saving.

**Bot-scan exclusion.** `lead.validation_failed` fires only when the API key is resolved successfully. Missing or invalid API keys do NOT fire — we don't know which partner to notify, and bombing partners on internet bot scans would be noise.

**Test leads excluded by construction.** This trigger lives on the rejection path; test leads land on the success path with `lead.isTest=true` and never enter the rejection branches.

---

## Users {#users}

**Location:** [/admin/users](/admin/users)

Manage admin, buyer, and partner user accounts. Every person who logs into the system needs a user record.

### User List

Browse all users with search and status filter (Active / Inactive / All). Click a user name to view or edit their profile.

| Column | Description |
|--------|-------------|
| Name | First and last name (links to detail) |
| Email | Login email address |
| Role | `admin`, `buyer`, or `partner` |
| Mgmt | Color-coded dots: blue = Partner Manager, amber = Account Manager, green = Sales Rep |
| Job Title | Optional job title |
| Status | `active` or `inactive` |

### Inviting a User (Magic Link)

<!-- react-flow:invitation-flow -->

Click **Invite User** to send a magic-link invitation. Admins do not set passwords — the invitee clicks the emailed link and sets their own password on accept.

| Field | Description |
|-------|-------------|
| Email | Login email (auto-lowercased, single invite per email per tenant) |
| Role | `admin` for tenant admins; for partner/buyer portal users, invites are sent from the partner/buyer detail page |
| Permissions | Accounts / Users / Billing / Offers / Reports — bound at invitation time; cannot be altered between invite and accept |

The invitee receives an email with a 7-day single-use link. On click, they land on `/accept-invite` with the full context (who invited them, to what organization, as what role, with which permissions). Existing users sign in and click **Accept**; new users fill a signup form with email pre-filled. If the invited role is admin or carries **Users** permission, the invitee must enroll 2FA before first login completes.

Pending invitations appear in a **Pending Invitations** section alongside Active Members on the partner/buyer Users tab, with **Resend**, **Revoke**, and **Copy link** actions. Resending a pending invite bumps the expiry and sends a fresh email; no duplicate rows are created.

One email = one user = one password across all tenants. Adding the same person to multiple tenants adds multiple `member` rows — the user signs in once and sees every tenant they belong to.

### Management Flags

Users can be assigned management roles that control visibility and assignment throughout the system:
- **Partner Manager** — can be assigned to partners
- **Account Manager** — can be assigned to buyers
- **Sales Rep** — can be assigned to contracts
- **Owner** — full account ownership (shown as a purple badge)

### Access Controls {#access-controls}

The user edit page has a consolidated **Access Controls** section with two toggles that govern what a user can see and do platform-wide.

**PII Access** — unmasks PII (`firstName`, `lastName`, `email`, `phone`, address, `dateOfBirth`, `zip`, custom PII-flagged fields) on reads. Users without it see masked values everywhere. Also gates the ability to mint PII-access API keys — see [PII-Access API Keys](#pii-access-api-keys).

**Read-only** — the user's session returns `403 { "error": { "code": "READ_ONLY" } }` on any `POST` / `PUT` / `PATCH` / `DELETE`. Reads (`GET` / `HEAD` / `OPTIONS`) work normally. A small self-service allowlist bypasses the guard so read-only users can still manage their own account: log out, change their password, enroll / manage 2FA, and create or revoke their own API keys (any key they mint is forced read-only).

### Read-Only Users {#readonly-users}

To flip a user to read-only: open their edit page, scroll to **Access Controls**, tick **Read-only (no writes)**, and click **Save Changes**. A confirmation modal surfaces the cascade — flipping to `true` immediately revokes the user's active BetterAuth sessions (count audit-logged) so the next page they hit kicks them back to login with the new flag in place. Flipping back to `false` takes effect on the next request with no cascade.

**Known limitation:** the session revocation kills BetterAuth DB sessions, but legacy `lr_session` JWT cookies expire on their own TTL — a user mid-session on that auth path may retain write access until the cookie lapses. For immediate hard lockout, set **Status = `suspended`** in addition to read-only.

**Bypasses** — superadmins, tenant owners, and active impersonation sessions always bypass the read-only guard by design. An admin impersonating a read-only user can still make changes as that user; the guard is meant to prevent the read-only identity from writing, not to lock out operators debugging on their behalf.

Use read-only for auditors, analysts, BI / reporting employees, or any staff member who needs platform visibility without write authority. Combine with a read-only + PII-access API key for the same user if they also run scripted extracts.

---

## Contacts {#contacts}

**Location:** [/admin/contacts](/admin/contacts)

Contacts represent individual people at buyer or partner organizations who may have portal access. Unlike Users (admin accounts), contacts are tied to a specific buyer or partner entity and can log into the buyer or partner portal with scoped permissions.

### When to Use Contacts vs Users

- **Users** are internal admin accounts with full system access based on their role.
- **Contacts** are external people at buyer/partner organizations who need portal access with limited permissions.

### Contact List

Filter by entity type (Buyers / Partners / All) using the dropdown above the table.

| Column | Description |
|--------|-------------|
| Name | First and last name (links to detail page) |
| Email | Contact's email address |
| Entity | Truncated UUID of the associated buyer or partner |
| Role | `buyer` or `partner` — which entity type this contact belongs to |
| Portal Access | Whether this contact can log into the portal |
| Status | `active` or `inactive` |

### Creating a Contact

Click **Add Contact** to open the creation form. Required fields:

| Field | Description |
|-------|-------------|
| Entity Type | `buyer` or `partner` |
| Entity ID | UUID of the buyer or partner organization |
| First/Last Name | Contact's display name |
| Email | Login email (auto-lowercased) |
| Job Title | Optional |
| Phone | Optional, formatted as (XXX) XXX-XXXX |
| Portal Access | Toggle to allow portal login |

### Permissions

Each contact has granular portal permissions:
- **Accounts** — view/manage account details
- **Users** — manage other contacts in their org
- **Billing** — view billing and payment info
- **Offers** — view and interact with offers
- **Reports** — access reporting dashboards

### Contact Detail Page

Click a contact name to open the detail page with three tabs:

- **Home** — edit contact info, phone numbers, portal access, and permissions (each permission is an on/off toggle). A **Save Changes** bar appears at the bottom only once you have unsaved edits and saves just the fields you changed; **Cancel** reverts them. Actions include **Login As Contact** (impersonation).
- **Login Activity** — table of login attempts showing browser, success/failure, IP address, and timestamp.
- **Activity** — audit log of all changes made to or by this contact.

### Reactivating existing users after the Phase 5.5 member migration {#contacts-reactivation}

After the Phase 5.5 member migration, existing users without a portal-access `member` row in the current tenant no longer appear on the Contacts list. Admins can bulk-invite them back through the new magic-link flow — each invite produces a time-limited email link that activates the user's portal access on accept.

**Why it's needed**
- The Contacts list now INNER JOINs `member` — only users with an active portal-access membership in the current tenant show up.
- Legacy portal users without that membership need a fresh magic-link invite to reconnect them to a buyer or partner entity.

**Bulk "Send magic link"**
1. Go to **Admin → People → Contacts**.
2. Select the rows you want to reactivate using the row checkboxes.
3. Click **Send magic link** in the action bar.
4. In the modal, pick the buyer or partner entity, the app role, and the permissions to grant.
5. Click **Send**.
6. Review the result drawer — any failed rows have a **Retry** button so you can fix and resend without re-selecting.

**Per-row send**
- Click the envelope icon next to any contact row to open the same modal pre-filled for that single contact.

**What happens on the user's side**
- They receive an email with a time-limited link.
- Clicking the link walks them through setting a password.
- On accept, their account `status` flips to `active` automatically — no admin follow-up needed.

---

## Block List {#block-list}

**Location:** [/admin/block-list](/admin/block-list)

Block traffic from specific referrer URLs, IP address ranges, or caller IDs. Blocked entries are rejected during lead submission and call routing before any processing occurs.

### Tabs

The page has three tabs:

**Blocked Referrers** — Block leads from specific referring URLs. Use when a partner sends traffic from unauthorized sources.

| Column | Description |
|--------|-------------|
| Referrer | The blocked URL pattern |
| Reason | Why it was blocked |
| Created | Date added |

**Blocked IP Ranges** — Block leads submitted from specific IP addresses. Specify a From IP and To IP to define the range (use the same IP for both to block a single address).

| Column | Description |
|--------|-------------|
| From IP | Start of blocked range |
| To IP | End of blocked range |
| Reason | Why it was blocked |
| Created | Date added |

**Blocked Caller IDs** — Block calls from specific phone numbers.

| Column | Description |
|--------|-------------|
| Caller ID | The blocked phone number |
| Reason | Why it was blocked |
| Created | Date added |

### Adding & Removing Entries

Click **Add Entry** to show the add form for the active tab. Fill in the required fields and click **Add**. To remove a block, click **Delete** on the entry's row.

---

## Suppression List {#suppression-list}

**Location:** [/admin/suppression](/admin/suppression)

The suppression list prevents leads or calls from being processed for specific email addresses or phone numbers. Unlike the block list (which blocks by source), suppression blocks by the contact's identity — useful for DNC (Do Not Contact) compliance, opt-outs, or known-bad contacts.

### Adding a Suppression Entry

Use the inline form at the top of the page:

| Field | Options | Description |
|-------|---------|-------------|
| Type | Email, Phone | What identifier to suppress |
| Value | (text input) | The email or phone to suppress |
| Scope | Global, Buyer, Vertical | How broadly this suppression applies |

Click **Add** to create the entry. The value is hashed for privacy — only a truncated hash is shown in the table.

### Suppression Table

| Column | Description |
|--------|-------------|
| Type | `email` or `phone` |
| Hash | Truncated hash of the suppressed value (original not stored in plaintext) |
| Scope | `global`, `buyer`, or `vertical` |
| Reason | Why the entry was added |
| Expires | Expiration date, or "Never" for permanent suppression |
| Created | Date added |

**Note:** Suppressed entries are checked during lead routing. A suppressed email/phone will prevent the lead from being sold to buyers within the configured scope.

---

## System Lists {#system-lists}

**Location:** [/admin/lists](/admin/lists)

System lists manage the predefined options used in dropdowns and reason fields throughout the system. Use this page to customize the available choices for dispositions, rejection reasons, media types, and other categorizations.

### Navigation

The page has a sidebar listing all list types and a main panel showing items in the selected list. Click a list type on the left to view its items.

### Available List Types

| List Type | Purpose |
|-----------|---------|
| Profanity Matches | Exact-match profanity words checked during lead validation |
| Profanity Contains | Substring profanity patterns checked during lead validation |
| Lead Return Reasons | Reasons a buyer can return a lead |
| Call Return Reasons | Reasons a buyer can return a call |
| Application Reject Reasons | Reasons for rejecting a partner application |
| Campaign Reject Reasons | Reasons for rejecting a campaign submission |
| Lead Accepted Reasons | Reasons for accepting a lead |
| Lead Rejected Reasons | Reasons for rejecting a lead during routing |
| Conversion Approved Reasons | Reasons for approving a conversion/disposition |
| Conversion Rejected Reasons | Reasons for rejecting a conversion/disposition |
| Media Types | Traffic source categories (e.g., Search, Social, Display) |

### Managing List Items

- **Add:** Type a name in the input field and click **Add** (or press Enter). Items are appended to the end of the list.
- **Delete:** Click **Delete** on any item's row. A confirmation prompt appears before removal.

**Note:** Removing a list item does not retroactively change records that already reference it. Existing leads, calls, or dispositions retain their original values.

---

## Webhooks {#webhooks}

**Location:** [/admin/webhooks](/admin/webhooks)

Webhooks send real-time HTTP notifications to external systems when events occur in the platform. Use them to sync data with CRMs, trigger automations, or notify external services.

**Webhook scopes** — webhooks can be configured at four levels:

| Scope | Where to configure | Fires for |
|-------|--------------------|-----------|
| Global | `/admin/webhooks` | All leads in the tenant |
| Partner | Partner detail → Webhooks tab | Leads from that partner |
| Offer | Offer detail → Webhooks tab | Leads on that offer |
| Campaign | Campaign detail → Webhooks tab | Leads from that specific campaign |

Campaign-scope is the most precise — use it when only one campaign needs a postback (for example a single Google Ads conversion action). Admin-configured campaign webhooks can use every delivery method. Partner portal campaign webhooks are limited to `GET` and `POST`. Partner- and campaign-scoped webhooks receive a partner-safe token whitelist (no buyer/contract data).

### CRM entity sync (auto-populate CRM ID)

Beyond lead/call events, a global webhook can subscribe to **entity-creation events** — `buyer.created`, `partner.created`, `vertical.created`, `offer.created` — to keep an external CRM in step with the entities you set up. The intended use is auto-populating each entity's **CRM ID**: point a global webhook at an automation endpoint (e.g. an n8n workflow) that find-or-creates the matching CRM record and writes its id back to the entity via `PATCH /api/v1/<entity>/<id>` with `{ "crmId": "..." }`.

- The event fires **once, on creation**, only when the entity's CRM ID is still empty (so re-saving never re-fires), and carries the entity's key fields (name, status, and for offers the linked vertical's name + CRM ID).
- **"Sync to Zoho" button** — on the buyer/partner/vertical/offer detail page, in the collapsed **Legacy & CRM** section, a **Sync to Zoho** button lets ops trigger the same sync for an existing entity (for records created before the webhook was wired, or that never linked). It's disabled once the entity has a CRM ID. *This button is only shown for the tenant whose CRM the sync targets.*

Nothing happens until you create the global webhook subscription for these events — the events fire into it, and the linked CRM ID appears on the entity once the automation writes it back.

### Webhook List

| Column | Description |
|--------|-------------|
| URL | The endpoint receiving notifications |
| Events | Number of events this webhook listens to |
| Status | `active` or `inactive` |
| Failures | Consecutive failure count |
| Last Delivered | Timestamp of last successful delivery |

### Creating a Webhook

Click **Add Webhook**. Key fields:

| Field | Description |
|-------|-------------|
| Name | Optional display name |
| Delivery Type | `POST`, `GET`, or `Email` |
| URL | The endpoint URL to receive notifications. Type `{` for tokens, `\|\|` after a `{token` to chain transformers (e.g. `{emailAddress\|\|lower_case\|\|encode:sha256}` for FB CAPI). See [Field Transformers](#delivery-field-transformers) for the full transformer list. |
| Event | Primary event trigger (required) |
| Description | Optional description |
| Interpret Using | How to validate the response (e.g., "Status Code") |
| Match Response | Expected response value for success validation |
| Timeout | Response timeout in seconds (default: 30) |
| Max Retries | Number of retry attempts on failure (default: 3) |
| Body Format | POST body format — see below |

**JSON body (POST webhooks).** A POST webhook defaults to **Form-encoded**: parameters go in the URL query string and no body is sent at all. Switch **Body Format** to **JSON** to send a token-interpolated JSON body (`Content-Type: application/json`) — use it for endpoints (n8n, Zoho, and similar) that expect a JSON payload. Type `{` to insert `{Token_Name}` placeholders; they are resolved and JSON-escaped at delivery time. A JSON body is required when this is set. Available on every webhook editor: **Global** (`/admin/webhooks`), the admin **Partner**, **Campaign**, and **Offer** detail pages, and the **partner portal** (Webhooks + Offer pages). Two body-editor authoring aids — **AI Token** (maps a pasted buyer JSON to your tokens) and **Generate Starter JSON** — are shown on the **admin** editors only; the partner portal keeps the **Insert Token** picker (partner-safe tokens) but not those two. Partner/campaign-scoped bodies are token-filtered at delivery, so a partner can never emit buyer/internal data.

**Computed `{Lead_Age}` token** — webhooks expose a computed `Lead_Age` token: the lead's age in whole years, derived from the lead's date of birth on the calendar date the webhook fires. Use it in URL templates (`{Lead_Age}` or `{{Lead_Age}}`) or in webhook filters (e.g. `Lead_Age greater_than 44`) — including partner- and campaign-scoped webhooks. If the lead has no date of birth, or the value is invalid (unparseable, a future date, or over 120 years), the token is omitted entirely and numeric `Lead_Age` filters will not match. The token is intentionally NOT named `Age` — fields named `age`/`Age` are tenant data and pass through to tokens unchanged.

**Available Events:**
- **Leads** — `lead.received`, `lead.created`, `lead.sold`, `lead.rejected`, `lead.dq_distributed`, `lead.returned`, `lead.dispositioned`, `lead.converted`, `lead.sale_adjusted`
- **Calls** — `call.incoming`, `call.dialed`, `call.connected`, `call.duration`, `call.billed`, `call.completed`, `call.converted`, `call.dispositioned`, `call.sale_adjusted`
- **CPA** — `cpa.converted`
- **Entity Sync** (global scope only) — `buyer.created`, `partner.created`, `vertical.created`, `offer.created`

**Real-time call trigger events.** Four call events mirror the call billing triggers and fire as the call progresses, not just at completion:

<!-- react-flow:webhook-call-trigger-events -->

| Event | Fires |
|-------|-------|
| `call.incoming` | Real time, when an inbound call is received (duplicate calls do not fire) |
| `call.dialed` | Real time, each time a buyer leg is dialed — the first dial and every failover dial |
| `call.connected` | Real time, when the buyer answers (fires once per call) |
| `call.duration` | At call completion, when the call meets this webhook's duration threshold (fires once per completed call) |
| `call.billed` | When the call bills — once per billed rule (see [Call Billing](#call-billing)) |

**Duration threshold (per webhook).** Webhooks subscribed to `call.duration` have two optional settings:
- **Min duration (seconds)** — the call must last at least this long for the webhook to fire. Leave empty (`null`) and `call.duration` fires on every completed call.
- **Duration anchor** — where the measurement starts: `incoming` (call received), `dial` (buyer leg dialed), or `connect` (buyer answered). Leave empty and it defaults to `connect` at fire time.

The threshold is evaluated per webhook, so two webhooks on the same campaign can use different thresholds and anchors. Calls below the threshold are logged as skipped deliveries in the webhook log (like filter skips), so you can see why a delivery didn't go out.

**`call.billed` (billing event).** `call.billed` fires off the billable event itself — one fire per billed rule, mirroring the `call.billed` timeline rows. Single billing mode: exactly one fire when the call bills a non-zero charge (no fire on $0 calls). Multiple billing mode (see [Call Billing](#call-billing)): one fire per met pricing rule, including non-winning tiers under the Highest charge policy (marked `{Rule_Charged} = false`). Each fire carries per-rule tokens: `{Rule_ID}`, `{Rule_Name}`, `{Rule_Price}`, `{Rule_Threshold_Sec}`, `{Rule_Anchor}`, `{Rule_Charged}`, and `{Charge_Mode}` (`sum`/`highest`); `{Total_Price}` is always the charged sale total on every fire. `call.duration` is unaffected by billing mode — it always fires once per completed call with no rule tokens. Ad-platform destinations (Google Ads / Facebook CAPI / AXON) upload conversions only from `call.duration`; `call.billed` never triggers them, so per-rule fires can't double-count conversions.

**Call event webhooks (calls → CRM).** Like `lead.sold`, the call events carry a rich token payload so an inbound call can create/maintain a CRM record the same way a lead does. When a call fires (e.g. `call.completed`), the webhook resolves the call's caller and commercial context and exposes:
- **Caller PII** *(present only when the caller has been identified — e.g. a matched or enriched person)*: `{firstName}`, `{lastName}`, `{emailAddress}`, `{phoneHome}` (the caller number — always present), `{state}`, `{city}`, `{zipCode}`.
- **Commercial context:** `{Buyer_ID}`/`{Buyer_Name}`, `{Contract_ID}`/`{Contract_Name}`, `{Offer_ID}`/`{Offer_Name}`, `{Campaign_ID}`/`{Campaign_Name}`, `{Vertical_Name}`, plus the `*_CRM_ID` external references.
- **Finances:** `{CPA_Revenue}` (buyer charge — the retained/converted signal), `{CPA_Payout}` (partner payout — may be `0`), `{Sale_Price}`.
- **Keys & timing:** `{Call_ID}`, `{Call_Sale_ID}` (the stable id to key a CRM record on — present on every call event), `{buyerLeadId}` (intake case id — attaches once the call is dispositioned/retained), `{Call_Created_Date}`/`{Call_Created_Time}`, plus the call-duration tokens (`{Total_Duration}`, `{Billable_Duration}`, …).
- **DNI attribution:** `{S1}`–`{S10}` sub IDs, click IDs (`{Gclid}`, `{Gbraid}`, `{Wbraid}`, `{Fbclid}`, `{Msclkid}`, `{Ttclid}`, `{Twclid}`, generic `{Click_ID}`), `{Utm_Source}`/`{Utm_Medium}`/`{Utm_Campaign}`/`{Utm_Term}`/`{Utm_Content}`, and `{Visitor_ID}` (from the DNI impression) — resolved from the landing-page params the DNI script captured for this call. Usable in URL/body templates and webhook filters. The attribution-only names (`{Wbraid}`, `{Msclkid}`, `{Ttclid}`, `{Twclid}`, `{Click_ID}`, the `{Utm_*}` set, `{Visitor_ID}`) are **admin-scoped only** — stripped from partner- and campaign-scoped deliveries and blocked in scoped filters. (`{S1}`–`{S10}`, `{Gclid}`, `{Gbraid}`, `{Fbclid}` were already partner-safe from the lead path and stay deliverable.) Empty string when the call has no captured attribution.

Token names match the lead tokens, so a webhook body built for `lead.sold` can be reused for calls with minimal changes. Tokens with no value for a given call render as empty strings.

**Note:** Failed webhooks increment the failure counter. Monitor the Failures column to identify endpoints that need attention.

### Webhook Log

**Location:** [/admin/webhook-log](/admin/webhook-log)

Every webhook delivery attempt (including filter/threshold skips) is logged. The **Lead / Call** column links each delivery to its source record — lead deliveries link to the lead, call-event deliveries link to the call (`/admin/calls/{id}`, shown by call display id). Click a row for the full request/response detail; the detail view carries the same call link. The list API supports a `?callId=` filter to pull all deliveries for one call, and the CSV export includes the `callId` column.

### Google Ads Conversion Postback

The platform can upload offline conversions directly to Google Ads when leads sell. This lets Google's bidding algorithm optimize for actual qualified leads rather than form submissions.

<!-- react-flow:google-ads-postback -->

**How it works:**

1. A user clicks a Google Ads campaign and lands on your landing page. Google adds a `gclid` parameter to the URL.
2. When the user submits a form, your landing page passes the `gclid` along with the lead data.
3. The lead routing system stores the gclid as a first-class field on the lead.
4. When the lead sells (or any event you choose), the configured Google Ads webhook fires.
5. The webhook calls Google's API directly with the gclid and conversion value.
6. The conversion appears in Google Ads within minutes, attributed to the original click.

**Setup:**

1. **Connect your Google Ads account** (one-time per tenant):
   - Go to Settings → Integrations → Google Ads
   - Click **Connect Google Ads**
   - Sign in with a Google account that has access to your Google Ads
   - Grant iSCALE permission to upload offline conversions
   - Done — you'll see "✓ Connected as you@example.com"

2. **Create a conversion action in Google Ads** (if you haven't already):
   - Sign in to the sub-account that runs the campaigns
   - Tools → Conversions → New conversion action → Import → Other data sources → Track conversions from clicks
   - Name it (e.g., "Qualified Lead — Rideshare")

3. **Add the webhook in the admin UI:**
   - Webhooks → Add Webhook
   - Method: Google Ads
   - Customer Account: pick from the dropdown (populated from your Google Ads accounts)
   - Conversion Action: pick from the dropdown (populated after account selection)
   - GCLID Token: `Gclid` (default)
   - Conversion Value: `Payout` (or a static value like `25.00`)
   - Trigger Event: typically `lead.sold` or `lead.converted`

**Limitations:**

- Only leads with a Google Click ID (gclid) fire conversions. Organic traffic is automatically skipped.
- iOS privacy-restricted clicks (gbraid only) are not yet supported. We store gbraid for future use.
- Conversions older than 90 days (gclid expiration) are rejected by Google.
- Test fire button uses Google's validation endpoint and does NOT record real conversions.

**Debugging:**

The Webhook Deliveries view shows the exact request body sent to Google for each fire, plus the response. If a conversion fails, expand the delivery row to see the error.

### Facebook Conversions API (CAPI) {#webhooks-facebook-capi}

Facebook CAPI sends server-side conversion events to Meta for campaign optimization and attribution. Unlike browser-based pixels, CAPI works even when cookies are blocked.

**Setup:**

1. Go to **Integrations → Media Buying** and click **Configure CAPI** on the Meta card
2. Enter your **Pixel ID** (from Meta Events Manager → Data Sources)
3. Enter your **System User Access Token** (from Meta Business Manager → System Users)
4. Optionally add a **Test Event Code** from Facebook's Test Events tool
5. Click **Test Connection** to verify, then **Save**

**Creating a Facebook CAPI Webhook:**

1. Go to any webhook manager (global, partner, offer, or campaign scope)
2. Click **Create Webhook** and select **Facebook CAPI** as the method
3. Choose an **Event Name** (Lead, Purchase, CompleteRegistration, etc.)
4. Configure the **Conversion Value** token (default: `{CPA_Payout}`)
5. Select events to trigger on (e.g., `lead.sold`, `cpa.converted`)

**How It Works:**

<!-- react-flow:FacebookCapiPostback -->

1. A visitor clicks a Facebook ad → lands on your page (fbclid captured in URL)
2. Partner submits the lead with the fbclid value
3. Lead is routed and sold to a buyer
4. The FACEBOOK_CAPI webhook fires, posting to `graph.facebook.com/v24.0/{pixelId}/events`
5. Facebook receives the conversion with matching click data for attribution

**Match Signals (strongest to weakest):**

- **fbclid** — Facebook Click ID from the ad click URL (`?fbclid=...`)
- **fbc cookie** — `_fbc` browser cookie set by Facebook pixel
- **fbp cookie** — `_fbp` browser cookie (Facebook browser ID)
- **Hashed PII** — SHA-256 hashed email, phone, name, city, state, zip

Enable **Include User Data** to send hashed PII for the best match rates. All PII is hashed with SHA-256 before leaving your server — Facebook never sees raw data.

**Limitations:**

- Conversion events are posted server-side; there is no browser pixel involved
- Test Event Code sends events to Facebook's test tool only — they do not count in real campaigns
- Facebook deduplicates events using the event_id field (lead ID + sale ID)

---

## Integrations {#integrations}

**Location:** [/admin/integrations](/admin/integrations)

Integrations live under one **Integrations** entry in the System menu, with two tabs:

- **Providers** — the catalog of managed third-party connectors (fraud/compliance, enrichment, media buying, billing/CRM, delivery). Each provider stores its own credentials or OAuth connection and is health-tracked.
- **Custom HTTP** — your own tenant-authored outbound HTTP calls, fired when a lead posts to an offer (ping a partner endpoint, an internal CRM, or a legacy webhook). A different data model from managed providers, so it lives as a sibling tab rather than mixed into the same grid.

> The old standalone **Offer Integrations** menu item is gone; its list is now the **Custom HTTP** tab. The old URL `/admin/integrations/list` redirects to `/admin/integrations?tab=custom-http`.

### Providers tab {#integration-providers}

The Providers tab is **status-first**, not category-first. A health strip at the top counts **Connected**, **Needs attention**, **Available**, and **Roadmap** providers. Below it, cards are grouped by health:

- **Needs attention** (pinned to top) — providers whose token expired or config broke (`reconnect_required` / `error`). Each shows the reason and a **Reconnect / Fix** action.
- **Connected** — set-up providers, with a **Configure** action.
- **Available** — not-yet-connected providers, with a **Connect** action.
- **Roadmap** — coming-soon providers, collapsed behind a **Show roadmap** toggle.

A **search box** and **category filter chips** (Fraud & Compliance · Enrichment · Media Buying · Billing & CRM · Delivery) narrow the grid — category is a filter, not the primary axis. Every live card links into that provider's detail page.

**Provider detail** lives at `/admin/integrations/providers/[slug]`. Each detail page uses the shared platform shell:

- A **status strip** (identity, status badge, health line: status · last success · last error) plus provider-specific actions (Test, Reconnect, Disconnect).
- **Capability tabs** that vary by provider — for example IPQualityScore shows **Connection · Checks · Activity**; Meta shows **Connection · Test Events · Activity**; Zoho shows **Connection · Events · Failures · Activity**; Stripe shows **Connection · Platform Fee · Activity**; Google Sheets shows **Connection · Default Recipients · Activity**.
- One dirty-gated **Save Changes** bar for key/secret providers (nothing is saved until you click Save). OAuth providers use their own Connect / Reconnect / Disconnect actions.
- Saved secrets show a **"✓ saved — leave blank to keep"** hint — the field is blank because the stored value is masked; only type a new value to replace it.

### Custom HTTP tab {#integration-custom-http}

Tenant-authored outbound HTTP integrations connect external offer sources (third-party lead buyers, aggregators, etc.) to the routing engine. When a lead is submitted, they fire HTTP requests to external systems as part of routing.

| Column | Description |
|--------|-------------|
| Name | Integration name (links to detail page) |
| Vertical | Associated vertical, or `—` for all |
| Relevant To | Context label for this integration |
| Prefix | Optional prefix for identification |
| Status | `active`, `paused`, or `inactive` |
| Created | Date created |

**Creating one** — Click **Add HTTP Integration**. Key fields:

| Field | Description |
|-------|-------------|
| Name | Descriptive name |
| Vertical | Optional — offers that vertical's fields as tokens in the request template |
| Prefix | Required — namespaces response fields on leads as `prefix_fieldName` |
| Relevant To | Contextual label |
| Status | Active, Paused, or Inactive |
| Notes | Internal notes |

**Where it runs** — a prescreen never runs on its own. Each offer opts in on its **Integrations** tab (the same opt-in model as Blacklist Alliance and IPQS): toggle the integration on under **Custom HTTP Prescreens**. The integration's **Enabled On** tab shows a read-only list of the offers currently opted in.

An **HTTP Configuration** section lets you configure the request URL, method, headers, body template, and field mappings for the external API call. See [Custom HTTP Prescreening](#custom-http-prescreening) below for how a configured integration fires during lead intake, what it does with the response, and how to test it.

### Custom HTTP Prescreening {#custom-http-prescreening}

A Custom HTTP integration is **your own HTTP endpoint, called during lead intake, before routing.** Where the managed enrichment providers (Blacklist Alliance, IPQualityScore) are prebuilt connectors, Custom HTTP lets you call *any* endpoint you control — your own scoring service, a partner's real-time API, an internal CRM check — and fold its answer back into the lead. Every integration you author runs alongside the managed providers in the same enrichment phase, so its results are available everywhere the managed fields are: contract filters, campaign intake filters, and scoring models.

<!-- react-flow:custom-http-prescreen-flow -->

**Creating one** — Go to [/admin/integrations](/admin/integrations) > **Custom HTTP** tab > **Add HTTP Integration**. Beyond the list fields (name, vertical, status) covered above, the HTTP configuration governs what actually gets called:

| Setting | What it does |
|---------|--------------|
| URL | The endpoint to call. Must be `https://` in production and is checked by an SSRF guard — internal, private, and loopback addresses are refused. |
| Method | `POST` or `GET` only. (Other verbs are rejected on save for this surface.) |
| Headers / Auth | Custom request headers, including an `Authorization` header or API-key header for endpoints that require auth. Credential values are encrypted at rest. |
| Body template | For `POST`, a JSON body built from a template with `{{token}}` placeholders that interpolate lead fields (e.g. `{{phone}}`, `{{email}}`, `{{custom.zip}}`). For `GET`, mapped fields are appended as query parameters. |
| Timeout | Per-integration request timeout, **clamped to a 10-second maximum** (5s default). Intake never blocks on a slow endpoint longer than the clamp. |

**Which leads trigger the call** — offer-level opt-in. After creating the integration, open each offer that should run it and toggle it on under **Integrations → Custom HTTP Prescreens**. Offers that haven't opted in never call your endpoint — new offers must be opted in explicitly, exactly like the managed providers.

**The prefix — how response fields land on leads.** Set a **Prefix** and the integration flattens its JSON response into `lead.custom` as `prefix_fieldName` — the same pattern the managed providers use (`ipqs_ip_fraud_score`, `bla_status`). For example, prefix `acme` turns a response `{ "score": 82, "tier": "gold" }` into the lead fields `acme_score` and `acme_tier`. Only primitive values are flattened (nested objects up to four levels deep, capped at 50 keys — when a response has more than 50 fields, shallower fields are kept first). Those fields are then usable in:

- **Contract filters** — buyers accept/reject on `acme_score` like any other field.
- **Campaign intake filters** — reject or accept-without-crediting on the prefixed field during intake.
- **Scoring models** — use the prefixed field as a scoring factor.

A prefix is **required to append fields** — without one, the integration can still accept or reject the lead (via response rules) but writes nothing back. Prefixes are slugified and must be unique per tenant.

**Response rules — accept, reject, or ignore.** In the integration's response mapping you define ordered rules, each matching against the response (a `/regex/` or a case-insensitive substring) and interpreting the match as an outcome:

- **Reject** — the lead is **blocked before routing** with your rule's message as the reject reason. No buyer is billed or posted to. The message is recorded as the lead's disposition (partner-facing reasons are sanitized).
- **Success / accept** — the lead proceeds to routing.
- **Any other outcome, no rule match, or an error** — treated as accept (see fail-open below).

**Fail-open is the default and the safety net.** If your endpoint is **down, times out, returns a non-2xx, or returns non-JSON**, the lead is **never blocked** — it proceeds to routing as if the check hadn't run. A tenant-authored endpoint can never take intake offline; the only way a Custom HTTP integration stops a lead is an explicit **Reject** rule that actually matched. An endpoint that fails several times in a row is also **short-circuited for a few minutes** (a circuit breaker — the Custom HTTP list shows a health badge such as *circuit open* or *erroring*), then automatically retried, so a dead endpoint doesn't add its timeout to every incoming lead.

**Test it before you rely on it.** The integration detail page has a **Test** button that fires the real request (SSRF-guarded, with decrypted credentials) against your endpoint and shows the full request/response plus which response rule matched and how it was interpreted — the same test model used for buyer delivery. Use it to confirm your body template, auth, and rules behave before flipping the integration to **active**.

**Secrets are masked after save.** Once saved, credential fields (passwords, tokens) show a **"✓ saved — leave blank to keep"** hint and never return their real value to the browser. Type a new value only to replace it; leaving the field blank preserves the stored secret.

**Not billed by the platform.** Custom HTTP integrations call *your* vendor or *your* endpoint — the platform does not meter or charge for them (unlike the managed enrichment providers, which bill per enriched lead). Any cost is between you and whatever you're calling.

**Where your variables appear.** Once an integration or managed provider is enabled, its fields — Blacklist Alliance's `bla_*`, IPQualityScore's `ipqs_*`, and each Custom HTTP integration's `{prefix}_*` — become selectable anywhere you pick a lead field, not just in filters. They show up in:

- **Contract custom filters** and **campaign intake filters** — accept/reject on the enriched value.
- **Scoring model factor picker** — as bare keys (e.g. `ipqs_ip_fraud_score`), since scoring reads flattened lead fields.
- **Buyer contract delivery field mapping** and the **HTTP / email token pickers** — reference them as `{{custom.key}}` in a mapped payload or template.
- **Ping configuration** and **intake-center delivery** — the same token form as contract delivery.
- **The Custom HTTP integration editor's own request mapping** — build outbound requests from managed-provider fields.
- **Report dimensions** and **sold-leads CSV export columns** — group and export by an enriched value.

The picker only lists a provider's fields while that provider is **enabled for the tenant** — disable it and its fields disappear from every picker. Configurations you already saved keep working regardless: runtime resolution of `{{custom.key}}` and scoring keys is unconditional, so a delivery template or scoring factor referencing an enrichment field still resolves even if the field is temporarily hidden from the pickers. **SMS delivery templates support enrichment variables via `{{custom.<key>}}`** (e.g. `{{custom.acme_score}}`); top-level lead fields keep the flat `{{field}}` form.

### Integration health {#integration-health}

Connected providers are monitored on every API call. When credentials expire or are revoked the platform marks the provider **Reconnect required** and surfaces a banner on the Providers tab (and next to affected dropdowns) whose **Reconnect / Fix** button links straight to that provider's detail page — so a dead token never silently breaks delivery. The health model applies across all providers; OAuth providers (Facebook CAPI, Google Ads, Google Sheets, Zoho) also drive the cross-app reconnect banner.

**Status meanings**

| Status | What it means |
|--------|---------------|
| `connected` | Last API call succeeded. No banner. |
| `reconnect_required` | Token expired or revoked, or required permissions are missing. Only a re-authorization recovers. Pixel / Ad Account / Customer dropdowns skip their fetch and show a reconnect CTA instead of spinning. |
| `error` | Configuration mismatch — for example a Pixel ID that is not visible to the connected ad account (FB code 803). The token is fine, but this specific call is doomed. Fix the config in the webhook and the next successful fire clears the state. |
| `disconnected` | Admin manually disabled the integration. Treated like `reconnect_required` for short-circuit purposes. |

**What triggers `reconnect_required`**

- Facebook token expired (OAuthException `code=190 subcode=463`).
- Facebook password changed (`190` / `459` / `460`).
- Permissions revoked or missing `ads_management` / `business_management` scope (`190` / `458` / top-level `200`).
- Facebook session expired (top-level `102`).
- Google `invalid_grant` (refresh token revoked or expired) on token refresh.
- Google `invalid_token` (HTTP 401) on any Ads API call.
- Google PERMISSION_DENIED (HTTP 403) when scopes are missing.

**What triggers `error`**

- Facebook `code=803` — the configured Pixel is not visible to the connected ad account. Fix the pixel selection on the webhook.

**How to recover**

1. Click **Reconnect Facebook** or **Reconnect Google** in the banner (or in the dropdown's empty state).
2. Re-authorize the OAuth flow.
3. The first successful API call after reconnect automatically clears the broken state and the banner disappears — no manual "mark connected" step.

**Webhooks are not auto-paused while you reconnect.** When a webhook fires against a broken integration, the delivery is logged with `error_code = integration_reconnect_required` and the webhook's `failureCount` is left untouched. This keeps the webhook armed so deliveries resume the moment you reconnect — you don't have to find every webhook that touched the dead token and un-pause it by hand.

**Read the status programmatically:** `GET /api/admin/integrations/status` returns `{ data: { [provider]: { status, statusReason, statusCode, lastErrorAt, lastSuccessAt, revokedAt } } }` scoped to the calling tenant.

---

## Lead Enrichment Integrations {#lead-enrichment-integrations}

**Location:** [/admin/settings](/admin/settings) > **3rd Party**

Lead enrichment providers run **before routing** and inject enrichment fields into `lead.custom`. Those fields are then available to every contract filter in the builder, so buyers can accept/reject leads based on the enrichment result. Providers run in parallel — total latency is bounded by the slowest provider, not the sum.

Three providers are currently supported: **Blacklist Alliance** (phone DNC/litigator lookup), **IPQualityScore** (IP / email / phone fraud scoring), and **VeracityHub** (soft credit pull / debt qualification).

### Blacklist Alliance

Pre-routing phone lookup against the Blacklist Alliance DNC + litigator database.

**Setup:**
1. Go to [/admin/settings](/admin/settings) > **3rd Party** > **Blacklist Alliance**
2. Toggle **Enabled** on
3. Enter your **API Key** and **Account**
4. Select **API Version** (`v1` or `v3`)
5. Optionally enable **Auto-Reject Blacklisted** to reject leads before routing when flagged
6. Save

**What it does:** On every incoming lead, calls `GET https://api.blacklistalliance.net/lookup` with the lead phone (5s timeout). The response is injected into `lead.custom`.

**Enrichment fields:**

| Field | Description |
|-------|-------------|
| `bla_status` | Lookup result — `Good`, `Blacklisted`, `Suppressed`, `FederalDNC`, `StateDNC` |
| `bla_wireless` | `true` if phone is a wireless/mobile number |
| `bla_carrier` | Carrier name (e.g., `Verizon Wireless`) |
| `bla_carrierState` | State associated with the carrier |

**Auto-reject:** When enabled, leads with `bla_status` of `Blacklisted`, `Suppressed`, `FederalDNC`, or `StateDNC` are rejected **before routing** — no buyers are billed or posted to.

### IPQualityScore

Pre-routing IP, email, and phone validation via IPQualityScore. Each of the three checks is independently togglable — enable only the ones you need.

**Setup:**
1. Go to [/admin/settings](/admin/settings) > **3rd Party** > **IPQualityScore**
2. Toggle **Enabled** on
3. Enter your **API Key**
4. Set the **Fraud Score Threshold** (default `85`) — leads above this score from any enabled check are auto-rejected
5. Toggle **Auto-Reject High Risk** on/off
6. Toggle individual checks: **Check IP**, **Check Email**, **Check Phone**
7. Save

**What it does:** Runs the enabled IPQS checks in parallel (IP proxy/VPN detection, email validation, phone validation). Each result is flattened into `lead.custom` with a prefix (`ipqs_ip_`, `ipqs_email_`, `ipqs_phone_`).

**Common enrichment fields:**

| Prefix | Example fields |
|--------|----------------|
| `ipqs_ip_*` | `ipqs_ip_fraud_score`, `ipqs_ip_proxy`, `ipqs_ip_vpn`, `ipqs_ip_tor`, `ipqs_ip_bot_status`, `ipqs_ip_country_code`, `ipqs_ip_is_crawler`, `ipqs_ip_recent_abuse` |
| `ipqs_email_*` | `ipqs_email_fraud_score`, `ipqs_email_valid`, `ipqs_email_disposable`, `ipqs_email_deliverability`, `ipqs_email_recent_abuse`, `ipqs_email_dns_valid`, `ipqs_email_smtp_score` |
| `ipqs_phone_*` | `ipqs_phone_fraud_score`, `ipqs_phone_valid`, `ipqs_phone_active`, `ipqs_phone_line_type`, `ipqs_phone_carrier`, `ipqs_phone_country`, `ipqs_phone_risky`, `ipqs_phone_recent_abuse` |

**Auto-reject:** When **Auto-Reject High Risk** is on, a lead is rejected if **any** enabled check returns a `fraud_score` above the threshold. Disabled checks are ignored.

### VeracityHub

Pre-routing debt lead qualification via the **VeracityHub Soft Pull v3** API. Each lead gets a user-authorized soft credit inquiry before routing, and the result — identity match, credit-card debt, mortgage and collection balances, credit score — lands on the lead as filterable `veracity_*` fields. A soft pull does not affect the consumer's credit.

**Setup:**
1. Go to [/admin/integrations](/admin/integrations) > **Providers** > **VeracityHub** (Fraud & Compliance) > **Connect**
2. Enter your **API Token** (sent as `X-API-Token` on every query; stored encrypted — after saving, leave the field blank to keep the existing token)
3. Choose a **Consent Language Source** and fill in **Consent Language** — the exact text the consumer accepted authorizing the credit pull. We never generate this text for you.
   - *Use the text below* (default) — the same configured text is sent on every inquiry. Required; until it is set, VeracityHub does not appear as a per-offer toggle and no soft pull runs.
   - *Use the lead's TCPA disclaimer* — sends that specific consumer's `tcpaDisclaimer` when the partner posted one, falling back to the text below when they didn't. Only ~8% of leads currently carry a disclaimer, so the fallback does most of the work; a lead with neither is skipped (and logged).

   Either way, text longer than 2,000 characters is truncated to VeracityHub's limit.
4. Pick a **Permissible Purpose** (default *Prequalification*) and **Consent Method** (default *Web Form*)
5. On the **Checks** tab, toggle **Auto-Reject No-Match** (default on) and optionally set a **Minimum Credit-Card Debt** and/or **Minimum Score**
6. Save

**What it does:** On every incoming lead through an opted-in offer, POSTs the lead's contact fields (first/last name, email, phone, address, city, state, zip — all eight are required; the check is skipped if any is missing), the lead's date of birth when present, and the compliance fields above to `https://api.veracityhub.io/v3/soft-pull` (7s timeout). The lead's ID is sent as `client_reference_id` so a specific inquiry can be traced on the vendor side. The response is injected into `lead.custom`.

**Enrichment fields:**

| Field | Description |
|-------|-------------|
| `veracity_match` | `success` when the identity matched a credit file, `fail` when it did not |
| `veracity_cc_debt_amount` | Reported credit-card debt — the qualification lever for debt offers |
| `veracity_mortgage_bal_amount` | Reported mortgage balance |
| `veracity_collection_bal_amount` | Reported collections balance |
| `veracity_score` | Credit score |
| `veracity_message` | Vendor message accompanying the result |
| `veracity_request_id` | VeracityHub request ID — quote it to the vendor when tracing a specific inquiry |

**Auto-reject:** When **Auto-Reject No-Match** is on (the default), leads VeracityHub cannot match (`match=fail`) are rejected **before routing** — no buyers are billed or posted to. When a **Minimum Credit-Card Debt** or **Minimum Score** is set, leads below either threshold are also rejected (a value equal to the threshold passes; leave a threshold blank to disable that check). Turning Auto-Reject off stops only the match-based rejection — the thresholds still reject. To record the `veracity_*` fields without rejecting anything, turn Auto-Reject off **and** leave both thresholds blank.

**Fail-open:** If VeracityHub is down, times out, returns a non-2xx, or returns an unexpected response, the lead is **never blocked** — it proceeds to routing as if the check hadn't run (the `veracity_*` fields are simply absent). The same applies when Consent Language is unset. The only way VeracityHub stops a lead is a real vendor response that fails Auto-Reject or a threshold.

> **Upgraded from the v2 pre-ping (Aug 2026):** the retired `/v3/debt-pre-ping` endpoint returned a single qualified/rejected `veracity_status`. That field no longer exists — filter on `veracity_match` and `veracity_cc_debt_amount` instead.

### Required Lead Fields Per Provider {#provider-lead-inputs}

Every built-in integration declares which lead fields it needs to run. A provider whose **required** fields are missing from a lead **silently skips** that lead (it is not rejected — the check simply doesn't run and no enrichment fields are written). These declarations are shown in two places:

- **Provider detail page** ([/admin/integrations](/admin/integrations) > Providers > provider > Connection tab) — a "Lead fields this integration uses" panel with required, optional, and behavior notes.
- **Offer Integrations tab** — a compact "Needs: …" line under each toggle that has a hard requirement.

Fields separated by "or" are alternatives (any one is enough); comma-separated entries are all required.

| Provider | Required lead fields | Optional / behavior |
|----------|----------------------|---------------------|
| VeracityHub | First Name, Last Name, Email, Phone, Address, City, State, Zip Code | All eight are mandatory; phone must contain at least 10 digits. Optional: Date of Birth (sharpens the credit-file match), TCPA Disclaimer (sent as the consent language when that source is selected) |
| Trestle | Phone | Optional: First Name, Last Name, Email, Address, City, State, Zip Code. Real Contact grading also uses the lead name when that toggle is on |
| BriteVerify | Email or Phone | Either check runs alone — email-only leads get email verification, phone-only leads get phone verification |
| TowerData | Email | — |
| FullContact | Email or Phone | Optional: First Name, Last Name |
| ZoomInfo | — | Runs only when the lead has an email or a first + last name — otherwise it skips |
| Experian | Email or Address or Zip Code | — |
| Melissa | — | All eight contact/address fields optional; runs on every lead, but a match practically needs name + address, or a phone/email |
| Versium | — | All eight contact/address fields optional; runs on every lead — match quality depends on how many fields are present |
| IPQualityScore | — | Per-check: the Email check needs an email, the Phone check needs a phone, the IP check uses the visitor IP captured automatically at posting |
| Blacklist Alliance | Phone | — |
| Jornaya LeadiD | Jornaya Lead ID | The LeadiD token (UUID) is posted with the lead by the partner |
| TrustedForm | TrustedForm Cert URL | The cert.trustedform.com certificate URL is posted with the lead by the partner |

### Per-Offer Opt-In (Off by Default)

Third-party integrations (Blacklist Alliance, IPQualityScore and its sub-channels, VeracityHub) are **off by default for every offer**. An offer runs a check only when you explicitly opt it in — nothing fires just because the integration is configured tenant-wide.

**Location:** [/admin/offers/[id]](/admin/offers) > **Integrations** tab

On the offer detail page, the **Integrations** tab lists every integration that is enabled + configured at the tenant level, each with a toggle that starts **off**. Turning a toggle on adds that key to the offer's `enabledIntegrations` allowlist. A check runs only when **both** are true: the key is in the offer's allowlist **and** the integration is enabled + configured tenant-wide. An empty allowlist means no third-party checks run for that offer.

Allowlist keys:

| Key | Effect when opted in |
|-----|--------|
| `blacklist` | Runs Blacklist Alliance for this offer |
| `ipqs` | Enables IPQualityScore for this offer (required parent for the sub-channels below) |
| `ipqs_ip` | Runs the IPQS IP check (only while `ipqs` is also on) |
| `ipqs_email` | Runs the IPQS email check (only while `ipqs` is also on) |
| `ipqs_phone` | Runs the IPQS phone check (only while `ipqs` is also on) |
| `veracityhub` | Runs the VeracityHub soft credit pull for this offer |

The IPQS sub-channels (`ipqs_ip`, `ipqs_email`, `ipqs_phone`) require the parent `ipqs` key: if the parent is off, no IPQS lookup runs regardless of the sub-channel toggles. Each offer's opt-in is independent — enabling a check on one offer never turns it on for another.

### Fields Each Provider Adds {#provider-output-fields}

The mirror of the required-fields declaration above: every built-in integration also declares which fields it **writes onto each lead**. Those names are exactly what becomes selectable in contract/campaign filters, lead scoring, and report dimensions once the integration is enabled — so what you see advertised is what you can actually filter on. Shown in three places:

- **Provider detail page** — a "Fields <provider> adds to each lead" panel listing every field name.
- **Offer Integrations tab** — a compact "Adds: …" line under each toggle (first few names, then "+N more").
- **Filter field picker** — enrichment fields are grouped under a header naming the integration that supplies them.

A declared field only carries a value on leads the provider actually returned data for; a lead the provider skipped simply has no `bla_*` / `veracity_*` / `tre_*` key. Jornaya and TrustedForm write lead columns rather than enrichment fields, so they add none.

### Filtering on Enrichment Fields

Every enabled provider's enrichment fields appear in the contract filter builder dropdown — grouped by integration — so buyers can accept/reject leads based on enrichment results without any custom configuration.

**Location:** [/admin/contracts/[id]](/admin/contracts) > **Filters** tab

Examples:

- **Reject leads on the federal DNC:** filter `bla_status` `not in` `FederalDNC, StateDNC, Blacklisted`
- **Require wireless phones only:** filter `bla_wireless` `equals` `true`
- **Reject high IP fraud score:** filter `ipqs_ip_fraud_score` `less than` `75`
- **Reject disposable emails:** filter `ipqs_email_disposable` `equals` `false`
- **Require valid phone:** filter `ipqs_phone_valid` `equals` `true`
- **Require a matched credit file:** filter `veracity_match` `equals` `success`
- **Require at least $10k in card debt:** filter `veracity_cc_debt_amount` `greater than` `10000`

Fields are typed appropriately in the filter builder — booleans get a true/false toggle, scores get a numeric comparator, and enum fields like `bla_status` get a multi-select of known values.

---

## Data Deletion {#data-deletion}

**Location:** [/admin/data-deletion](/admin/data-deletion)

Track and manage data deletion requests for GDPR, CCPA, and other privacy compliance. When a consumer requests their data be deleted, create a request here to track the process through completion.

### Request List

| Column | Description |
|--------|-------------|
| Type | Type of deletion (e.g., full erasure, partial) |
| Identifier | The value identifying the data subject (email, phone, etc.) |
| Identifier Type | What the identifier represents |
| Status | `pending` (awaiting processing), `processing` (in progress), `completed` (done), `failed` (error occurred) |
| Requested | When the request was created |
| Completed | When processing finished, or `-` if still pending |
| Notes | Additional context about the request |

### Creating a Request

Click **New Request** to submit a data deletion request. Provide the identifier (e.g., email address), identifier type, and any notes. The request starts in `pending` status and moves through `processing` to `completed`.

**Important:** Data deletion is irreversible. Once completed, the associated lead data, person records, and PII are permanently removed from the system. Audit log entries are preserved but with PII redacted.

---

## Buyer Self-Service Onboarding

Anonymous visitors can buy lead packs directly from a tenant's marketing site without admin intervention. The full flow:

```
Marketing landing page (e.g. qualityinsurance.co)
  → click "Get [Tier] Pack"
  → /buyer/signup?tenant=<tenant-slug>&sku=<sku-slug>
  → fill PII form (company, name, email, phone, address, tax ID, terms, TCPA, custom questions)
  → Stripe Checkout (full Stripe-hosted payment with branded line item)
  → magic-link email arrives (15min TTL, IP-bound, single-use)
  → click email link
  → /welcome/setup wizard — pick states + daily lead cap
  → /buyer dashboard — leads start flowing
```

<!-- react-flow:buyer-onboarding -->

The diagram above shows the full path. Note: the wizard's delivery-method picker is multi-select — buyers can enable any combination of SMS, Google Sheets, and Email self-serve. API/webhook delivery is not self-serve; selecting it sends an email to iSCALE admins to configure the post manually.

**What gets created on payment**:
- `buyer.status = 'pending_payment'` flips to `'active'`
- New `buyerSku` row with `leadsRemaining = leadCount + bonusLeads`
- New paused `contract` bound to the SKU's vertical (no offer link yet)
- Magic-link token issued (15min TTL)
- Slack notification fires to `SLACK_WEBHOOK_URL` if configured: "New buyer needs an Offer link"
- Audit log entry: `notification_pending` with `kind: 'needs_offer_link'`

**Ad-ops responsibility**: open Admin → Buyers → Contracts → filter `status=paused` → find the new contract (Offers column will be empty) → click in → link to an active Offer. Until linked, the buyer's wizard shows "We're getting your account ready" placeholder.

**SKU capability flags** (added in Phase 2):
- `filterAge` — whether the wizard exposes the age-range filter
- `filterCreative` — whether the wizard exposes sub2/sub3 source filters
- `dayparting` — whether the wizard exposes hours-of-operation
- `crmIntegration` — whether the wizard exposes a CRM webhook delivery option

State filtering and Google Sheets delivery are always available regardless of SKU. The 4 capability flags appear as toggles on the SKU detail page (Edit tab → Capabilities section).

**Cleanup cron**: `/api/cron/abandon-pending-payment-buyers` runs hourly. Buyers in `pending_payment` for 24+ hours flip to `abandoned`. If a delayed Stripe webhook arrives later, the webhook handler revives `abandoned` → `active` (race-at-cleanup-boundary handling).

**Demo + replication docs**:
- [Demo walkthrough](./buyer-onboarding-demo.md) — narrated end-to-end with Stripe-refund cleanup steps
- [Tenant setup guide](./tenant-self-service-checkout-setup.md) — step-by-step for a new tenant to wire up their own self-service funnel

## Onboarding Wizard Builder

### Overview

The Onboarding Wizard Builder replaces the hardcoded `/welcome/setup` flow (states → cap → delivery) with a configurable wizard composed of typed step blocks. Admins build wizards once and attach them to offers; the buyer runtime hydrates each buyer's wizard from `offer.wizardId` at magic-link click time, falling back to the tenant's default wizard when the offer has no override.

Why it exists: every tenant's onboarding flow drifted from the hardcoded one (different states question phrasing, additional consent gates, custom intake questions). The builder makes those variations configurable instead of code changes.

<!-- react-flow:onboarding-wizard-builder -->

### Building a wizard (admin)

**Location:** [/admin/wizards](/admin/wizards)

Click **+ New Wizard** to open the create modal: enter a **Name** (auto-kebab-cased into the **Slug** field), and toggle **Default for tenant** if this should be the fallback when an offer has no `wizardId`.

The wizard editor has two tabs — **Steps** (the builder) and **Options** (vertical, contract options, geo/custom filters) — with **one floating save bar**: it appears whenever anything differs from the last saved version and saves the whole wizard document (name, slug, options, and the full step list) in one atomic write. Cancel is implicit — reload discards; Publish/Archive/Clone stay separate lifecycle buttons in the header.

The Steps tab is a 3-pane layout:
- **Left pane — Step palette.** Drag (or click) to add a step of any of the 5 supported types.
- **Middle pane — Ordered step list.** Drag-reorder steps; click a step to edit it on the right.
- **Right pane — Per-step config form.** Fields specific to the selected step type, plus a Live Preview pane that renders exactly what the buyer will see.

#### The 5 step types

| Type | What it does |
|---|---|
| `statePicker` | Multi-select US states. On `/wizard/complete`, writes the selected states to `contract.geoFilters.states`. |
| `integerCap` | Daily lead cap. Writes the entered integer to `contract.dailyLeadCap`. |
| `deliveryPicker` | Buyer picks one or more delivery channels (SMS, Google Sheets, Email, API/Webhook). Each selected channel inserts a `deliveryMethod` row at completion — except `apiWebhook`, which has no self-serve setup and instead routes a notification to the admin email for manual configuration. |
| `customQuestion` | Non-PII config questions. Supported answer formats: text, number, boolean, date, list, email, phone, url, textarea. Answers are stored in `buyer.customAnswers` keyed by the step ID and surfaced in admin via the `CustomAnswersDisplay` component. |
| `termsAccept` | Renders a markdown body and a checkbox the buyer must tick to proceed. Records the buyer's `ipAddress` at completion for audit. |

#### Status flow

`draft` → `active` (via **Publish**) → `archived`. Both `draft` and `active` wizards are editable in place. Saving an active wizard bumps `version` so new buyers' magic-link snapshots are attributable to a distinct revision; in-flight buyers keep finishing on the frozen snapshot from the version they started with (decision 22). `archived` wizards are immutable audit history — clone one to a new draft if you need its content. **Clone to Draft** also remains available on active wizards when you want to stage major changes before going live instead of editing directly.

Each tenant has exactly one `isDefault` wizard at all times (decision 10). The UI prevents archiving the only default — promote another wizard to default first.

Each step's config form includes a **Live Preview** so admins see the buyer-facing rendering as they edit.

### Linking a wizard to an offer

**Location:** Offer detail → **Onboarding Wizard** tab

Use the SearchableSelect to choose any `active` wizard from the tenant. Leaving it as the **Use tenant default** placeholder leaves `offer.wizardId` null, in which case the runtime resolves to the tenant's `isDefault` wizard (decision 10).

Editing a wizard while it is attached to live offers does **not** disturb in-flight buyers (decision 22). Each buyer's snapshot is taken once at magic-link click time, so they always finish on the version they started — admin edits only affect buyers who haven't started yet.

### Buyer experience

After Stripe payment + the magic-link click, the buyer lands on `/welcome/setup`. The wizard renders one step per page from the buyer's snapshot.

- **Back button** preserves answers — buyers can move forward and backward freely without losing inputs.
- **Browser close mid-wizard.** Progress persists server-side via `buyer.wizardProgress` (decision 8). When the buyer returns through the same magic-link path, they resume on the step they left.
- **Final step** posts to `/api/buyer/wizard/complete`, which activates the contract via `assertContractTransition` (decision 11) and redirects to `/buyer`.
- **Session lifetime.** The 15-minute magic link is single-use; once clicked, the runtime issues an `lr_wizard_session` cookie (60-minute sliding window) so buyers never time out mid-wizard (decision 9).

### Custom answer review (admin)

The buyer detail page renders the `CustomAnswersDisplay` component. It resolves answers against the snapshot's labels — so an admin reviewing a buyer sees `How did you hear?: Google` rather than `<step-uuid>: Google` (decision 15).

This component reads from the buyer's snapshot only — it never JOINs to the live `wizard` table (decision 23). That keeps historical answers stable even after the wizard is edited or archived.

### PII guidance

Custom questions are for **non-PII configuration only** (decision 16). The wizard builder UI surfaces a banner reminding admins of this. PII collection on the lead-intake side is handled by the segregated PII vault — never wire a custom question to ask for SSN, DOB, full address, or other regulated identifiers.

### Hardening (2026-05-13)

**Admin-facing changes:**
- **apiWebhook step now notifies admins.** When a buyer completes a wizard that includes an API/Webhook delivery channel, the integrations team gets an in-app notification (and a Sentry breadcrumb) on top of the existing audit row — no more silent "configure this manually" hand-offs.
- **SMS is a first-class delivery method.** Wizard-created SMS delivery rows now write `deliveryMethod.method = 'sms'` directly instead of being stashed under the `'post'` method with a config flag. SMS delivery is queryable in reports and filterable in admin UIs like every other channel.
- **Terms acceptances live in `buyer.acceptedTerms`.** Each accepted terms step writes a structured record (`stepId`, `versionHash`, `acceptedAt`, `ipAddress`, `userAgent`, `bodyHash`) to a first-class `jsonb` column on the buyer — easier to pull for compliance/legal exports than scraping `buyer.customAnswers`. During the 30-day soak, the writer also keeps the legacy fallback populated; the `customAnswers` fallback is scheduled for removal ~2026-06-13.

**Compliance / legal:**
- **Per-acceptance audit trail.** Every terms acceptance carries IP + userAgent + timestamp + body hash, so a future challenge ("did this buyer actually see THIS version of the terms?") is answerable from one column.
- **Session-bound IP for TCPA evidence.** The wizard session cookie now binds the IP captured at magic-link click. The terms-accept writer compares the binding IP against the request IP at accept time and flags any mismatch as a Sentry breadcrumb — surfaces session hijack / shared-link abuse patterns before they pollute the consent record.
- **CSRF guards on all wizard POSTs.** Same-origin enforcement on `/api/buyer/wizard/step/[stepId]` and `/api/buyer/wizard/complete` — third-party origins cannot drive a buyer's wizard.

**Operational:**
- **Magic-link retries are idempotent.** A buyer who completes the wizard then hits a transient error can safely re-click their magic link — the `/complete` endpoint short-circuits on already-completed buyers before the rate limiter increments, so legitimate retries no longer get blocked behind the 5-per-minute cap.
- **Wizard sessions on Vercel previews now use Secure cookies.** Preview deploys (which run on HTTPS) now match production cookie semantics — only local `vercel dev` sees plain cookies.

### Delivery defaults + auto-Sheet (2026-05-13)

**For admins building a wizard:**
- In any `deliveryPicker` step, you can now pre-select a **default SMS template** and a **default Google Sheets layout**. Both selectors are gated on which channels are in `allowed` — adding SMS to `allowed` reveals the SMS template dropdown (lists tenant-scoped + global `smsTemplate` rows); adding Google Sheets reveals the layout dropdown (lists `verticalGoogleSheetsTemplate` rows grouped by vertical). These act as starting points — buyers see them at runtime, can edit the SMS body and phone before submitting; the GS layout is locked-in by the admin pick.
- New wizard-level **"Land contracts in Setup"** toggle (default ON). Completions leave the new contract in `setup` status so admin reviews delivery config, geos, caps, etc., before lead flow starts. Flip OFF to go straight to `active` (e.g. for trusted self-serve verticals). The value is frozen onto each buyer's snapshot at magic-link click, so editing this toggle mid-flow never flips an in-flight buyer's outcome.
- Set your tenant's **abbreviation** (max 8 chars) in tenant settings — used as the prefix in auto-generated sheet titles. Example: with `abbreviation = QI`, a sheet for buyer John Doe on the IUL vertical is named `QI - John Doe - IUL`. Tenants without an abbreviation fall back to the first 2 letters of the tenant name uppercased; a warning is logged so it's easy to backfill.

**For buyers going through the wizard:**
- When you tick **SMS** in the delivery picker, an indented sub-form reveals an editable template body and a phone number. The body pre-fills from the admin's default template (or boilerplate if none was set); the phone pre-fills from whatever you typed into the earlier wizard step that asked for your phone number. Edit either, submit, done. Untick SMS to clear the sub-form; re-ticking restores your last-typed values within the same session. SMS edits are saved to your contract's delivery config only — they don't change the admin's template.
- When you tick **Google Sheets**, a new spreadsheet is auto-created the moment you finish the wizard. You'll receive a Drive invitation as `writer` on the email you used at signup — no setup steps, no service-account dance.

**For operations / support:**
- **Pre-checkout signup now requires Last Name** (was previously optional). New buyers always have both first + last name on `buyer.firstName` / `buyer.lastName`. Legacy buyers missing `lastName` fall back to first-name-only in sheet titles (e.g. `QI - John - IUL`).
- **Google Sheets creation failure does NOT block wizard completion.** If the Sheets/Drive API call throws (rate limit, transient network, quota), the contract is still created and the wizard still succeeds — the GS delivery action is persisted with `enabled: false` and a `creationError` note, and the failure is captured in Sentry. Admin can retry the creation manually from the contract's delivery page.
- **Magic-link replays don't double-create sheets.** If a buyer hits `/complete` after a partial completion or transient error, the writer checks `contract.deliveryConfig` for an existing GS action with a non-empty `spreadsheetId` first — if found, it skips the API call and reuses the existing spreadsheet. Safe to re-click the same magic link as many times as needed.

### Per-channel audience — SMS/Email to Buyer or Lead (2026-05-13)

**For admins building a wizard:**
- In any `deliveryPicker` step, you can now pick — per channel — **who receives the delivery**: the buyer's own contact info, or each lead's contact info.
- **SMS recipient** radio (Buyer | Lead). Default: **Buyer** (back-compat — existing wizards keep their current behavior).
- **Email recipient** radio (Buyer | Lead). Default: **Lead** (most outbound delivery emails go to leads). A new **Email Template** dropdown (next to the SMS Template and Sheets Layout selectors) lists tenant-scoped + global `emailTemplate` rows so you can pre-fill the buyer's subject + body.
- The audience choice drives whether the buyer sees an editable sub-form at runtime, or just an info label.

**For buyers going through the wizard:**
- When admin picks **Buyer recipient**: ticking the channel reveals an editable sub-form. SMS shows **template body** + **phone number**. Email shows **subject** + **body** + **to-email** — pre-filled from the admin's email template, then from your earlier email-question answer in the same wizard, then your account email as a final fallback. Edits are saved to your contract's delivery config only — they don't change the admin's template.
- When admin picks **Lead recipient**: ticking the channel shows a one-line info label ("SMS will be sent to each lead's phone" / "Email will be sent to each lead's email") in place of the sub-form. No fields for you to fill — every delivered lead's own phone/email is used at send time.

**For operations / support:**
- Lead-recipient deliveries ship the literal tokens `{{lead.phone}}` / `{{lead.email}}` through `smsConfig.phoneNumber` and `emailTo` on the contract's delivery action. The SMS and email dispatchers interpolate these per-lead at send time — there is no per-lead config row, just one token-bearing action.
- The email action writes `emailTo` / `emailSubject` / `emailBody` (matches what the email dispatcher actually reads). An older `bodyTemplate` typo on the SMS placeholder was corrected at the same time.
- Defaults are codified in Zod: omit `smsAudience` from a saved step config and it resolves to `buyer`; omit `emailAudience` and it resolves to `lead`. Existing wizards in prod are not migrated — they inherit the defaults on next read.

### Deterministic SKU → Wizard/Offer provisioning (2026-05-13)

Until today, a Stripe checkout fired a Slack ping to ad-ops who had a ~15-minute window to manually link the buyer's contract to an offer before the buyer's magic link landed. Miss the window and the wizard ran off the tenant default with no offer attached, breaking lead flow until someone reconciled. SKUs now carry the wizard and offer themselves, so provisioning is deterministic at webhook time.

**For admins:**
- New **Onboarding & Offer** card on the SKU edit form (`/admin/skus/[id]`) with two dropdowns:
  - **Wizard** — lists tenant active wizards with the tenant default marked. When set, this wizard runs for any buyer who purchases this SKU, regardless of which offer the resulting contract attaches to.
  - **Offer** — lists same-vertical active offers in the tenant (browser-filtered against the SKU's `verticalId`). When set, the Stripe checkout webhook auto-creates the `offerContract` row at provisioning time. No more Slack hand-off.
- The SKU list page now shows a compact **Setup** column with pill badges so you can see at a glance which SKUs are fully wired (Wizard configured + Offer configured) vs. falling back to the legacy Slack ad-ops flow.
- **Validation:** changing the SKU's vertical auto-clears the Offer if it no longer matches. Write-time validation on `POST /api/admin/skus` and `PUT /api/admin/skus/[id]` rejects mismatched offers with `400 VALIDATION_ERROR`.
- **Legacy SKUs without either field set keep the existing flow** — Slack still pings ad-ops, manual offer linking still works. No migration pressure to backfill every SKU at once.

**For buyers:**
- Same experience visually — fill out the wizard, hit complete, contract gets provisioned. Behind the scenes the wizard they go through and the offer their contract attaches to are deterministic now: they don't rely on ad-ops being awake when the Stripe webhook fires.

**For operations / support:**
- **Contract reuse on second purchase.** When a buyer makes a *second* purchase in the same vertical, the new buyerSku attaches to the **existing** contract for `(buyerId, verticalId)` in status `setup | active | paused` instead of creating a duplicate. The lead pack accumulates onto the same contract — no fresh Slack ping, no offer auto-link (the existing offer stands), and the wizard does NOT re-run (`buyer.wizardCompletedAt` is already set). Audit row `sku_attached_to_existing_contract` records the attach.
- **New contract naming.** Newly-created contracts use `{First Last} - {Vertical}` (e.g. `John Doe - IUL`). Fallbacks: email local-part if both first + last name are missing, `Buyer` if everything is missing, `General` if vertical is somehow missing. Existing contracts named `{SKU name} (auto)` are NOT retroactively renamed — only new contracts created from today forward use the new format.
- **Slack ad-ops pings are now narrower.** The ping fires only when `sku.offerId` is NULL or the auto-link fails (offer deleted, vertical mismatch, cross-tenant guard). The 15-minute race window for manual offer linking is dead for any SKU with `offerId` set.
- **Resolution chain.** When a magic link lands on `/welcome`, the wizard is resolved in this order: `sku.wizardId` → `offer.wizardId` → tenant `isDefault=true` wizard → throw. The chain has always had the tenant-default fallback at the tail; we just added the SKU layer at the head.
- **Backlog:** the bulk-import route doesn't accept the new fields yet — admins must edit SKUs individually for now to wire wizard + offer. Bulk imports default both to NULL (safe — falls through to the legacy flow).

## Buyer Dashboard

The buyer dashboard provides a self-service interface for buyers to manage their accounts and view purchased leads.

### Dashboard Overview

**Location:** [/buyer](/buyer)

Overview showing:
- Leads received today
- Leads this month
- Current balance (warning if low)
- Pending returns
- Recent leads table

**Return decision popup:** When the routing team approves or rejects one of your return requests, a popup opens the next time you visit the dashboard showing each decision — the lead, the outcome, the refunded amount, and any note from the team. Click **Got it** (or close the popup) to dismiss it; the decision details stay visible on the lead's detail page afterwards.

### Portal Leads

**Location:** [/buyer/leads](/buyer/leads)

View qualifying leads sold to your buyer account with filters:
- Search by name, email, phone
- Filter by status
- Date range filtering

### Portal Lead Detail

**Location:** [Buyer Leads](/buyer/leads) > Lead Detail

View comprehensive lead information:
- Contact details
- Address
- Custom fields
- Notes
- TCPA consent details

**Return Requests:**
If within 7 days of purchase, request a return:
1. Click "Request Return"
2. Select reason
3. Add optional notes
4. Submit for review

While a return is under review the lead detail page shows an amber "under review" banner. Once decided, the banner turns green (approved, with the refunded amount) or red (declined), and includes any note the team attached to the decision.

### Contracts {#buyer-contracts}

**Location:** [/buyer/contracts](/buyer/contracts)

Everything you are set up to receive, and whether it is flowing right now.

**Delivery.** A card at the top reports how many of your contracts are active or paused and carries one **Pause Delivery Only** / **Resume Delivery** button that acts on all of them at once. This card used to sit on the Billing page; it lives here so the switch is next to the contracts it switches.

**Your Contracts.** Below it, one row per contract showing units remaining across *every* package purchased under that contract — you never add packs up by hand — plus a per-contract **Pause** / **Resume** and a link into the contract.

**The contract table.** The full list underneath: product, vertical, lead/call type, status, pack count, and units remaining, each row linking to its detail page.

Pausing anything here can cross-prompt your subscription billing — see [Pausing & Resuming](#buyer-pause-resume).

### Contract Delivery Settings {#buyer-contract-delivery}

You can also pause or resume delivery for this contract directly from the **Delivery Controls** card on the same page — including optionally pausing/resuming the linked subscription billing — without going back to the Contracts list.

**Location:** [/buyer/contracts](/buyer/contracts) > Contract Detail

Each contract has a **Delivery Settings** card you can edit yourself — no need to email the routing team. Changes apply to future routing immediately; they never affect leads already delivered.

**Transfer numbers (call contracts only).** On call-enabled contracts the card shows a **Transfer Number** and an optional **Backup Number**. The transfer number is where we bridge inbound calls; the backup is used only if the primary does not answer (leave it blank if you have none). Enter a US 10-digit number or E.164 format (`+1...`). Number changes take effect on the next call. Non-call contracts do not show these fields.

**Delivery schedule.** Set a **Timezone** (your operating timezone) and a per-day hours grid marking the days and hours the contract accepts leads. Hours are measured in the timezone you pick. **An empty schedule means the contract is always on** (leads accepted around the clock). Overnight windows are supported — a window that starts in the evening and ends the next morning carries across midnight.

**State filters.** Restrict which states the contract accepts. Choose **Include** to receive leads only from the selected states, or **Exclude** to receive leads from everywhere except the selected states.

#### Delivery Destinations {#buyer-delivery-destinations}

Below Delivery Settings, the **Delivery Destinations** card shows exactly where your accepted leads are sent. Each destination is a card with a type badge, an **Enabled**/**Disabled** status, and a **Primary** pill on the main destination. Destinations are set up by your account manager; the SMS message text is the one part you can edit yourself.

What you see depends on the destination type:

- **Webhook (POST or GET).** The destination URL (scheme, host, and path only — query strings are hidden). If the URL can't be shown safely it reads **Endpoint configured**.
- **Email.** The **To** address and any **CC** address leads are emailed to.
- **Google Sheets.** A clickable link to the connected sheet — it opens the live Google Sheet in a new tab.
- **SMS.** Who the message is sent to (**Sends to:** a fixed number, or **the lead's own phone**), the message text, and a note if a template fallback is configured.
- **FTP.** Shown as a configured destination; details are managed by your account manager.

**Editing the SMS message.** On an SMS destination, click **Edit message** to change the text sent for each accepted lead. You can only edit the message body — the destination number, sending number, and any template are managed by your account manager. Use the merge-field buttons (`{{firstName}}`, `{{lastName}}`, `{{phone}}`, `{{email}}`, `{{state}}`, `{{city}}`, `{{zip}}`) to insert values that are filled in with each lead's details at send time. A live counter shows the character count and how many carrier **segments** the message uses (longer messages split into more segments). Messages are limited to 1,600 characters. Read-only members can view the message but not edit it.

### Buy More {#balance-payments}

**Location:** [/buyer/billing](/buyer/billing)

The purchase surface, and where you land after logging in. It holds your current balance, the catalog of packages you can buy (searchable and filterable by vertical, lead/call inventory, one-time vs subscription, and promos), the checkout flow with coupon and account-credit options, and a **Custom Top-Up** for adding funds without buying a package.

What you already own is not here — that is [My Packages](#buyer-my-packages) — and delivery controls are on [Contracts](#buyer-contracts).

**Balance Overview:**
- Current balance
- Credit limit
- Available credit

**Auto-Recharge Settings:**
View (contact admin to modify):
- Status
- Recharge amount
- Threshold

**Recent Transactions:**
Recent buyer-scoped transactions showing type, amount, and balance. Legacy `/buyer/balance` redirects to `/buyer/billing`.

#### Pausing & Resuming (delivery ⇄ subscription cross-prompts) {#buyer-pause-resume}

**Delivery** (whether contracts route leads and calls to you) and **subscription billing** (whether Stripe renews and charges you) are two separate switches. To stop you paying for inventory you can't receive, the portal cross-prompts between the two whenever you pause or resume one of them. Delivery controls live on [Contracts](#buyer-contracts); subscription billing controls live on [My Packages](#buyer-my-packages).

- **Pause Delivery Only** (global, on Contracts) and per-contract **Pause** open a confirmation modal. If the affected contract(s) have an active subscription, a checkbox **"Also pause subscription billing?"** appears, **checked by default** (recommended) — pausing delivery without pausing billing is what leaves a buyer paying while dark. Leave it unchecked to pause delivery only. The global button cascades to every active subscription; the per-contract button only affects subscriptions on that contract.
- **Resume Delivery** (global, on Contracts) and per-contract **Resume** open a modal. If a paused subscription is affected, a checkbox **"Also resume subscription billing? (this will resume charges)"** appears, **unchecked by default** — because resuming a subscription re-bills you. Check it only when you're ready to be charged again.
- **Resume Billing** (on a paused subscription, on My Packages) opens a modal with **"Also resume lead delivery?"**, **checked by default** (recommended) so you receive the leads the subscription pays for.

**Re-bill caveat:** resuming a Stripe subscription resumes charges on its normal renewal schedule. The resume cross-prompts default the billing-resume checkbox to unchecked and label it clearly so a charge never fires without an explicit opt-in. Pausing is always safe (it only stops charges); resuming billing is the only money event.

Each confirmation shows a success toast describing exactly what happened (e.g. "Lead delivery paused; subscription billing paused" vs "Lead delivery paused; billing unchanged").

**Admin status changes cascade too.** The same cross-prompt now fires when an admin changes status from the admin dashboard, so an admin pause never leaves a buyer paying while dark (and a reactivation never silently re-bills):

- **Admin → Contract detail → Status** flipped active→paused (or paused→active) on a subscription-backed contract opens a confirm modal with **"Also pause subscription billing?"** (checked by default) / **"Also resume subscription billing? (this resumes charges)"** (unchecked by default).
- **Admin → Buyer detail → Deactivate Buyer** modal adds a second checkbox **"Also pause this buyer's N active subscriptions?"** (checked by default) alongside the existing "expire contracts?" choice, shown only when the buyer has active subscriptions.
- **Admin → Buyers / Contracts list → bulk Set status** to paused/inactive (or active) opens a confirm modal offering **"Also pause/resume subscriptions for affected rows?"** (checked for pause, unchecked for resume).

In every case the cascade is best-effort: a Stripe failure never rolls back the status change, and the toast reflects the actual outcome (it surfaces "needs attention" if the subscription side failed).

### My Packages {#buyer-my-packages}

**Location:** [/buyer/skus](/buyer/skus)

Everything you have bought, and what is left of it. Each row is one purchased package with its purchase date, status, units used, and units remaining, filterable by status (active, exhausted, expired, refunded, cancelled). Expanding a row shows the funding trace behind it — what paid for it and whether it can still fill inventory today.

**Subscription Billing.** Beneath the package list, every recurring package you own with its billing state (**Active** / **Paused**) and a **Pause Billing** / **Resume Billing** button. This section moved here from the Billing page, so what you own and the charge that renews it read together. A package that is exhausted but still billing stays listed — that is exactly when you want the pause button. Pausing prompts to also stop receiving inventory (checked by default); resuming prompts to also resume delivery. See [Pausing & Resuming](#buyer-pause-resume).

### Payment History

**Location:** [/buyer/transactions](/buyer/transactions)

Full transaction history with filters:
- Type (Sale, Refund, Payment, Adjustment, Fee)
- Date range

### Portal Reports

**Location:** [/buyer/reports](/buyer/reports)

Analytics on lead spending:
- Total leads and spend
- Average cost per lead
- Breakdown by status
- Breakdown by vertical
- Daily activity chart

### Portal Settings

**Location:** [/buyer/settings](/buyer/settings)

**View Contact Info:**
(Contact admin to update)

#### Lead Completion Experience {#buyer-lead-completion}

Self-service control of what the consumer sees after they submit a lead you buy. theLeadRouter does not host a completion page — these values are returned to the **partner's landing page** in the lead-submit response, and the partner's page decides how to use them. Whether they are actually sent is still governed by per-campaign toggles managed by your account manager, so if a field isn't reaching partners, ask them to enable it on the campaign you're bought into.

- **Completion phone** (`partnerResponseBuyerPhone`): a phone number in E.164 format (e.g. `+12131231234`) the partner's page can show or warm-transfer the consumer to. Leave blank to clear.
- **Redirect URL** (`partnerResponseRedirectUrl`): where the consumer is sent after submitting. Two ways to use it:
  - A **branded / about-you page** — your website or a landing page introducing your company.
  - A **scheduling link** — a Calendly, Cal.com, or Google appointment URL. Scheduling links are auto-detected (see below), and partner landers can embed your booking calendar directly on the completion page instead of redirecting.
  - The URL supports the `{{leadId}}` and `{{partnerId}}` tokens, which are substituted at response time. Any other `{{token}}` is rejected. The URL must be `http(s)`.
- **Photo or video** (`partnerResponseBuyerMediaUrl`): an optional face shot or brand intro shown alongside the response. JPG/PNG/WebP photos up to 10 MB; MP4/WebM videos up to 50 MB. Upload replaces any prior media; remove clears it.

Read-only portal members can view these settings but cannot change them.

**API Keys:**
- Generate new `lr_` portal API keys for buyer-scoped programmatic access
- View existing keys (portal keys only — posting keys are admin-managed)
- Keys shown only once when created

**Sign-in:**
Use `/login` magic link, Google, or passkey. Password-management API routes are deprecated.

---

## Partner Portal {#partner-portal}

The Partner Portal provides lead source partners with self-service access to their lead activity, offers, and payouts. Internally these are modeled as "campaigns" (the partner-facing record of a lead source), but in the partner portal UI they are labeled **Offers** everywhere end-users see them.

### Accessing the Portal
Partners log in at [/login](/login) using credentials created by the admin. Admin users can create partner portal users from the admin panel by creating a user with the `partner` role and linking it to a partner entity.

### Dashboard
The partner dashboard shows key metrics:
- **Leads Today / This Month**: count of leads submitted
- **Earnings Today / This Month**: total cost of sold leads
- **Current Balance**: accrued earnings awaiting payout
- **Next Payout**: scheduled payout date

### Leads
View all submitted leads with search and filter by status. Click a lead to see:
- **Home**: contact info, lead fields, cost/revenue
- **Dispositions**: timeline of status changes
- **Revenue Events**: sales prices and delivery status (buyer names hidden)
- **Distribution**: routing waterfall with disposition and pricing

### Offers (partner-facing)
View all your offers with posting URLs. Partners use the posting URL to submit leads via API:
```
POST /api/v1/leads/submit
Authorization: Bearer YOUR_POSTING_KEY
```

**Lead IDs:** API responses return an 8-character short ID as `leadId` (e.g., `"aB3xK9mQ"`). Use this ID to look up leads via the API or in URLs. Both short IDs and full UUIDs are accepted when querying leads.

**Change Detection:** Partners can poll `GET /api/partner/campaigns/sync` to efficiently detect configuration changes. The endpoint returns a hash that changes whenever offers, fields, or posting specs are modified, reducing unnecessary full-sync API calls from partner integrations.

#### Public Posting Spec Links {#public-spec-links}

Each offer has a public spec link that can be shared with developers who need to integrate but don't have portal access. The link shows the full posting specification (fields, sample cURL, example responses) without requiring login.

- **Copy the link** from the offer detail > Posting Specs tab (both admin and partner portal)
- **Regenerate** the link if it's been compromised — the old link stops working immediately
- The link does **not** include the API key — send that separately via a secure channel
- URL format: `/specs/{campaignId}?token={specToken}` (the `campaignId` path segment is the internal identifier — unchanged for API compatibility)
- Append `&format=text` to the URL for a plain-text markdown version suitable for developer tools and AI assistants

#### Call Tracking & DNI Embed (partner-facing) {#partner-call-tracking}

Call-enabled offers show a **Call Tracking** card on the offer's Posting Specs tab (partner portal) and a **Dynamic Number Insertion (DNI)** section in the Call panel of the public spec page:

- The **tracking number** is the static inbound number for the offer — display it on landing pages and calls route automatically.
- When DNI is enabled, a copyable **embed script** (`<script src="{origin}/dni/{specToken}.js" async></script>`) is shown. Place it just before the closing `</head>` tag (or at the end of the body).
- Add `class="lr-number"` or `data-lr-number` to phone elements on the page for a flicker-free number swap.
- Partners see only whether DNI is on and the embed snippet — pool sizing, path targeting, and other DNI configuration stay internal.

#### Build your lander with AI {#partner-ai-lander}

Instead of hand-wiring a landing page, use the **Copy AI builder prompt** button to generate a ready-made prompt for Claude Code that scaffolds a complete, working page for the campaign. The button appears on the partner Posting Specs tab (inside the Call Tracking card for call campaigns, or as its own card for lead-only campaigns) and on the admin campaign's Call Tracking panel.

Click it, paste the copied prompt into Claude Code, and it builds a single self-contained `index.html` wired to this campaign. The prompt adapts to the campaign type:

- **Lead campaigns (form posting)** — a **standard multi-step quiz** (not a flat form): each qualifying question (fields with fixed answer options, plus Yes/No booleans) is its own step with big tappable buttons that auto-advance, a progress bar, and a back link. Free-text contact fields (name, phone, email, ZIP) group on the final step, followed by a thank-you step. Hidden inputs mirror every answer using the exact posting-spec field name the moment it's chosen — so capture and lead posting map 1:1, and the prompt tells Claude Code never to rename these fields. Lead submission posts to the endpoint with your Bearer key (when available), with UTM passthrough and retry handling. A TCPA consent checkbox on the contact step carries a `[REVIEW: compliance language for <vertical>]` placeholder — the prompt never invents legal text, so a human must finalize the disclosure before going live.
- **Call + lead campaigns (hybrid)** — the same quiz, plus phone CTAs (tracking number wrapped in `class="lr-number"` inside an E.164 `tel:` link) kept in a sticky header on every step, the DNI embed and anti-flicker cloak, and a "Call now to speak with a licensed agent" CTA on the thank-you step.
- **Call-only campaigns** — instead of a quiz, a pure **click-to-call page**: an above-the-fold urgency headline ("Your spot is reserved — licensed agents are standing by"), a large tap-to-call button, trust/benefit cards, and secondary call CTAs — all on the tracking number. UTM params are still captured for attribution. The prompt bans fake countdown timers and fabricated statistics and flags any factual claim for human review.

If the prompt embeds a live posting key, treat it as a secret: don't commit it to a public repo or share it in an open channel.

#### Posting Keys vs Portal Keys {#posting-portal-keys}

The system uses two types of API keys for partners:

**Posting Keys** (`pk_` prefix, admin-managed) — scoped to `POST /api/v1/leads/submit` only. One per partner, auto-provisioned when the first offer is created. Shown on the offer's Posting Specs page. Only admins can regenerate a posting key (Partner detail > API Keys tab > Regenerate Posting Key). The old key stops working immediately.

**Portal Keys** (`lr_` prefix, partner-managed) — scoped to the partner portal API (`/api/partner/*`). Partners create and revoke these from Settings > API Keys. Use portal keys for programmatic access to offers, leads, payouts, and other portal data. Partner-minted keys are always **read-only** by default; only an admin can grant write access on a portal key (Partner Detail > API Keys tab > "Make Read/Write" action). Partners requesting write access should contact their account manager.

When sharing integration credentials with a developer:
1. Share the **posting key prefix** shown on the campaign Posting Specs page (or send the full key via secure channel)
2. Share the **public spec link** — it shows fields, sample cURL, and example responses without requiring login
3. Do **not** share portal keys with developers who only need to submit leads

#### Read-Only API Keys {#readonly-api-keys}

Read-only keys allow `GET` / `HEAD` / `OPTIONS` requests only. Any `POST`, `PUT`, `PATCH`, or `DELETE` returns `403 { "error": { "code": "READ_ONLY", "message": "This API key is read-only." } }`.

Create one from **Admin → API Keys → Create** and tick the **Read-only key** checkbox before shipping. The admin list view shows a "Read-only" badge on any key that was minted with the flag set.

Partner-scoped keys support the toggle in two places:
- **Admin → Partners → [partner] → API Keys** has a Read-only checkbox in the Create Portal API Key card and an Access column on the keys table showing **Read-only** vs **Read/Write** badges.
- **Partner Portal → Settings → Portal API Keys** lets partners self-mint a read-only key from the Generate New Key panel; the keys list shows a Read-only badge next to the key name.

Typical use cases: reporting scripts, BI / warehouse sync jobs, and external integrations that should never write. A reporting employee can hold a read-only key for scripted pulls while keeping a normal session login for any admin work they still need to do — only the API key is constrained.

Key-level read-only and user-level read-only (see [Read-Only Users](#readonly-users)) are independent. A write-capable user can mint read-only keys; a read-only user always mints read-only keys (forced). The `READ_ONLY` 403 on API requests can originate from either the key flag or the user's session flag.

PII redaction still applies by default — standard keys inherit the masked view regardless of the read-only flag. To unmask PII on reads, see [PII-Access API Keys](#pii-access-api-keys).

#### PII-Access API Keys {#pii-access-api-keys}

By default API keys return masked PII (`firstName`, `lastName`, `email`, `phone`, address, `dateOfBirth`, `zip`, custom PII-flagged fields). Tick **Allow PII access** on key create to mint a key that returns unmasked values. The admin list view shows a red **PII** pill on any such key.

Create from **Admin → API Keys → Create** and tick **Allow PII access**. The checkbox is disabled (with a gray hint) unless the creator has `piiAccess` on their own account — you cannot grant a privilege you don't hold. Superadmins bypass this. Attempting to create a PII key without the privilege returns `403 FORBIDDEN`.

Recommended combo for BI / warehouse / CRM sync: **read-only + PII access**. The external system pulls unmasked rows but cannot write back — the smallest blast radius for a leaked credential that still does the job.

The red warning on the create dialog exists because an unmasked key leaking out is a data-breach notification event. Issue sparingly, rotate on any suspicion, and revoke immediately when the integration is retired.

#### Acceptance Criteria {#acceptance-criteria}

The **Acceptance Criteria** tab on the offer detail page (`/partner/campaigns/[id]` — URL uses the internal `campaigns` slug unchanged) shows partners what leads will be accepted for this offer. The criteria are compiled server-side from all active buyer contracts linked to the offer:

- **States Accepted** — which US states have at least one active buyer willing to purchase leads
- **Schedule Hours** — the widest acceptance window per day of week across all contracts, with timezone displayed
- **ZIP Codes** — accepted or excluded ZIP code lists, if any contracts have ZIP filters
- **Custom Filters** — field name, operator, and accepted values for any additional contract filters (e.g., age range, homeowner status)

This gives partners a clear picture of routing criteria without exposing individual buyer details.

### Payouts
View payout history with current balance, total paid out, and next payout date. Payout cycles (weekly, bimonthly, monthly) are configured by the admin. Payouts are auto-generated when the balance meets the payment threshold.

### Webhooks

**Location:** [/partner/webhooks](/partner/webhooks)

Partners configure their own postback webhooks from the portal with the partner-safe webhook manager:

- **Delivery methods:** `GET` or `POST`.
- **Events:** `lead.received`, `lead.sold`, `lead.rejected`, `lead.returned`, `lead.dispositioned`, `lead.dq_distributed`, `cpa.converted` (partner-safe event set — no buyer/contract events exposed).
- **Token URL builder:** type `{` inside the URL field to insert tokens (`{Lead_ID}`, `{Payout}`, `{Partner_ID}`, etc.) — tokens are filtered to the partner-safe whitelist, so no buyer or contract data is ever leaked through a partner postback.
- **Delivery log:** expand any webhook row to see the last 50 deliveries with full request URL, request body, response status, duration, and delivered timestamp. Each row has a **Refire** button that re-fires the delivery against the current configuration.
- **CSV export:** download the delivery log as CSV from the expanded row header.
- **Test fire:** send a synthetic delivery to validate the endpoint before leads start flowing.
- **Auto-pause:** webhooks that fail 10 consecutive times are paused automatically and show a failure count. Fix the endpoint and flip status back to `active` to resume (the failure counter resets).

### Postback Report

**Location:** [/partner/reports/postbacks](/partner/reports/postbacks)

Operational health check for the partner's webhooks. Shows, for the selected date range: total leads, sold leads, webhook fires (total), successful, failed, and missing (where a webhook *should* have fired but no delivery row exists).

**What the report audits** — the banner at the top of the page splits the partner's subscribed events into two groups:

- **Audited** (blue pills) — `lead.received`, `lead.sold`, `lead.rejected`. The report computes a "should have fired" expected set for each of these from the partner's lead data and flags any leads where no delivery row exists for that event.
- **Tracked (delivery log only)** (gray pills) — `cpa.converted`, `lead.returned`, `lead.dispositioned`, `lead.dq_distributed`. These fire and are logged, but there's no well-defined "expected set" to audit against (conversions depend on disposition state, returns depend on buyer action, etc.). The delivery log tab on the Webhooks page is the source of truth for these.

**Subscription-aware** — the report only audits events the partner is actually subscribed to. If Adveropia subscribes only to `cpa.converted`, the "Missing" summary card will always read 0 and no discrepancy rows will appear for `lead.sold` — because no `lead.sold` webhook is set up, no row is expected. The system does not fire (or log) a webhook unless an active subscription matches the event.

**Historical cutoff** — if a partner adds a `lead.sold` webhook today, the report will not flag yesterday's sold leads as "missing." The earliest webhook `createdAt` per event becomes the cutoff: only leads created on or after that timestamp are audited.

**Multisell-correct math** — per-campaign `Sold Hooks`, `Received Hooks`, and `Rejected Hooks` counts are distinct-lead counts, not delivery-row counts, so a multisell lead with 3 `lead.sold` deliveries counts as 1 (not 3) toward the "delivered" numerator.

**Subscription scope** — the report considers webhooks from four scopes that can fire for the partner's leads: global, partner-scope (their own), offer-scope (for any offer their campaigns route through), and campaign-scope (for any of their campaigns). All four contribute to the "Audited" / "Tracked" banner.

**Empty state** — if the partner has zero active webhooks, the report shows a yellow banner linking to `/partner/webhooks` with no discrepancy rows.

### Time of Revenue & Time of Lead Reports {#partner-reports}

Two time-series reports in the partner portal, scoped to the logged-in partner. Both linked from the sidebar **Reports** menu.

- **Time of Revenue** — sidebar flyout with 4 views over the same date range:
    - **Summary** — [/partner/reports/revenue](/partner/reports/revenue) — daily aggregate, split into three buckets:
        - **CPL** — cost-per-lead payout: `lead.cost + lead.dqPayout` (earnings for each qualified or DQ-distributed lead)
        - **CPA** — cost-per-action payout: `lead.cpaPayout + lead.dqCpaPayout` (bonus earned when a lead converts)
        - **CPCall** — call revenue share: `callSale.payout + callSale.cpaPayout` (earnings for qualifying inbound calls and call-based conversions)
    - **Leads** — [/partner/reports/revenue/leads](/partner/reports/revenue/leads) — log of paid-CPL leads in the range, one row per lead (lead ID, received, offer, sub1-5, CPL Payout). Pivot by Day / Offer / Sub1-5 for aggregated roll-ups. Only leads with `cost + dqPayout > 0` appear.
    - **Calls** — [/partner/reports/revenue/calls](/partner/reports/revenue/calls) — log of paid call revenue, one row per paid callSale (call ID, received, offer, duration, Call Payout). Pivot by Day or Offer only — sub pivots are rejected because `callSale` has no sub tracking columns.
    - **Acquisitions** — [/partner/reports/revenue/acquisitions](/partner/reports/revenue/acquisitions) — unified log of CPA conversions from both lead and call sources (source badge, entity ID, converted date, offer, sub1-5 for lead rows, CPA Payout). `source=lead|call|all` toggle filters the view. Sub1-5 pivots collapse call-source rows into a single `(call)` bucket.
- **Time of Lead** — [/partner/reports/time-of-lead](/partner/reports/time-of-lead) — daily lead counts for the selected date range, bucketed by `lead.createdAt` and split by status: **sold**, **pending**, **returned**, **rejected**, plus a total. Time of Lead uses the same preset date ranges, URL-state, Offer/Sub filters, Pivot By (Day / Offer / Sub1 / Sub2 / Sub3), drill-down, summary cards, and 3-chart day view as Time of Revenue — the only difference is that Time of Lead buckets by lead intake date (when the lead was received), while Time of Revenue buckets by the revenue-event date.

Both pages show summary cards, bar charts (Day view), a daily breakdown table with row-click drill-down, and a pivot table (non-Day views). Default range is the last 7 days; range is capped at 365 days.

**Preset date ranges** — quick-pick one of **Today**, **Last 7 Days**, **Last 30 Days**, **Last 90 Days**, **This Month**, **Last Month**, or **Custom** to enter your own window. The preset, the start/end dates, the pivot, and every filter are mirrored into the page URL — copy the URL to share an exact view with a teammate.

**Summary cards (Time of Revenue)** — six KPIs at the top: **Total Payout**, **CPL**, **CPA**, **Paid Sales** (paid leads + paid calls), **Avg CPL** (CPL payout per paid lead), and **Avg CPA** (CPA payout per paid conversion). Averages guard against divide-by-zero when no paid rows exist.

**Summary cards (Time of Lead)** — five counts at the top: **Total Leads**, **Sold**, **Pending**, **Returned**, and **Rejected**. These mirror the status columns in the daily table.

**Pivot By** — group the report by **Day** (default, with charts and drill-down), **Offer**, or **Sub1 / Sub2 / Sub3 / Sub4 / Sub5**. Calls log pivots are restricted to **Day** and **Offer** only. Buyer and Contract pivots are deliberately unavailable — partners never see per-buyer revenue attribution or contract identity.

**Filter bar** — narrow results by **Offer** and any of **Sub1 – Sub5** tracking values. Filters compose with the date range and pivot. Unknown or admin-only filter keys in the URL (e.g. `buyerId=`) are silently stripped server-side.

**Day drill-down (Time of Revenue)** — in Day view, click any date row to expand an inline panel with three sections — **Lead Sales** (lead ID, received, offer, sub1/2/3, **CPL Payout**), **Call Sales** (call ID, received, offer, duration, **Call Payout**), and **CPA Conversions** (source badge, original received, converted, **CPA Payout**). Each section is capped at the first 500 rows; a banner appears if the cap is hit — narrow the date range to see all. Every column is payout-only.

**Day drill-down (Time of Lead)** — in Day view, click any date row to expand an inline panel with two sections — **Leads on this day** (lead ID, received, offer, sub1/2/3, status badge, **CPL Payout**) and **Calls on this day** (call ID, received, offer, duration, status badge, **Call Payout**). All leads and calls received on the day appear, not just paid ones — that's the difference from Time of Revenue, which filters to paid rows only. Each section is capped at the first 500 rows.

**Extra count columns** — daily and pivot tables include **Paid Leads**, **Paid Calls**, and **Paid CPA** counts alongside CPL / CPA / CPCall / Total payout.

**Privacy guarantee:** partners see their own payout totals and lead counts only. Buyer revenue, buyer or contract identity, campaign cost to buyer, and admin audit data never cross the partner API boundary — the filter whitelist, pivot whitelist, and drill-down SELECT lists enforce this at every endpoint.

**Timezone:** days bucket in the tenant's local timezone. A lead created at 11:59 PM local on day N stays in day N, not day N+1.

### Settings
View contact information and payment configuration (read-only). Sign in through [/login](/login) with a magic link, Google, or passkey. Manage portal API keys for programmatic access to partner portal data. Posting keys are managed by admins — see [Posting Keys vs Portal Keys](#posting-portal-keys).

---

## Intake Centers {#intake-centers}

**Location:** [/admin/intake-centers](/admin/intake-centers)

Intake centers represent real-world call centers that receive and work delivered leads. They bundle delivery configuration and disposition code mapping in one place, so multiple contracts can share the same call center setup without duplicating configuration.

### Intake Center List

| Column | Description |
|--------|-------------|
| Name | Call center name |
| Status | Active, Paused, Inactive |
| Contracts | Number of contracts using this IC |
| Created | Creation date |

### Creating an Intake Center

**Location:** [/admin/intake-centers/new](/admin/intake-centers/new)

The create form is a slim identity form — delivery configuration and disposition mappings live on the detail page after creation (you land on the Delivery tab automatically).

**Required Fields:**
- **Name**: Descriptive name for the call center

**Optional Fields:**
- **Status**: Active (default) or Inactive
- **Notes**: Internal notes about this center

### Intake Center Detail

**Location:** [Intake Centers](/admin/intake-centers) > Intake Center Detail

**One save bar:** edits on the Info and Delivery tabs — plus staged edits/deletes of disposition mappings — feed a single floating save bar that appears when something changed and sends only the changed fields. Cancel reverts everything to the last saved values. Adding mapping rows and cloning mappings apply immediately.

#### Info Tab
Basic information: name, status, notes.

#### Delivery Tab
Shared delivery configuration used by all contracts assigned to this IC:
- **Method**: HTTP POST, HTTP GET, or Email
- **URL**: Delivery endpoint
- **Config**: Full HTTP configuration (headers, auth, body template, field mapping, response mapping)

**When email delivery fires.** With the Email method, emails send when a lead sale is **delivered** on a contract linked to this intake center — the contract link is checked first, with the offer link as fallback — including DQ and reciprocity-absorbed sales. One email goes out per **To** recipient, once per sale, through the platform email provider. Every attempt is recorded in the [Delivery Log](#delivery-log).

If the center is not active, a banner warns that it will not receive lead deliveries. The **Test Delivery** button sends a sample POST using your current (including unsaved) settings.

When a contract uses this intake center, it inherits this delivery config instead of maintaining its own.

##### Token Interpolation {#token-interpolation}

Delivery URLs, body templates, and headers support **token interpolation** — placeholders that are replaced with actual lead and entity data at delivery time. Use single-brace syntax: `{token_name}`.

**Available Token Categories:**

| Category | Example Tokens |
|----------|---------------|
| System IDs | `{Lead_ID}`, `{Sale_ID}`, `{Contract_ID}`, `{Offer_ID}`, `{Campaign_ID}` |
| Lead PII | `{firstName}`, `{lastName}`, `{emailAddress}`, `{phoneHome}`, `{address}`, `{city}`, `{state}`, `{zipCode}` |
| Sub IDs | `{Affiliate_ID}` (alias: `{Partner_ID}`), `{Sub_ID}`, `{Sub_ID_2}` through `{Sub_ID_10}` |
| Entity Names | `{Buyer_Name}`, `{Partner_Name}`, `{Campaign_Name}`, `{Offer_Name}` |
| Date Parts | `{Year}`, `{Month}`, `{Day}`, `{Hour}`, `{Minute}` |
| Account Manager | `{Account_Manager_Name}`, `{Account_Manager_Email}` |
| Alt IDs | `{Partner_Alt_ID}`, `{Campaign_Alt_ID}`, `{Offer_Alt_ID}`, `{Partner_External_ID}`, `{Campaign_External_ID}`, `{Offer_External_ID}` |
| Flags | `{Is_Test}`, `{Is_Exclusive}` |
| Compliance | `{TCPA_Consent}`, `{TrustedForm_Cert_URL}`, `{Jornaya_ID}` |

**Usage example** (body template):
```json
{
  "leadId": "{Lead_ID}",
  "name": "{firstName} {lastName}",
  "email": "{emailAddress}",
  "source": "{Campaign_Name}",
  "subId": "{Sub_ID}"
}
```

**Backward compatibility:** The older double-brace syntax `{{fieldName}}` still works. Both `{firstName}` and `{{firstName}}` resolve identically.

**Token Picker Panel:** When editing a body template, click the token browser icon to open a searchable, categorized panel. Click any token to insert it at the cursor position.

##### Response Mapping {#response-mapping}

Response mapping rules determine how buyer HTTP responses are interpreted as delivery outcomes. Instead of simple success/error string matching, you define pattern-based rules that map response content to specific outcomes.

**Creating a rule:**
1. Go to the Delivery tab on an Intake Center or Contract
2. In the Response Mapping section, click **Add Rule**
3. Enter a **Pattern** — text to match in the response body
4. Select **Match Type**: `substring` (plain text contains) or `regex` (regular expression)
5. Select **Delivery Outcome**: how a match should be interpreted

**Available delivery outcomes:**

| Outcome | Meaning |
|---------|---------|
| `success` | Buyer accepted the lead |
| `reject` | Buyer rejected the lead |
| `duplicate` | Buyer already has this lead |
| `no_match` | Lead didn't match buyer criteria |
| `failed_data_validation` | Lead data failed buyer validation |

**Auto-detected outcomes** (no rule needed):
- `timeout` — HTTP request timed out
- `communication_error` — connection failure or non-HTTP error
- `unknown_response` — no rule matched the response

**Rule priority:** Rules are evaluated top-to-bottom. The first matching rule wins. If no rule matches, the outcome is `unknown_response`.

**Example rules:**

| Pattern | Match Type | Outcome |
|---------|-----------|---------|
| `"success":true` | substring | success |
| `"status":"duplicate"` | substring | duplicate |
| `invalid.*email` | regex | failed_data_validation |
| `rejected` | substring | reject |

##### Delivery Test Harness

Before going live, test your delivery configuration with synthetic payloads:

1. On the Delivery tab, click **Test Delivery**
2. The system generates a sample lead payload using your token configuration
3. Review the full HTTP request (URL, headers, body with tokens resolved)
4. Execute the test to see the actual buyer response
5. Response mapping rules are applied to show the resulting delivery outcome

This lets you verify URL construction, token resolution, authentication, and response mapping without creating real lead records.

#### Disposition Mappings Tab
Map external disposition codes (sent by the call center) to internal system disposition codes. Mappings are scoped by **vertical group** so a single intake center can handle multiple verticals with different code sets.

**Per-mapping fields:**
- **External Code**: The code the call center sends (e.g., "SALE", "DNC", "VM")
- **System Code**: The internal disposition code it maps to
- **Override CPA**: Optionally override whether this mapping triggers a CPA conversion
- **Override Return**: Optionally override whether this mapping triggers a return

**Actions:**
- **Import System Codes**: Creates a 1:1 mapping of all system codes as a starting point
- **Clone to Vertical Group**: Copy all mappings from the current vertical group to another vertical group within the same intake center
- **Add Mapping**: Add individual external-to-internal mappings

### Assigning Intake Centers to Offers and Contracts

Intake centers are assigned at the **offer** level and inherited by contracts:

1. On the **Offer Detail > Details** tab, select an intake center from the IC dropdown
2. All contracts linked to that offer automatically inherit the IC
3. On the **Contract Detail > Details** tab, the IC field shows "None (inherit from Offer)" by default
4. To override, select a different IC on the contract — that contract will use its own IC instead of the offer's

### Contract Disposition Overrides

**Location:** Contract Detail > **Conversions** tab > Disposition Mappings

Individual contracts can override the IC-level disposition mappings:

- **Populate from IC**: One-click button that copies all current IC mappings into the contract as overrides (useful as a starting point for per-contract customization)
- **Add Override**: Add individual contract-level mapping overrides
- **Source column**: Shows where each active mapping comes from (Contract, Intake Center, or System)

### Disposition Cascade Flow

When a buyer or call center posts a disposition with an external code, the system resolves it through a priority cascade:

<!-- react-flow:disposition-cascade-flow -->

**Cascade priority:**
1. **Contract-level override** — if the contract has a mapping for this external code, use it
2. **Intake Center mapping** — if the contract (or its offer) has an IC assigned, look up the mapping for the IC + vertical group
3. **Direct code lookup** — fall back to matching the code directly against the vertical group's disposition codes (backward compatible)
4. **Unmapped** — if no match is found at any level, the disposition is stored as "unmapped". CPA and return triggers do not fire for unmapped dispositions.

**Unmapped override:** If a postback fires before a disposition mapping is configured, the code is stored as unmapped and triggers are skipped. Once the mapping is added, re-firing the same postback will detect the previously unmapped record, replace it with the mapped version, and fire any CPA or return triggers normally. This means you can safely add mappings after the fact — just re-send the postback to trigger the conversion.

---

### Delivery Log {#delivery-log}

The **Delivery Log** is the single source of truth for every outbound delivery — HTTP POST, HTTP GET, SMS, email, Google Sheets, and webhook — across real lead deliveries and admin test sends.

<!-- react-flow:delivery-log-flow -->

**Channels covered**

- `http_post` / `http_get` — buyer URL deliveries
- `sms` — Twilio SMS deliveries
- `email` — SMTP email deliveries
- `google_sheets` — rows appended to a shared sheet
- `webhook` — disposition/event push-backs

**Rows are per-attempt, not per-sale.** Each retry is its own row. This makes retry waterfalls easy to read — you can see exactly how many attempts happened, which failed, and what the buyer's response was on each.

**Test vs real**

- `isTest = true` rows come from the contract **Send Test** button in the admin UI. No lead exists for these — `leadId` and `leadSaleId` are null.
- `isTest = false` rows are live production deliveries tied to a real `leadSale`.
- The **Test** filter at the top of the page toggles between "All", "Real only", and "Test only".

**Retention**

Rows older than **90 days** are deleted nightly by a cron job. This matches the posting-log retention policy.

**Show legacy rows**

A toggle at the top right switches to pre-cutover data. Before the unified delivery log shipped, HTTP deliveries were summarized on the `leadSale` row (one row per sale, latest attempt only, no per-attempt history). SMS deliveries and test sends wrote to the posting log instead. When **Show legacy rows** is on, the page queries the old `leadSale` data and shows the pre-cutover column set (Delivery Status, Outcome, Response, Attempts, Price, Buyer, Contract, Lead). The two shapes are different so the page intentionally swaps column sets instead of trying to unify them.

**Buyer view**

Buyers have a scoped version at `/buyer/delivery-log`. They see only their own rows, never test rows, and cannot see other buyers' deliveries. Phone numbers and email addresses are masked (last 4 digits / domain only). Raw request/response bodies are admin-only.

### Delivery Inspector {#delivery-inspector}

Click any row in the Delivery Log tab to open a detailed request/response inspector.

**Request tab** shows:
- HTTP method and destination URL
- Request headers (Content-Type, signature, event type)
- Full JSON request body

**Response tab** shows:
- Response headers from the receiving server
- Response body (truncated to 10KB)

The summary bar shows the HTTP status code, response time in milliseconds, attempt number, and timestamp.

**Copy as cURL** — Click the button on the Request tab to copy a ready-to-paste cURL command for replaying the exact request in your terminal.

> Deliveries created before this feature was added will show "Data not captured for this delivery" since the request/response details were not recorded at that time.

### Redelivering to a Buyer

When a delivery to a buyer fails — bad endpoint, transient 5xx, timeout — you can manually redeliver from the lead detail page.

1. Open the lead at **Leads → [lead id]**
2. Click the **Deliveries** tab
3. Find the row you want to retry and click **Redeliver**
4. Optionally adjust the request body, endpoint URL, method, or headers
5. Click **Redeliver** in the modal

Each redeliver writes a fresh `deliveryLog` row with `attemptNumber + 1`. It does NOT create a new sale, does NOT trigger billing, and does NOT touch the original `leadSale`. The action is recorded in the audit log.

Currently HTTP-only — SMS, email, and Google Sheets channels show a disabled button.

### Aborted-Delivery Reconciler {#aborted-delivery-reconciler}

Some buyer endpoints write the lead synchronously and then run heavy post-insert work that exceeds our 30-second primary-delivery timeout. Our AbortController fires, we mark the sale `failed`, but the record is already at the buyer. Result: the lead is at the buyer, but our books say it isn't — caps don't increment, the buyer isn't charged, and CPA postbacks have nothing to attribute against.

The aborted-delivery reconciler is a background cron that runs **every 30 minutes** and resolves these zombies.

**How it works**

1. Scans `leadSale` rows whose `deliveryStatus = 'failed'` and whose stored failure response looks like an abort or timeout, AND whose `createdAt` is between 5 minutes and 6 hours ago. The lower bound avoids racing in-flight primary retries; the upper bound stays inside typical buyer dedup windows so we don't create a new record at the buyer.
2. For each candidate, checks the contract's first delivery config. If its `responseMappingRules` array contains at least one rule with `interpretAs = 'duplicate'`, the contract is opted in.
3. Re-POSTs the original payload with a **15-second timeout** (shorter than primary — we're confirming presence, not delivering).
4. Runs the response through the same response-mapping engine the primary delivery path uses. If the outcome is `duplicate` or `success`, the sale is flipped to `delivered`, the cap is incremented, the buyer is charged (only on `revenueOnAcceptance` contracts — CPA contracts charge later via postback), and an audit row is written to the Delivery Log tagged `errorMessage = 'reconciled-from-abort'`.
5. If the retry returns a non-duplicate response, or aborts itself, the cron writes an audit row but leaves the original sale alone — the next run will retry while the sale is still in window.

**Opt-in**

There is no flag or toggle. A contract is opted in by adding a response-mapping rule whose `interpretAs` is `duplicate`. The rule's presence is the opt-in. Add the rule under contract → Delivery → Response Mapping.

**What it does NOT do**

- Does not fire webhooks. If your dashboard subscribed to `lead.failed`-style events, you may have already received a failed event on the primary attempt — a delayed `delivered` event downstream would confuse real-time consumers, so the reconciler is silent on the webhook channel by design.
- Does not capture the buyer's record ID. Buyers' duplicate-detection responses (e.g. `{"error":"Duplicate found."}`) typically don't include the original record's ID. CPA postbacks for reconciled sales attribute via email/phone match.

**Audit trail**

Every retry — whether it reconciles or not — writes a row to the Delivery Log. Reconciled rows are stamped `errorMessage = 'reconciled-from-abort'` so they're greppable apart from primary deliveries.

---

## Disposition Webhook {#disposition-webhook}

External systems (call centers, buyers) can update lead dispositions via the inbound disposition webhook.

### How It Works

<!-- react-flow:disposition-webhook -->

### Disposition Codes {#disposition-codes}

Dispositions are managed per **Vertical Group** on the Vertical Group detail page ([/admin/vertical-groups](/admin/vertical-groups)). Each vertical group has its own set of disposition codes with:
- **Label**: Display name (e.g., "Sold", "Bad Number")
- **Slug**: Unique identifier (auto-generated from label)
- **Category**: working, converted, rejected, or returned
- **Triggers CPA**: Automatically fires CPA conversion when posted
- **Triggers Return**: Automatically creates a pending refund when posted

New tenants are seeded with 10 default dispositions: Contacted, Qualified, Appointment Set, Converted, Sold, Bad Number, Not Interested, Do Not Call, Duplicate, and Bad Data. Tenant admins can fully edit, add, or remove dispositions from the Vertical Group detail page.

### API Usage

```
POST /api/v1/dispositions
Authorization: Bearer lr_xxx

{
  "saleId": "uuid-of-the-sale",
  "disposition": "contacted",
  "agentName": "John Smith",
  "callDuration": 180,
  "notes": "Left voicemail",
  "externalId": "your-unique-ref-123"
}
```

The `externalId` field provides idempotency — posting the same `externalId` for a sale returns the original result without creating a duplicate.

**Disposition date (`dispositionDate`).** By default a disposition is timestamped with the moment LR *receives* the postback. To record the **actual event time** instead — useful for delayed or batched postbacks — include an optional `dispositionDate`:

- Accepts an **ISO-8601 datetime** (with offset, e.g. `2026-06-20T14:30:00Z` or `2026-06-20T14:30:00+05:00`) **or** a **date-only** `YYYY-MM-DD` (resolved to your tenant timezone's midnight).
- **Future dates are rejected** (`400`); omit it to keep the default receive-time behavior.
- Works on the postback receiver too, as a query param: `GET /api/v1/postback?saleId=…&disposition=sold&dispositionDate=2026-06-20`.
- Re-firing a logged postback preserves its original `dispositionDate`.

**Where it shows (admin).** On a lead's **Dispositions** tab, the Disposition History table has an **Event Date** column (the supplied `dispositionDate`) next to **Date** (when LR received the disposition); it's blank when none was supplied. This is **admin-only** — buyer/partner API responses do not include `dispositionDate` yet.

---

## People Hub {#people-hub}

The People Hub provides a unified view of every person in the system, aggregating their leads, calls, and revenue across all verticals and partners.

**Location:** [/admin/people](/admin/people)

### Accordion-by-Vertical Layout

The people list groups each person's activity by vertical in a collapsible accordion. Each vertical row shows lead count, call count, total revenue, and last activity date. Expand a vertical to see individual leads and calls for that person in that vertical.

### Person Detail

**Location:** [People](/admin/people) > Person Detail

**One save bar:** field edits on the Overview tab (name, contact, address, tags) feed a single floating save bar that appears when something changed and sends only the changed fields. Notes and merges apply immediately in their own controls.

Tabs:

- **Overview**: editable profile (name, email, phone, DOB, address, tags), activity stat cards, marketing attribution chips, and all known emails/phones
- **Leads**: all leads linked to this person, with status, consent, and revenue
- **Calls**: all calls associated with this person, with status, duration, and revenue
- **Timeline**: chronological feed of all activity (leads submitted, calls received, sales completed)
- **Sales**: every lead and call sale for this person, showing buyer, contract, price, and status
- **Notes**: free-form internal notes (saved immediately)
- **Merge**: duplicate suggestions with similarity scores; merging is confirm-gated and irreversible — it moves the other record's leads, emails, phones, and notes into this one
- **Activity**: audit log

Merged records show a banner linking to the surviving record.

### Person Linking & Matching {#person-linking}

Every lead and call that enters the system is linked to a **person record**. The person record is the single source of truth for a real human — their contact info, lead history, calls, sales, and revenue all live there. The system automatically decides whether an incoming lead belongs to someone already in the database or is a brand new person. This section explains exactly how that decision is made, step by step.

#### Why This Matters

Without smart person matching, the same human can end up as 5 different records because they used a different email, called from a different phone, or had their name submitted by an intake center. That leads to duplicate sales, inflated metrics, and confused buyers. The matching system solves this by intelligently linking records while being careful not to incorrectly merge two different people (like a mother and daughter who share a phone number).

#### Match Priority Setting {#match-priority}

The first decision a new tenant makes is: **which contact field should the system check first** when trying to find an existing person?

- **Email First** (default): The system checks email first. If no email match, it falls back to phone.
- **Phone First**: The system checks phone first. If no phone match, it falls back to email.

**When to choose each:**

| Choose Email First if... | Choose Phone First if... |
|---|---|
| Most of your leads come via web forms with email | Most of your leads come via phone calls or SMS |
| Your verticals rely on email as the primary identifier | People in your vertical change emails frequently |
| You rarely see shared email addresses | You rarely see shared phone numbers (no household lines) |

This setting is chosen during first admin login via a one-time setup modal and is **permanent** — it cannot be changed after selection. This is because changing it mid-stream would cause the same person to match differently on new leads vs. old ones, creating data inconsistencies. You can view your current setting in **Settings > People**.

For **calls**, only phone matching is used regardless of this setting (calls don't carry email addresses).

The following diagram shows the full person matching decision flow, including confidence checks and case splits.

<!-- react-flow:person-matching -->

#### How Matching Works — Step by Step

When a new lead arrives, the system runs this sequence:

**Step 1 — Primary match.** Check the primary field (email or phone, based on your priority setting) against all existing person records. If a match is found, go to Step 3.

**Step 2 — Fallback match.** If the primary field didn't match (or the lead doesn't have that field), check the other field. If a match is found, go to Step 3. If neither field matches, skip to Step 4.

**Step 3 — Confidence check.** Before linking, the system evaluates how confident it is that this is truly the same person. See [Confidence-Based Linking](#confidence-linking) below. If confidence is high, the lead is linked to the existing person. If confidence is low, a new person is created instead (and both records are flagged as related).

**Step 4 — New person.** No match found. A new person record is created with the lead's contact info.

**For calls**, the sequence is slightly different because calls only have a phone number:
1. If the call already carries a `personId` (from a prior lead), use that
2. If the call is linked to a lead that has a `personId`, use that
3. Check phone against existing person records
4. If no match, create a new person

#### Confidence-Based Linking {#confidence-linking}

This is where the system gets smart. Not every "match" means the lead belongs to that person. The confidence check prevents the system from merging records that share contact info but are actually different people.

**The core rule:** If a phone number matches an existing person, but that person already has a *different* email address on file, the system does NOT link them. Instead, it creates a new person. This avoids the most common mistake in lead routing — merging a mother and daughter (or husband and wife, or business partners) who share a home/office phone.

**Full confidence matrix:**

| Match Type | Person's Existing Data | Incoming Lead Data | Confidence | What Happens |
|---|---|---|---|---|
| Phone match | Person has no email | Lead has an email | **High** | Link to person. Add email as enrichment. |
| Phone match | Person has `john@example.com` | Lead has `john@example.com` | **High** | Link to person. Same email confirms identity. |
| Phone match | Person has `john@example.com` | Lead has `jane@example.com` | **Low** | New person created. Different email = likely different person. Both flagged as Related. |
| Phone match | Person has `john@example.com` | Lead has no email | **High** | Link to person. No conflicting data. |
| Email match | Person has `555-1234` | Lead has `555-9999` | **High** | Link to person. Add new phone as secondary. People commonly have multiple phones. |
| Email match | Person has no phone | Lead has `555-1234` | **High** | Link to person. Add phone. |
| Email match | Person has `555-1234` | Lead has `555-1234` | **High** | Link to person. No new info needed. |
| No match | — | — | — | New person created. |

**Why email matches are always high confidence:** Email addresses are almost always unique to one person. If two leads share an email, they're almost certainly the same person — even if the phone numbers differ (people change phones, use work vs. personal, etc.). Phone numbers, on the other hand, are frequently shared (household landlines, office numbers, a parent's cell used by their kid).

#### Multiple Emails & Phones per Person

Each person can accumulate multiple email addresses and phone numbers over time. When a high-confidence match occurs and the lead carries contact info not yet on file, it's **automatically added** as a secondary email or phone. This enrichment happens silently — no admin action required.

**Example:** Person A is created with `john@gmail.com`. A month later, a lead arrives with `john@work.com` and the same phone. Email match on the phone → high confidence → link. Now Person A has two emails: `john@gmail.com` and `john@work.com`.

#### Complete Scenario Reference {#scenario-reference}

These walkthroughs cover every common situation you'll encounter. Each one traces the exact system behavior from lead arrival through person resolution.

---

**Scenario 1: Simple repeat lead (same email, same phone)**

> John submitted a lead last month with `john@example.com` and `555-1234`. He submits again today with the same info.

1. Lead arrives: `john@example.com`, `555-1234`, "John Smith"
2. Primary check (email): matches Person "John Smith"
3. Confidence: email match → always high
4. Result: **Lead linked to existing Person "John Smith".** No new contact info added. Revenue and lead count increment on the existing record.

---

**Scenario 2: Same person, new phone number**

> John got a new cell phone and submits a lead with the same email but a new number.

1. Lead arrives: `john@example.com`, `555-9999`, "John Smith"
2. Primary check (email): matches Person "John Smith" (has `555-1234`)
3. Confidence: email match → always high, even with a different phone
4. Result: **Lead linked to existing person. `555-9999` added as a secondary phone.** Person now has two phone numbers.

---

**Scenario 3: Call-first, then lead enrichment**

> A call comes in from an unknown number. Later, that same person submits a web form with their email.

1. **Call** arrives from `555-1234` → no match → **Person A created** (phone only, no email, no name)
2. **Lead** arrives: `john@example.com`, `555-1234`, "John Smith"
3. Primary check (email): no match (Person A has no email)
4. Fallback check (phone): matches Person A
5. Confidence: phone match + Person A has no email → **high confidence**
6. Result: **Lead linked to Person A. Email added. Name updated.** Person A is now "John Smith" with `john@example.com` + `555-1234`.

This is the most common enrichment pattern for businesses that receive calls before web leads.

---

**Scenario 4: Mother and daughter share a phone (the key use case)**

> Jane (mom) submitted a lead using the home phone. Later, her daughter Sarah submits a lead with the same phone but her own email.

1. **Lead 1** arrives: `jane@example.com`, `555-1234`, "Jane Smith"
   - No match → **Person A "Jane Smith"** created with `jane@example.com` + `555-1234`

2. **Lead 2** arrives: `sarah@example.com`, `555-1234`, "Sarah Smith"
   - Primary check (email): `sarah@example.com` → no match
   - Fallback check (phone): `555-1234` → matches Person A "Jane Smith"
   - Confidence check: Person A already has `jane@example.com`, incoming email is `sarah@example.com` → **different emails → LOW confidence**
   - Result: **New Person B "Sarah Smith" created** with `sarah@example.com` + `555-1234`

3. **What admins see:**
   - People list: two separate records — Jane Smith and Sarah Smith
   - Both have the "Related" badge (shared phone `555-1234`)
   - Using the "Shared contacts only" filter shows them grouped together
   - Admin can review and confirm they're correctly separated — or merge if it was actually the same person with two emails

**Why this matters:** Without the confidence check, Sarah's lead would have been merged into Jane's person record. Jane's lead count and revenue would be inflated, Sarah would have no record, and the buyer would see incorrect data. The confidence check prevents this automatically.

---

**Scenario 5: Husband and wife share email (rare but possible)**

> A couple shares a joint email address but has separate phone numbers.

1. **Lead 1** arrives: `smithfamily@example.com`, `555-1111`, "Robert Smith"
   - No match → **Person A "Robert Smith"** created

2. **Lead 2** arrives: `smithfamily@example.com`, `555-2222`, "Linda Smith"
   - Primary check (email): matches Person A "Robert Smith"
   - Confidence: email match → **always high** (email matches are trusted)
   - Result: **Lead linked to Person A. `555-2222` added as secondary phone.**

3. **The problem:** Both leads are now on "Robert Smith." Linda doesn't have her own person record.

4. **Admin resolution:** The admin sees Person A has two phones and two different lead names. They can:
   - Use the People list to find the record
   - Manually create a Person B for Linda
   - Reassign Linda's lead to the new person record
   - This is intentionally a manual process — the system errs on the side of linking (since shared emails are rare) rather than creating duplicates

**Note:** If shared email addresses are common in your vertical, consider using **Phone First** priority. That way, different phone numbers would be the primary check, and Robert and Linda would get separate records from the start.

---

**Scenario 6: Lead arrives with phone only (no email)**

> A partner's web form only collects phone numbers, not email.

1. Lead arrives: no email, `555-1234`, "John Smith"
2. Primary check (email): skipped (no email on lead)
3. Fallback check (phone): `555-1234` → matches Person A
4. Confidence: phone match + lead has no email → **high confidence** (no conflicting email to worry about)
5. Result: **Lead linked to Person A.**

---

**Scenario 7: Lead arrives with email only (no phone)**

> An online-only partner collects email but not phone.

1. Lead arrives: `john@example.com`, no phone, "John Smith"
2. Primary check (email): matches Person A
3. Confidence: email match → always high
4. Result: **Lead linked to Person A.**

---

**Scenario 8: Same phone matches Person A, same email matches Person B**

> Due to previous leads, the phone and email point to different person records.

1. Database state: Person A has `555-1234`. Person B has `john@example.com`.
2. Lead arrives: `john@example.com`, `555-1234`, "John Smith"
3. Primary check (email, assuming Email First): matches **Person B**
4. Since primary matched, fallback is skipped entirely
5. Result: **Lead linked to Person B. `555-1234` added as secondary phone on Person B.**

Person A and Person B now share `555-1234` and will show as Related People. The admin can investigate and merge if appropriate.

If the tenant used **Phone First** instead, Step 3 would match Person A first, and the email would go through the confidence check before linking.

---

**Scenario 9: Office phone shared by coworkers**

> Three employees at the same company submit leads using the office phone `555-0000` but with individual emails.

1. **Lead 1**: `alice@company.com`, `555-0000` → **Person A "Alice"** created
2. **Lead 2**: `bob@company.com`, `555-0000` → Phone matches Person A, but Person A has `alice@company.com` → **low confidence** → **Person B "Bob"** created
3. **Lead 3**: `carol@company.com`, `555-0000` → Phone matches Person A (or B), existing person has a different email → **low confidence** → **Person C "Carol"** created

Result: three separate person records, all showing as Related via shared phone `555-0000`. The system correctly identified them as different people despite sharing a number.

---

**Scenario 10: Person changes their email address**

> John originally used `john@oldcompany.com` and now submits with `john@newjob.com` but the same phone.

1. Database state: Person A "John Smith" has `john@oldcompany.com` + `555-1234`
2. Lead arrives: `john@newjob.com`, `555-1234`
3. Primary check (email): `john@newjob.com` → no match
4. Fallback check (phone): `555-1234` → matches Person A
5. Confidence: Person A already has `john@oldcompany.com`, incoming is `john@newjob.com` → **different emails → LOW confidence**
6. Result: **New Person B created** with `john@newjob.com` + `555-1234`

**This is a false separation** — it's the same John, he just changed jobs. The admin sees both records as Related (shared phone), reviews them, and merges Person B into Person A. After the merge, Person A has both emails and all leads.

**This is by design.** The system cannot know whether `john@newjob.com` is the same John or John's wife who borrowed his phone. It's safer to separate and let the admin merge than to auto-merge and combine two different people's data. The Related People filter makes finding these cases easy.

#### Conflict Resolution Summary

| Scenario | What Happens |
|---|---|
| Same email, same phone | Links to existing person. No new contact info added. |
| Same email, different phone | Links to existing person (email match is high confidence). New phone added as secondary. |
| Same phone, no existing email on person | Links to existing person. Email added as enrichment. |
| Same phone, person already has a different email | **New person created.** Both people flagged as Related. |
| Email matches Person A, phone matches Person B | Priority setting decides which match wins. The other person gets flagged as Related. |
| No email or phone match | New person created with the lead's contact info. |

#### Related People {#related-people}

When person records share contact info (same phone or email across different people), they appear in the **Related People** filter on the People list. This is the admin's tool for reviewing the system's matching decisions and fixing any cases where records should be merged or separated.

**How people become related:**
- The confidence check creates a new person instead of linking (e.g., mother/daughter with shared phone)
- A case split spawns a new person with shared contact info
- Two partners submit leads for different people who happen to share a phone
- The same person submitted leads with different emails over time

**To find related people:**
1. Go to **People** list
2. Open **Advanced Filters**
3. Enable **"Shared contacts only"**
4. People with overlapping contact info are shown with a **"Related" badge**
5. Click the badge to see who they're related to and which contacts are shared (email, phone, or both)

**Common patterns you'll see:**
- **Mother/daughter, husband/wife** — shared home phone, different emails. Usually correctly separated. Leave as-is.
- **Same person, changed email** — shared phone, different emails. Should be merged.
- **Office coworkers** — shared office phone, different emails. Usually correctly separated.
- **Same person, changed phone** — shared email, different phones. Already auto-merged by the system (email match is always high confidence), so you won't see this in Related People.

#### Merging Related People

From the Related People view, you can merge records directly:
1. Find the related people using the "Shared contacts only" filter
2. Click the **"Related" badge** to expand the related records
3. Click **"Merge"** next to the person you want to merge into the current record
4. Review the merge preview and **confirm**

**What happens when you merge two person records:**
- All leads, calls, sales, emails, phones, and notes move to the **survivor** record
- Duplicate emails/phones are silently skipped (no duplicates created)
- The merged record is soft-deleted and marked as merged, pointing to the survivor
- A merge log entry is created for audit purposes
- Activity counters (lead count, call count, revenue) on the survivor are recalculated
- The merge is **permanent** — it cannot be undone. Review carefully before confirming.

**When to merge:** The records represent the same real person (e.g., same person with old vs. new email).
**When NOT to merge:** The records represent different people who share contact info (e.g., mother and daughter sharing a phone). Leave them as separate related records.

### Automated Case Splits {#case-splits}

Case splits handle a specific real-world scenario: a buyer's intake center discovers that a single lead actually represents multiple qualified people. Instead of losing track of the additional cases, the system automatically spawns new lead and person records.

#### When Case Splits Happen

Case splits are triggered during **disposition postbacks** — the API call that an intake center makes to report the outcome of a lead they received. A case split fires when the disposition includes:
- A **new external ID** (a case ID that was never seen before on this sale)
- A **different first or last name** than the original lead

Both conditions must be true. A new external ID with the same name is just a normal disposition update. The same external ID with a different name is also a normal update (intake corrected the name).

#### What the System Does Automatically

When a case split is detected:

1. **Creates a new lead** — copies the parent lead's contact info (email, phone, address, all custom fields) but replaces the name with the new name from the disposition
2. **Runs person resolution** — the new lead goes through the same matching + confidence logic described above. This determines whether the new person is linked to the existing person or gets their own record
3. **Creates a new lead sale** — for the same buyer and contract, with intake's case ID stored as the `buyerLeadId` so both systems stay in sync
4. **Returns the result** — the API response includes `caseSplit: true` plus the IDs of the new lead and sale

#### Use Case 1: Mother and Daughter — Shared Email (Most Common)

**Setup:** Mom calls a TV ad and submits a lead using her email and home phone. Intake qualifies her, then discovers her daughter also qualifies.

| Step | What Happens | System State |
|---|---|---|
| 1. Mom submits lead | `jane@example.com`, `555-1234`, "Jane Smith" routes to intake | Lead A → Person A "Jane Smith" |
| 2. Intake qualifies mom | Posts disposition: `externalId: "CASE-001"`, `firstName: "Jane"` | Normal disposition. Name matches. No case split. |
| 3. Intake discovers daughter | Posts disposition: `externalId: "CASE-002"`, `firstName: "Sarah"`, `lastName: "Smith"` | **Case split triggered** |
| 4. System spawns lead | New Lead B: "Sarah Smith", `jane@example.com`, `555-1234` (copied from parent), `spawnedFromLeadId` → Lead A | Lead B created |
| 5. Person resolution runs | Email `jane@example.com` matches Person A "Jane Smith". Email match = always high confidence. The matching engine matches on **contact info, not name** — so Lead B links to Person A. | Lead B linked to Person A "Jane Smith" |
| 6. Final state | Person A "Jane Smith": has both Lead A (CASE-001) and Lead B (CASE-002). Both sales tracked separately on the buyer side. | One person, two leads, two sales |

**Important:** Because the spawned lead inherits mom's email and phone, and email matches are always high confidence, the system links Sarah's lead to mom's person record. The system cannot distinguish mother from daughter when they share the same email — it trusts email as a strong identifier.

**What the admin sees:**
- Lead A detail page: "Spawned Leads (Case Splits)" section showing Lead B
- Lead B detail page: blue "Spawned from Lead A" banner with "Sarah Smith" as the lead name
- Person A detail page: two leads — one for Jane (CASE-001), one for Sarah (CASE-002)
- Buyer: two separate sales in their dashboard, each with the correct case ID

**Admin action (optional):** If the admin wants Sarah as a separate person, they can manually create Person B and reassign Lead B. For most businesses, having both leads on one person record is acceptable since the sales are tracked separately.

#### Use Case 1b: Mother and Daughter — Different Emails

**Setup:** Same situation, but Sarah has her own email address. This is where the confidence check really shines.

| Step | What Happens | System State |
|---|---|---|
| 1. Mom submits lead | `jane@example.com`, `555-1234`, "Jane Smith" routes to intake | Lead A → Person A "Jane Smith" |
| 2. Intake posts dispo for mom | `externalId: "CASE-001"`, `firstName: "Jane"` | Normal disposition |
| 3. Intake discovers daughter | `externalId: "CASE-002"`, `firstName: "Sarah"`, `lastName: "Smith"` | **Case split triggered** |
| 4. System spawns lead | Lead B: "Sarah Smith", `jane@example.com`, `555-1234` (copied from parent) | Lead B links to Person A (same email) |
| 5. Sarah later submits her own lead | `sarah@example.com`, `555-1234`, "Sarah Smith" | New lead arrives with different email |
| 6. Person resolution | Email: `sarah@example.com` → no match. Phone: `555-1234` → matches Person A. Confidence: Person A has `jane@example.com`, incoming is `sarah@example.com` → **LOW confidence** | **Person B "Sarah Smith" created** |
| 7. Final state | Person A "Jane Smith": Lead A + Lead B. Person B "Sarah Smith": Lead C. Both share `555-1234` → Related People. | Two people, three leads |

Now the admin has both people visible as Related and can reassign Lead B from Person A to Person B if they want to clean up.

#### Use Case 2: Business Partner Referral

**Setup:** A legal intake center receives a lead for a car accident. During intake, they learn the lead's passenger also wants to file a claim.

| Step | What Happens |
|---|---|
| 1. Lead submitted | "Robert Johnson", `robert@email.com`, `555-4444` → Lead A → Person A |
| 2. Intake qualifies Robert | Disposition: `externalId: "INJ-100"`, `firstName: "Robert"` → normal |
| 3. Intake finds passenger | Disposition: `externalId: "INJ-101"`, `firstName: "Maria"`, `lastName: "Garcia"` → **case split** |
| 4. System spawns | Lead B: "Maria Garcia", `robert@email.com`, `555-4444` (copied from parent) |
| 5. Person resolution | Person B "Maria Garcia" created (different name = new person from case split) |
| 6. Admin review | Both people share contact info from the original lead. Admin may want to update Maria's actual contact info later. Both people appear as Related. |

#### Use Case 3: Family Plan (Three Cases from One Lead)

**Setup:** An insurance lead turns out to represent a family — dad, mom, and adult child all need separate policies.

| Step | Action | Result |
|---|---|---|
| 1. | Dad submits lead: "James Wilson", `jwilson@email.com`, `555-7777` | Lead A → Person A "James Wilson" |
| 2. | Intake: `externalId: "POL-A"`, `firstName: "James"` | Normal dispo |
| 3. | Intake: `externalId: "POL-B"`, `firstName: "Mary"`, `lastName: "Wilson"` | Case split → Lead B → Person B "Mary Wilson" |
| 4. | Intake: `externalId: "POL-C"`, `firstName: "Tyler"`, `lastName: "Wilson"` | Case split → Lead C → Person C "Tyler Wilson" |

Final state: 3 leads, 3 people, 3 sales — all tracked separately but visibly related. Parent lead (Lead A) shows both spawned leads. All three people share `555-7777` and `jwilson@email.com` as Related People.

#### Use Case 4: Duplicate Case ID (No Case Split)

**Setup:** Intake posts the same case ID twice — maybe a retry or an update.

| Step | Action | Result |
|---|---|---|
| 1. | Lead: "Jane Smith", routes to intake | Lead A → Person A |
| 2. | Intake: `externalId: "CASE-X"`, `firstName: "Jane"` | Normal dispo |
| 3. | Intake retries: `externalId: "CASE-X"`, `firstName: "Jane"` | Same externalId, same name → **no case split**, just a dispo update |

The system only spawns when it sees a truly *new* external ID with a different name. Retries and updates are handled normally.

#### Viewing Case Splits in the Admin UI

On the **parent lead detail page**:
- A **"Spawned Leads (Case Splits)"** section appears below the lead details
- Each spawned lead is listed with its name, case ID, and a link to its detail page
- The spawned lead count is visible at a glance

On the **spawned lead detail page**:
- A blue **"Spawned from"** banner appears at the top, linking back to the parent lead
- All other details (person, sales, dispositions) work normally

In the **People list**:
- People created from case splits appear as normal person records
- They share contact info with the original person and show up as **Related People**
- The admin can review, leave as-is, or merge if appropriate

#### Disposition API Payload for Case Splits

To trigger a case split, include `firstName` and/or `lastName` in the disposition postback along with a new `externalId`:

```json
{
  "saleId": "uuid-of-original-sale",
  "disposition": "QUALIFIED",
  "externalId": "CASE-B",
  "firstName": "Sarah",
  "lastName": "Smith"
}
```

**Rules:**
- `externalId` must be a value never previously used on this sale
- `firstName` or `lastName` (or both) must differ from the original lead's name
- If the name matches the original lead, no case split occurs — it's treated as a normal disposition update
- The `saleId` must reference an existing lead sale
- The spawned lead inherits the parent's email, phone, address, and all custom field values

#### Putting It All Together — The Complete Lifecycle

Here's how matching, confidence, related people, and case splits work together in a real scenario:

1. **Call arrives** from `555-1234` → no match → Person A created (phone only, no email, no name).
2. **Web lead** arrives: `jane@example.com`, `555-1234`, "Jane Smith" → email: no match. Phone: matches Person A, no email on Person A → **high confidence** → linked. Person A now has email + phone + name.
3. **Lead routes** to intake center. Intake qualifies Jane, posts disposition with `externalId: "CASE-001"`, `firstName: "Jane"` → name matches → normal disposition.
4. Intake discovers **daughter Sarah** also qualifies → posts disposition with `externalId: "CASE-002"`, `firstName: "Sarah"` → **case split fires**.
5. System spawns Lead B "Sarah Smith" with mom's contact info (`jane@example.com`, `555-1234`). Person resolution: email `jane@example.com` matches Person A → high confidence → **Lead B links to Person A.** (Both leads on one person — the system can't distinguish mother/daughter when they share an email.)
6. Weeks later, **Sarah submits her own lead**: `sarah@personal.com`, `555-1234`, "Sarah Smith" → email: no match. Phone: matches Person A, but Person A has `jane@example.com` ≠ `sarah@personal.com` → **low confidence** → **Person B "Sarah Smith" created** with `sarah@personal.com` + `555-1234`.
7. **Admin reviews** Related People: Person A "Jane Smith" and Person B "Sarah Smith" share `555-1234`. Admin sees Lead B on Person A was for Sarah → optionally **reassigns Lead B to Person B** for cleaner records.
8. Months later, **another lead** arrives: `jane.smith.new@gmail.com`, `555-1234` → email: no match. Phone: matches Person A (has `jane@example.com`) and Person B (has `sarah@personal.com`). Both have different emails → **low confidence** on both → **Person C created**. Admin checks Related People, sees it's Jane with a new email, **merges Person C into Person A**.

The system handles the complexity automatically while giving admins full visibility and control through Related People and the merge tools.

The system handles the complexity automatically while giving admins full visibility and control through Related People and the merge tools.

### Where Dedup Can Happen {#dedup-map}

A lead passes through several checkpoints before it's sold. Any of them can stop it for being a duplicate (or opted-out, which behaves the same way from a partner's perspective). They run in this order:

#### 1. Before the lead is saved

**Suppression list.** If the phone or email is on the opt-out list, the lead is rejected at the door. No lead record is created. Visible in the posting log only.

**Vertical-level dedup.** Each vertical can reject leads where the same email/phone/name/etc. already exists in that vertical within a window (default 30 days). This fires *before* a lead record is created — so there's no row in the lead list and no distribution waterfall. Visible in the posting log only. Configured per vertical under **Verticals > [vertical] > Duplicate Settings**.

#### 2. After the lead is saved, before routing runs

**Cross-partner dedup.** The lead record exists. We check: has this same *person* (matched by email/phone) sent a lead to this vertical from *any* partner recently? If yes, rejected before any contracts are evaluated. A "Regular routing — skipped" row shows in the distribution waterfall so you can see why nothing else ran. Global toggle at **Settings > Deduplication**; per-offer override on the offer detail page.

**Enrichment rejection.** Not dedup — but rejects at the same point. Blacklist Alliance, IPQS fraud, and other enrichment providers can reject based on fraud score, blacklist hit, etc. Lead is created, marked rejected, and DQ is attempted. Not treated as a duplicate.

#### 3. During routing — per buyer

**Buyer's own dedup.** Each buyer can opt in to dedup — "don't sell me the same email or phone twice within N days." When the routing engine evaluates contracts, it checks each buyer's dedup history (`leadDedup` table). If this buyer has seen this lead recently, that contract gets skipped with reason "Duplicate Lead". Other buyers who haven't seen it still get their shot. Configured on **Buyers > [buyer] > Deduplication Settings**.

**Fuzzy dedup fallback.** Optional. When the exact-match check misses, a fuzzy check compares normalized email (Gmail dots stripped, plus-tags removed), normalized phone (country prefix stripped), and optional name+address. Same per-buyer behavior. Global toggle under **Settings > Deduplication > Fuzzy Enabled**.

#### 4. In the DQ flow

**Primary-was-duplicate short-circuit.** If the primary path rejected the lead for any duplicate reason (cross-partner dedup in step 2, or the buyer's API saying "duplicate" in step 5 below), DQ links with **Apply Dedup** checked are skipped with reason "Primary Duplicate — DQ Skipped". This prevents selling a known-duplicate lead to the DQ buyer.

**DQ buyer's own dedup.** Even when the primary wasn't a duplicate, if **Apply Dedup** is checked on the DQ link, the DQ buyer's own dedup history is still checked — so a buyer who's on both the primary and DQ side won't receive the same lead twice.

Configured per DQ contract link on **Offers > [offer] > DQ Contracts**.

#### 5. After delivery — the buyer's API rejects it

**Primary buyer returns "duplicate" outcome.** After successful delivery, the buyer's API response is mapped (via response mapping rules) to an outcome. When that outcome is `duplicate`, the lead is marked rejected and DQ is attempted. DQ links with **Apply Dedup** checked are skipped by the same short-circuit as step 4.

#### 6. Engagement attribution (only when Engagement mode is enabled)

When the **useEngagementDedup** system setting is on, cross-partner dedup (step 2) is replaced by **engagement attribution**. Instead of a global "is this person a duplicate" check, every potential sale is evaluated per-buyer against an attribution policy — "does this lead belong to partner A or partner B based on who first brought the person in?" Per-sale rejections show up as `leadSale.disposition = 'rejected'` with reason "Engagement attribution rejected". This is a more nuanced alternative to cross-partner dedup; most tenants use one or the other, not both.

#### What to do when you see a duplicate you didn't expect

1. Open the lead detail page and look at the **Distributions** tab (waterfall).
2. If the waterfall is empty → rejection happened in step 1 (pre-insert). Check the **Posting Log** — it captures vertical-level dedup and suppression rejections.
3. If the waterfall has a **"Regular routing — skipped"** row → cross-partner dedup (step 2) fired. The reason column tells you which window/vertical matched.
4. If the waterfall shows individual contracts with disposition `skipped` and reason "Duplicate Lead" → buyer-level dedup (step 3).
5. If a DQ row shows reason "Primary Duplicate — DQ Skipped" → the fix from step 4 kicked in.
6. If a DQ row shows reason "DQ Duplicate Lead" → the DQ buyer's own dedup caught it.

### Cross-Partner Deduplication {#cross-partner-dedup}

Cross-partner dedup prevents the same person from being sold multiple times when submitted by different partners. The engine matches on person identity (email + phone) within a vertical.

**Global settings** (Settings > Deduplication):
- **Cross-Partner Dedup Enabled**: Master toggle. When on, leads from different partners for the same person+vertical are rejected.
- **Lookback Window (days)**: How many days back to check for prior activity. Default 30.
- **Include Calls**: Whether inbound calls count as prior activity for dedup purposes.

### Per-Offer Dedup Overrides {#offer-dedup-override}

Each offer can override the global dedup settings:
- **Cross-Partner Dedup**: Override the global toggle for this offer (enable, disable, or inherit global)
- **Lookback Window**: Override the global window for this offer
- **Include Calls**: Override whether calls count for this offer

Set these on the Offer Detail page under the dedup section.

The offer's Dedup tab also surfaces a read-only **Vertical Dedup Settings** panel showing the underlying vertical config (enabled, fields checked, window, AND/OR condition, dupe-against scope) and the verticalGroup's dedupScope. Edits happen on the vertical page — this view is for visibility into the full hierarchy: vertical fields + window → buyer dedup → contract override.

### Multi-Source Badge

When a person has been submitted by more than one partner, a "Multi-Source" badge appears on their record. This is visible to admins only and helps identify shared leads across your partner base.

### Partner Portal — People

**Location:** [/partner/people](/partner/people)

Partners see a scoped view of people associated with their leads and calls. The same accordion-by-vertical layout is used, but limited to the partner's own activity. Partners cannot see the multi-source badge or other partners' data.

---

## Call Tracking {#call-tracking}

The call tracking system routes inbound phone calls to buyers using the same contract evaluation engine as lead routing. Calls are matched to tracking numbers, optionally processed through an IVR menu, then bridged to eligible buyers.

**Live header stats.** When Call Tracking is enabled for your tenant, the admin top bar shows four at-a-glance chips that refresh every 30 seconds (and pause while the browser tab is in the background): **Live** — calls currently in progress (ringing, in an IVR, routing, on hold, or connected), with a green pulse when one or more calls are live; **Leads** — leads created today; **Sold** — sold leads today; and **Calls** — every call received today. "Today" follows your tenant's timezone. The chips are hidden on small screens and disappear entirely for tenants without Call Tracking.

<!-- react-flow:call-flow -->

### Call Readiness Check {#call-readiness}

Every call-enabled campaign shows a **Call Readiness** card in the Call panel of the campaign detail page (below the DNI settings). It answers one question: *could an inbound call to this campaign actually complete right now?* The card runs the same gates the routing engine uses, so a **Ready** verdict means a call would connect end-to-end. It reports one of three states — **Ready** (green), **N warnings** (yellow, calls work but something needs attention), or **Blocked** (red, a call cannot complete) — and expands to a checklist explaining each gate:

- **Campaign active** — the campaign is active and has calls enabled.
- **Tracking number** — an assigned tracking number exists so calls can reach the campaign.
- **Offer linked** — an active offer is linked (warns if the tracking number points to a different offer than the campaign).
- **Telephony provider** — provider credentials resolve for the tenant (warns when inheriting shared platform credentials).
- **Call-capable contracts** — at least one linked contract is active, call-enabled, has a transfer number, and belongs to an active buyer (failures list the exact breakdown).
- **Within schedule** — at least one call-capable contract is inside its hours right now (warning only — hours are temporary).
- **Funding** — at least one call-capable contract has prepaid SKU inventory or a matching call billing rule.
- **Daily caps** — not every call-capable contract has hit its daily cap (warning only).
- **DNI pool** — when Dynamic Number Insertion is on, pool numbers are available or auto-buy is enabled.

Use **Refresh** to re-run the check after making changes.

### Tracking Numbers {#tracking-numbers}

**Location:** [/admin/call-settings?tab=phone-numbers](/admin/call-settings?tab=phone-numbers) (Calls > Phone Numbers)

Tracking numbers are phone numbers purchased from a telephony provider (SignalWire, Twilio, Telnyx, or Bandwidth) and assigned to a campaign and offer. When a caller dials a tracking number, the system looks up the associated offer and evaluates contracts. They are managed on the merged **Phone Numbers** inventory (see [Number Pool Management](#number-pool-management)).

**Creating a Tracking Number:**
1. Go to **Calls > Phone Numbers** and click **Add Number**
2. Select a provider (defaults to **SignalWire**, the primary number provider; Twilio, Telnyx, and Bandwidth remain available)
3. Search for available numbers by area code or region
4. Purchase the number
5. Assign it to a campaign and offer
6. Optionally attach an IVR configuration

**Monthly Billing:**
Each tracking number a tenant owns bills **150 credits per number per month** (the `credits.tracking_number` rate, editable by a superadmin under **Billing → Service Pricing**). A monthly cron (`/api/cron/tracking-number-billing`, runs on the 1st of each month) charges every **billable** number once for that calendar month. Billable means the tenant currently owns the number: statuses **`assigned`** (bound to a campaign) and **`available`** (provisioned and held in the pool). Numbers that are **`released`** (returned to the carrier) or **`provisioning`** (not yet live) are not billed. Billing is idempotent per number per month — a re-run never double-charges. `billingExempt` tenants get a 0-credit usage row instead of a debit; a tenant without enough credits is skipped for that number with a `tracking_number_billing_failed` audit entry (no debit, no auto-release in this version).

#### Twilio Call Tracking Setup {#twilio-calls-setup}

**Step 1: Get Twilio Credentials**
- If you already set up Twilio for SMS, you can use the same Account SID and Auth Token
- If not, go to twilio.com > Console Dashboard
- Copy your Account SID and Auth Token

**Step 2: Set Environment Variables**
These go in your Vercel environment (not in-app provider config):
- `TWILIO_ACCOUNT_SID` — Your Account SID (starts with AC)
- `TWILIO_AUTH_TOKEN` — Your Auth Token
- Optional: `TWILIO_API_KEY_SID` and `TWILIO_API_KEY_SECRET` for sub-account access

**Step 3: Configure Voice Webhook**
- When you purchase a tracking number through iSCALE, the webhook is auto-configured
- If configuring manually: Phone Numbers > your number > Voice & Fax > "A CALL COMES IN":
  - Webhook URL: `https://theleadrouter.com/api/v1/calls/webhooks/inbound`
  - HTTP Method: POST

**Step 4: Purchase a Tracking Number**
- In iSCALE, go to Admin > Tracking Numbers > New
- Select "Search & Purchase" mode
- Provider: Twilio
- Enter area code and number type (local/toll-free)
- Click Search — select from available numbers
- Assign to a Campaign and Offer
- Configure call handling (whisper, recording)
- Click Purchase

---

#### Telnyx Call Tracking Setup {#telnyx-calls-setup}

**Step 1: Create a Telnyx Account**
- Go to telnyx.com and sign up
- Add a payment method (required for number purchases)

**Step 2: Create an API Key**
- Go to telnyx.com > Auth > API Keys
- Click "Create API Key"
- Name: `iscale-calls`
- Copy the key (starts with KEY...)

**Step 3: Create a Call Control Application**
- Go to Voice > Call Control > Applications > Add New
- Name: `iscale-inbound`
- Webhook URL: `https://theleadrouter.com/api/v1/calls/webhooks/inbound`
- Note the Application ID (also called Connection ID)

**Step 4: Get Your Public Key (for webhook verification)**
- Go to Auth > Public Keys
- Copy your Ed25519 public key

**Step 5: Set Environment Variables**
- `TELNYX_API_KEY` — Your API key (starts with KEY...)
- `TELNYX_APP_ID` — Your Call Control Application ID
- `TELNYX_PUBLIC_KEY` — Your Ed25519 public key (for webhook signature verification)

**Step 6: Purchase a Tracking Number**
- In iSCALE, go to Admin > Tracking Numbers > New
- Select "Search & Purchase" mode
- Provider: Telnyx
- Enter area code and number type
- Click Search — select from available numbers
- Assign to Campaign and Offer
- Click Purchase

---

#### Bandwidth Call Tracking Setup {#bandwidth-calls-setup}

Bandwidth is a tier-1 CPaaS provider offering voice and messaging APIs with direct carrier connectivity. Bandwidth uses **BXML** (Bandwidth XML) for call control — structurally similar to TwiML.

**Prerequisites:**
- A Bandwidth account at [dashboard.bandwidth.com](https://dashboard.bandwidth.com)
- An API user with Voice API permissions
- A Voice Application created in the Bandwidth dashboard

**Step 1: Get API Credentials**
- Log into the Bandwidth dashboard
- Go to Account > API Users
- Note your **Account ID**, **API Token**, and **API Secret**

**Step 2: Create a Voice Application**
- In the Bandwidth dashboard, go to Applications > Voice
- Create a new Voice Application
- Set the **Callback URL** to: `https://theleadrouter.com/api/v1/calls/webhooks/inbound`
- Set **Callback Credentials** (username/password) — these are used for webhook authentication
- Note the **Application ID**

**Step 3: Configure Webhook Authentication**

Bandwidth uses HTTP Basic Auth with a **401 challenge-response** flow for webhooks:
1. Bandwidth sends the initial webhook request **without** an Authorization header
2. Your endpoint responds with `401 Unauthorized` + `WWW-Authenticate` header
3. Bandwidth retries the request **with** Basic Auth credentials (the callback username/password you configured)

iSCALE handles this automatically — just ensure the callback credentials in Bandwidth match your environment variables.

**Step 4: Set Environment Variables**
```
BANDWIDTH_ACCOUNT_ID=your-account-id
BANDWIDTH_API_TOKEN=your-api-token
BANDWIDTH_API_SECRET=your-api-secret
BANDWIDTH_VOICE_APP_ID=your-voice-application-id
BANDWIDTH_CALLBACK_USERNAME=your-callback-username
BANDWIDTH_CALLBACK_PASSWORD=your-callback-password
```

**Step 5: Purchase a Tracking Number**
- In iSCALE, go to Admin > Tracking Numbers > New
- Select "Search & Purchase" mode
- Provider: Bandwidth
- Enter area code and number type
- Click Search — select from available numbers
- Assign to Campaign and Offer
- Click Purchase

---

#### SignalWire Call Tracking Setup {#signalwire-calls-setup}

SignalWire is a Twilio-API-compatible CPaaS with materially cheaper DIDs. iSCALE supports it as a 4th telephony provider via the SignalWire LaML (Markup Language) API. Adding SignalWire **does not migrate existing numbers** — existing Twilio/Telnyx/Bandwidth numbers stay on their providers. Marking SignalWire as the default for the `numbers_purchase` capability routes only **new** pool number purchases through SignalWire.

**Prerequisites:**
- A SignalWire account at [signalwire.com](https://signalwire.com) (a Space is auto-created — host looks like `your-space.signalwire.com`)

**Step 1: Get API Credentials**
- Log into the SignalWire Dashboard
- Go to **API** > **API Tokens**
- Note your **Project ID** (UUID)
- Create or copy a **REST API Token** — used for all REST calls (purchase numbers, place calls, etc.)

**Step 2: Get a Webhook Signing Key (separate from the API token)**
- In the Dashboard, go to **API** > **Signing Keys** (sometimes labeled **Webhook Signing Keys**)
- Create a key — this is what SignalWire uses to sign the `X-SignalWire-Signature` header on inbound webhooks. **It is NOT the same as your REST API token.**
- iSCALE validates webhook signatures with this key; misconfiguring it will cause inbound webhooks to 401.

**Step 3: Note Your Space Host**
- Your Space host is the bare hostname **without** scheme — e.g. `iscale.signalwire.com` (NOT `https://iscale.signalwire.com`)
- iSCALE prepends `https://` automatically; entering a scheme will fail validation

**Step 4: Add the Credential in iSCALE**
- Go to Admin > System > Providers > **Add Provider**
- Provider: **SignalWire**
- Fill in: **Project ID**, **API Token**, **Webhook Signing Key**, **Space Host**
- Save — iSCALE encrypts all four secrets at rest (AES-256-GCM via the provider encryption key)

**Step 5: Mark as Default for New Number Purchases (optional)**
- On the new credential row, open the **Defaults** tab
- Set **Default = ON** for the `numbers_purchase` capability
- From now on, the next pool refill that triggers a number purchase will buy from SignalWire instead of Telnyx
- **Existing numbers remain on their original providers** — SignalWire only handles new buys

**Step 6: Purchase a Tracking Number**
- Admin > Tracking Numbers > New > "Search & Purchase"
- Provider: SignalWire
- Enter area code and number type → Search → Purchase
- Or let pool auto-refill use SignalWire if you set the default in Step 5

**Pricing note:** Verify per-DID pricing in your SignalWire console before flipping the default; rates vary by area code and country.

---

### IVR Configuration {#ivr-configuration}

**Location:** [/admin/calls/ivr-configs](/admin/calls/ivr-configs)

IVR (Interactive Voice Response) menus greet callers with a recorded message and collect DTMF keypress input before routing. Each IVR config defines:

- **Greeting**: Text-to-speech or audio URL played to the caller
- **Menu Options**: Map keypresses (1-9, 0, *) to actions (route to offer, play message, transfer)
- **Timeout**: Seconds to wait for input before fallback
- **Max Retries**: How many times to re-prompt on invalid input

### Call Routing and Buyer Evaluation {#call-routing}

<!-- react-flow:call-routing-flow -->

When a call arrives, the routing engine resolves the tracking number to an offer, then evaluates contracts through a priority chain:

#### 1. Lead Affinity (if enabled)

Matches the caller's phone number against person phone records linked to leads on this offer. Looks up all known phone numbers for each person (not just the lead's primary phone).

- **Off** — skip this step
- **Same Contract** — if a match is found, route directly to the contract that won the matched lead (if active and call-enabled). Routing is complete.
- **Different Contract** — if a match is found, exclude that contract from the distribution pool. Continue to next step.

#### 2. Caller Affinity (if enabled)

Checks if this caller has previously been routed on this offer by looking up prior call sale records.

- **Off** — skip this step
- **Same Contract** — route to the same contract as the previous call (if active and call-enabled). Routing is complete.
- **Different Contract** — exclude the previous call's contract from the distribution pool. Continue to next step.

Both affinity steps respect the **Affinity Lookback Window** — only matches within the configured number of days (default 30) are considered. Exclusions from both steps stack. Both affinity fast-paths also pass the [concurrency gate](#concurrency-caps) — a matched contract at its concurrency cap is skipped, not routed to.

#### 3. Standard Distribution (fallback)

Evaluates all remaining contracts (minus any excluded by affinity):
- Buyer status (active)
- Buyer balance (sufficient funds)
- Schedule (within operating hours)
- Geographic filters (caller area code / zip)
- Cap limits (daily/weekly/monthly call caps when cap mode is 'separate')
- Concurrency caps (live simultaneous-call limits per contract and per buyer — see [Concurrency Caps](#concurrency-caps))

Contracts are ranked by the offer's **Call Distribution Rule**:

| Rule | Behavior |
|------|----------|
| **Price** | Highest resolved call billing price wins, including matching call billing rules |
| **Priority** | Highest priority number wins |
| **Weight** | Random weighted selection |
| **Round Robin** | Rotates through contracts |
| **Simultaneous** | All eligible contracts ring at once |

**Distribution Type** controls how winners are attempted:
- **Sequential** — dial one contract at a time in ranked order, failing over to the next if unanswered or rejected. The waterfall is capped at **5 dial attempts per call**; eligible contracts beyond the cap are not dialed.
- **Non-Sequential** — attempt all eligible contracts at once

When Simultaneous is selected as the distribution rule, distribution type is automatically set to non-sequential and hidden.

### Whisper and Bridge {#whisper-and-bridge}

**Bridge**: The system connects the caller to the buyer's destination number via a two-leg call.

**Whisper**: If enabled on the contract, the buyer hears a whisper prompt before connecting with the caller. The buyer can:
- **Accept** (press 1) — call is bridged
- **Reject** (press any other key or hang up) — system fails over to the next buyer

If a buyer does not answer or rejects, the system attempts the next eligible buyer. If all buyers are exhausted (or the 5-dial-attempt cap is reached), the call is marked as missed.

### SIP Ingress — Partner-Delivered Calls Over SIP {#sip-ingress}

Partners with call centers can deliver calls over SIP instead of dialing a tracking
number. Their dialer sends a SIP INVITE to an address we issue
(`sip:<identity>@<SIP host>`); the identity in the address resolves to a tracking
number exactly the way a dialed number does, and everything downstream — routing,
whisper, bridging, recording, billing — is identical to a DID call.

<!-- react-flow:sip-ingress -->

**Setting it up (partner detail → SIP tab):**

1. **Provision** — creates the partner's SIP endpoint and mints digest credentials
   automatically. The password is displayed exactly once at creation; only an
   encrypted digest is stored, so copy it to the partner immediately.
2. **Authentication** — credentials by default. Expand the accordion to switch to an
   IP allowlist (for dialers with static addresses) or require both.
3. **Assign SIP identities** — a tracking number row with a SIP identity and no phone
   number. Identities are human-readable (e.g. `acme-mvasettlement`) and may never
   look like a phone number. A partner can also send an E.164 user part matching a
   tracking number they already own (DNIS-style).
4. **Activate** — refused until the endpoint has auth material matching its method.
   Endpoints are closed on arrival and stay closed until activated.

**What to know about SIP calls:**

- The call detail page shows an **Ingress: SIP** badge with the arrival domain.
- **Caller ID on a SIP call is supplied by the partner's dialer** — it does not carry
  carrier verification the way a dialed call does, and the call detail page says so.
- A SIP call addressed to an unknown identity, or to another partner's number, is
  rejected with a failed call row you can find in the Calls list (never silent dead
  air). The hangup cause names the reason (`SIP_IDENTITY_NOT_FOUND`,
  `SIP_CROSS_PARTNER_NUMBER`, `SIP_ENDPOINT_DISABLED`).

### Call RTB — Real-Time Bidding for Calls {#call-rtb}

Call RTB lets publishers *ping* a campaign before sending a call — "do you want this
caller, and what will you pay?" — and lets ping-post buyer contracts *bid* on a live
call while it rings. The feature is off by default at every layer: a superadmin
feature key (`callRtb`) per tenant, and a per-vertical eligibility toggle (default
off; TSR-restricted verticals are hard-blocked).

<!-- react-flow:call-rtb-ping-flow -->

**The publisher ping** (`POST /api/v1/calls/rtb/ping`, `calls_rtb` API-key scope) walks
the gates above and answers with either a quote — the campaign's configured call payout
and its terms — or a single `LR-1xx` reject code. The codes are deliberately coarse for
external counterparties (e.g. `LR-101` feature off, `LR-109` campaign not routable,
`LR-103` no eligible buyer); they never leak contract or buyer detail. The ping is
write-free and uses the same eligibility engine as live routing, so a quoted ping and an
arriving call always agree.

**The eligibility preview** (campaign detail → Calls tab) runs that same probe from the
admin side and shows everything the external response hides: each gate individually,
every contract's disposition with its reason and mapped code, and the payout that would
be quoted. Use it to answer "why is this campaign rejecting pings?" without spending a
ping or placing a call. It is write-free — nothing is logged, counted, or dialed.

**The live auction**: contracts priced as ping-post with a call-side ping URL become
bidders. When a call arrives, all bidders are pinged inside a hard deadline; a bid that
beats the best static price wins, its number (or SIP address) is dialed, and the quoted
price is frozen onto the leg — a mid-call config change never moves the settle. Ties,
timeouts, errors, unaffordable bids, and bids above the contract's max-payout ceiling
all fall back to static routing untouched. Every bidder's outcome, latency, and price
lands in the call's Routing Trace (`RTB Bid Winner (101ms)` / `RTB Bid: timeout`).

### Routing Trace (Call Detail) {#call-routing-trace}

The admin call detail page's **Routing** tab answers "why did this call go where it went" with a timeline card and three sections:

**Timeline** — a Gantt-style chart of the call on its real time axis, so the shape of the call reads at a glance before you look at any table. The top lane is the **Caller** leg (gray *waiting* until the call connects, then green *talking*), followed by one lane per dial attempt labelled with its contract name (amber *ringing*, then green *talking* if the buyer answered). Each lane ends with a short outcome label — *caller hung up*, *answered*, *no answer 4s*, *busy*. Hover any bar for the exact start and end timestamps in your tenant timezone. A talk segment drawn with a **diagonal hatch** means its start was calculated rather than observed — see *Why "unconfirmed"* under Dial Waterfall. Very short events stay visible as a minimum-width sliver even on a 20-minute call, so a 1-second ring on a long call is never invisible. The time ticks along the bottom adapt to the call's length: per-second for calls under 10 seconds, coarser humanized steps for longer ones. Attempts that were never dialed carry no timestamps and so have no lane — the Dial Waterfall table below covers them. The card is omitted entirely for old calls with no usable timestamps.

**1. Routing Decision** — one row per contract evaluated by the routing engine, with disposition (eligible/skipped/rejected), rejection reason, rank, price, and an expandable per-filter breakdown. This is the pre-dial decision log — it is sealed when routing runs and never changes afterward. Rows are grouped under two headers: **In dial stack** (eligible contracts, ordered by their stack rank) and **Evaluated — not eligible** (everything the engine considered and cut, with the reason why).

**2. Dial Waterfall** — the full ranked dial stack, one row per stack slot. Each row shows its **Stack #** (the contract's rank in the waterfall), and dialed slots add the attempt number, contract, buyer number, dialed-at time (tenant timezone), **"rang Xs of Ys"** (how long the buyer leg rang vs. the configured ring timeout for that attempt), the raw dial outcome badge, and a plain-English verdict. In-stack contracts that were **never reached** appear as muted rows with a gray **not dialed** badge and one of two verdicts: *an earlier attempt connected* (a higher-ranked buyer answered, so the waterfall stopped) or *call ended before this attempt* (the caller hung up or the call died before this slot's turn). Dialed-attempt verdicts:

| Verdict | Meaning |
|---|---|
| **answered after Xs — agent confirmed** | A person pressed 1 at the whisper prompt. This is the only verdict that proves a human took the call. |
| **carrier reported answer after ~Xs — unconfirmed** | The carrier said the line answered. Nobody confirmed it — see *Why "unconfirmed"* below. |
| **caller hung up while ringing (rang Xs of Ys)** | The *caller* abandoned before the buyer answered — the attempt ended well short of its timeout and the call died with it. Not a buyer miss. |
| **rang out — no answer after Xs** | The buyer let it ring for the full timeout and never picked up |
| **buyer line busy** | Buyer leg returned busy |
| **dial failed (cause)** | Carrier could not complete the dial |
| **no answer** | Unanswered, but not enough telemetry to distinguish abandon from rang-out (older calls) |

The abandon-vs-rang-out distinction matters when reviewing buyer performance: a caller who gives up 2 seconds into a 30-second ring is not the buyer's fault, and previously looked identical to a genuine 30-second rang-out.

**Why "unconfirmed", and what the tilde means.** For most providers the platform is only told the outcome of a dial *after* it finishes, never at the moment of pickup. It therefore works the answer time out backwards, by subtracting the talk duration the carrier reports from the moment the call ended. That arithmetic is only as honest as the carrier: some phone systems — forwarding services, PBXs, hunt groups — answer the incoming leg instantly and *then* start ringing a real phone, so the caller hears ringing while the carrier reports a long, healthy "conversation". The telemetry is identical to a genuine call.

So the tab labels what it actually knows:

- **`~1s`** (with a tilde) — the ring time is a leftover from that subtraction, not a measurement. Only the "of 30s" timeout beside it is exact, because that one is configured by you.
- **Hatched talk bar on the Timeline** — the same warning in visual form. A diagonal hatch over the green talk segment means the boundary between ringing and talking was calculated, not observed. Hover it for the explanation.
- **No tilde, no hatch, "agent confirmed"** — a human pressed 1. A machine cannot press 1, so this is real.

If your buyers are paid per connected call, **turn on the whisper prompt** for those contracts (Contract → Calls → whisper). It converts "the carrier says answered" into "a person confirmed", and it is the only thing that stops a false answer from looking like — and billing as — a genuine connect.

**3. Final Outcome** — the call's final status and hangup cause, plus the exhaustion reason when every buyer was tried and missed ("All buyers exhausted → missed").

Calls that never reached a dial (rejected in routing) show "No dial attempts — see decision table for why." Per-attempt raw dial telemetry (dial status, buyer-leg duration, provider leg ID, ring timeout used) is captured from the feature's ship date forward; older calls show ring seconds where timestamps allowed a backfill, with the generic "no answer" verdict. Older calls without a persisted dial stack show dialed attempts only (no stack ranks or not-dialed rows).

### Agent Connect — Live Call Taking {#agent-connect}

Agent Connect lets a buyer take live inbound calls through the buyer portal (and, later, the mobile app) instead of relying purely on their PSTN transfer number. It layers a presence gate, a real-time event feed with an on-screen "screen-pop", webhook subscriptions, and a disposition write-back on top of the existing call routing and bridge. Calls still ring the buyer's phone via the normal two-leg bridge — Agent Connect controls *whether* a presence-gated contract is eligible and *what the buyer sees* while the call is live.

<!-- react-flow:agent-connect-flow -->

#### Availability Toggle {#agent-connect-availability}

Each buyer has ONE availability flag, shared across every surface (buyer portal and mobile). Turning it on opens a time-boxed window; turning it off clears it immediately.

- **One flag, last write wins** — the portal and the mobile app write the same `buyer.callAvailableUntil`. Whichever surface writes last owns the state; the response reports the superseded surface so the other surface can update its UI.
- **TTL auto-expiry** — when the buyer sets availability on, they pick a duration (15–720 minutes, default 480). Availability auto-expires when that window lapses — there is no way to leave it on indefinitely, so a buyer who forgets to turn it off is not left ringing overnight.
- **Portal:** the toggle lives in the top bar and on the header of the Calls page, with a duration picker.
- **API:** `GET /api/buyer/call-availability` returns the current (TTL-aware) state; `POST /api/buyer/call-availability` with `{ available, durationMinutes }` sets it.

#### Contract Presence Gate {#agent-connect-presence-gate}

The contract-level **Require Agent Presence** setting (`callRequiresAgentPresence`, default off) controls whether the buyer must be available for the contract to win a call:

- **Off (default)** — routing is unchanged. The contract is evaluated exactly as before; agent availability is ignored.
- **On** — the routing engine only lets the contract win when the buyer is currently available. If the buyer is unavailable the contract is skipped with reason `agent_unavailable`; if the buyer already holds a live call leg — **ringing or answered**, on any of their contracts — it is skipped with `agent_busy`. Both reasons are masked from partners. The busy check runs on the same live-leg counter as [concurrency caps](#concurrency-caps), as an effective buyer cap of 1 — so a leg that is still ringing already blocks the buyer, and an agent is never double-dialed.

#### Surfaces {#agent-connect-surfaces}

There are three ways a buyer can take calls — the **web portal**, the **mobile app**, and plain **PSTN** (the transfer number). Each surface can be turned on or off independently, at three levels:

| Control | Where | Default | Effect |
|---|---|---|---|
| **Portal Call Taking** (`portalCalls`) | System → Features (tenant) | On | Turns the buyer-portal availability toggle and live call pops on or off for the whole tenant. Requires the Calls module. |
| **Mobile App Calls** (`mobileCalls`) | System → Features (tenant) | On | Lets mobile app servers set buyer availability via the service-token API. Requires the Calls module. Tenants with no app turn this off. |
| **Disable mobile app calls** (`mobileCallsDisabled`) | Buyer detail → Calls tab → Call Taking | Off | Per-buyer kill switch for the mobile surface. Shown only when the tenant has Mobile App Calls on. |
| **Accept portal / mobile availability** (`callPortalEnabled` / `callMobileEnabled`) | Contract → Calls → Availability Surfaces | On / On | Which surface's availability may satisfy *this* contract's presence gate. |

PSTN has no toggle — a contract accepts PSTN transfers whenever it has a transfer number set. The tenant and per-buyer switches gate *who can set availability*; the contract switches gate *which surface's availability counts* once the presence gate is on. For example, a contract with `callPortalEnabled` off but `callMobileEnabled` on will treat a buyer who is available via the portal as `agent_unavailable`, but route normally when the same buyer is available via the mobile app. Writing availability from a disabled surface is rejected `403 FEATURE_DISABLED` (mobile only — the portal surface is gated at the page/route level).

#### Screen-Pop Event Feed {#agent-connect-feed}

While a presence-gated call is live, the portal shows an incoming-call card driven by a real-time event feed. The feed is delivered over Server-Sent Events (`GET /api/buyer/call-events/stream`) with a polling fallback (`GET /api/buyer/call-events?after=<cursor>`) that returns only events newer than the last `cursor` the client has seen, oldest first.

**Redaction is enforced at the feed and webhook layer, not the UI.** Pre-accept events (`ringing`, `missed`) carry only a redacted consumer preview and NEVER the consumer phone number or last name. The full contact is only revealed once the buyer accepts the call.

#### Disposition {#agent-connect-disposition}

After a connected call, the buyer records an outcome via `POST /api/buyer/calls/{id}/disposition`. Only post-connect dispositions are accepted — `voicemail`, `call_back`, `quoted`, `sold`, `not_interested`, `dnc` (`no_answer` is excluded because a connected call cannot be a no-answer). The write goes through the unified disposition system (`dispositionLog`), is ownership-scoped to the buyer's own sale on the call, and is idempotent — re-posting the current disposition is a no-op.

#### Agent Connect Webhooks {#agent-connect-webhooks}

Buyers can subscribe an external https endpoint to receive Agent Connect events server-to-server. This is separate from the lead-postback webhook system — it uses its own subscription table and HMAC secret.

**Subscribing (API-first).** Create a subscription with `POST /api/buyer/webhook-endpoints` with `{ url, eventPattern }` (the URL must be `https://`; `eventPattern` matches event names, e.g. `call.*`). The response includes an `hmacSecret` that is shown **exactly once** — store it immediately, it is never returned again. List subscriptions with `GET /api/buyer/webhook-endpoints` (the secret is never listed) and disable one with `DELETE /api/buyer/webhook-endpoints/{id}` (soft-disable; the emitter then skips it).

**Envelope.** Every delivery is a JSON body of this shape:

```json
{
  "event": "call.agent.ringing",
  "deliveryId": "3f2b1a0c-9d8e-7f6a-5b4c-3d2e1f0a9b8c",
  "occurredAt": "2026-07-04T18:05:00.000Z",
  "tenant": "b2c3d4e5-f6a7-8901-bcde-f12345678901",
  "apiVersion": "v1",
  "data": { }
}
```

**HMAC verification.** Each request carries these headers:

| Header | Value |
|--------|-------|
| `X-LR-Event` | The event name (e.g. `call.agent.ringing`). |
| `X-LR-Delivery-Id` | Unique id for this delivery attempt's payload (matches `deliveryId`). |
| `X-LR-Signature` | `sha256=<hex>` — HMAC-SHA256 of the **raw request body** using your `hmacSecret`. |

To verify: compute `sha256=` + `hmac_sha256(hmacSecret, rawBody).hexdigest()` and compare it to `X-LR-Signature` using a **constant-time** comparison (e.g. `crypto.timingSafeEqual`). Compute the HMAC over the exact bytes received — do not re-serialize the parsed JSON first, or whitespace differences will break the signature.

**Retry ladder.** A delivery that does not return a 2xx is retried on a fixed backoff: **1s → 5s → 30s → 5m → 30m → 2h**. After the final attempt fails the delivery is moved to the dead-letter state and no longer retried.

**Event payloads.**

| Event | Payload |
|-------|---------|
| `call.agent.ringing` | Redacted preview only: `{ firstName, lastInitial, age, product, state, city }`. **No consumer phone or last name.** |
| `call.agent.accepted` | Full contact (buyer has taken the call). |
| `call.agent.missed` | Redacted preview only (same shape as `ringing`). **No consumer phone or last name.** |
| `call.completed` | Full contact plus call outcome (duration, disposition). |
| `call.availability.changed` | `{ available, availableUntil, surface }`. |

### Call Billing {#call-billing}

The unified call billing model uses the same vocabulary as Ringba and Retreaver. Configure it via the **Call Billing** form section on offer, campaign, or contract detail pages.

**Payout types:**
- **Fixed** — flat dollar amount per qualified call
- **Revshare** — percent (0–100%) of upstream revenue, optional max payout cap
- **Raw call** — pays for every inbound call regardless of conversion

**Conversion events** (when the payout fires):
- **Contract billable event** (campaign/partner rules only — the default for new campaign rules) — the payout fires exactly when the buyer contract billed the call, so the partner payout pairs with the same duration threshold as the contract rule; pick **Call Length** instead when the partner payout needs its own custom duration
- **Call Length** — call duration must meet/exceed the `Length (seconds)` threshold
- **Connected** — fires the moment target answers
- **Dialed** — fires the moment we dial the target
- **Postback** — fires only when an external system posts back conversion (CPA-style)

**Start Call Length On** (where the duration timer starts for Call Length rules):
- **Connect** (default) — starts when target answers
- **Dial** — starts when we dial the target
- **Incoming** — starts when caller enters the system (includes IVR time)

For positive length thresholds, the target must answer before the conversion can fire. `Dial` and `Incoming` can count ringing/IVR time toward the threshold after answer, but no-answer attempts do not qualify.

**Duplicate payouts** (dedup):
- **Allow** — always pay
- **Block** — never pay duplicate caller
- **Window** — only pay once per `Duplicate Window (sec)`

**Inheritance:** the system resolves billing rules in order: contract → offer → campaign. If a parent has no matching active rule, it falls through to the next.

**Multi-tier pricing** is expressed as stacked `callBillingRule` rows on a parent (offer/campaign/contract) — mirroring Ringba "Add Campaign Payout Setting" and Retreaver "Tag-based conversion criteria". Each rule has a priority, optional tag/disposition match, and its own billing config.

**Billing Mode (contracts).** The contract's Call Billing panel adds a **Billing Mode** selector:
- **Single** (default) — the highest-priority matching rule bills, exactly as before. The panel shows that one rule's fields inline.
- **Multiple** — every contract rule whose duration threshold the call meets bills its own revenue event. Example: rules at 10s → $3 and 60s → $10 on a 70-second call both fire.

**Charge Policy** (Multiple mode only) controls what the buyer is charged:
- **Sum all met rules** — charges the total of every met rule ($13 in the example above).
- **Highest met rule only** — charges only the richest met rule ($10 in the example). The other met rules still fire their `call.billed` webhooks, marked `{Rule_Charged} = false`.

**Multiple-mode restrictions:** only **Call Length** rules qualify — rules with a disposition match or a **Revshare** payout can't be created on a Multiple-mode contract, and a contract can't switch to Multiple while such rules exist (the save is rejected with the offending rule names). When no eligible rule matches a call, billing falls back to the normal single-winner resolution (contract → offer).

**Billed timeline events.** Billing writes a `call.billed` event to the call's Events timeline — one row per billed rule (rule name, price, threshold, anchor, charge policy) in both modes. Calls that bill $0 write no `call.billed` row.

If no complete buyer billing config resolves, call completion fails closed with a `$0.00` buyer charge instead of falling back to historical connect-price fields. For price-based routing, incomplete billing resolves to a `0` sort price.

#### Per-Call Service Fee {#call-service-fee}

Everything above is money moving between you, your buyers, and your partners. The **Service Fee** is different: it's what *you* paid the platform for an individual call — the call-tracking credits deducted from your balance for it, converted at the fixed peg of 1 credit = $0.01 (see [Tenant Plan, Credits & Auto-Recharge](#tenant-plan-credits)). It is always shown in **USD**, even when your account runs a different currency.

Where it appears:

- **Calls list** — a **Service Fee** column in the Financial group, hidden by default; enable it from the column picker. Group-by subtotals include it, and it also shows in each row's expanded Summary panel.
- **Call detail** — a **Service Fee** row in the Info tab's Financial section.
- **Calls CSV export** — a **Service Fee (USD)** column (see [CSV Export](#csv-export)).
- **Call Drilldown** — a picker-only **Service Fee** column summing the group's fees, exported as **Service Fee (USD)**.

A call with no per-call charge shows **—**, never $0.00. Notably, whenever the platform runs daily-aggregate call billing, charges land in your transaction ledger as daily summary rows rather than on individual calls — so every call shows **—** while that mode is active. To reconcile charges programmatically in either mode, use the [call-charges feed](#call-charges-feed).

### CPA Conversions

Call dispositions can trigger CPA (Cost Per Acquisition) conversions, just like lead dispositions. When a call disposition with `triggersCpa: true` is posted, the system creates a conversion event and adjusts partner payouts accordingly.

### CPA Postback URL {#cpa-postback}

When CPA revenue is enabled on a contract, the system shows a **postback URL** that buyers can use to fire conversions without needing API keys or dashboard access. The postback key is configured at the **buyer level**, so one key is shared across all of a buyer's contracts.

**Setting up the postback key:**
1. Go to the **buyer detail page** > Settings section
2. Type a key manually or click **Generate** to create a random `pb_...` key
3. Save — the key is now used in all postback URLs for this buyer's contracts

**How it works:**
1. Enable the CPA Revenue capability on a contract (Details tab → Capabilities card) and save
2. A postback URL appears in the CPA Revenue section with format:
   `https://theleadrouter.com/api/v1/postback?key=pb_xxx&saleId={{saleId}}`
3. If the buyer has no postback key set, the URL omits the `key` parameter and works without authentication
4. The buyer places this URL as a pixel, postback, or webhook in their system
5. When a conversion happens, their system fires the URL with the actual `saleId`
6. The CPA amount is charged to the buyer's balance

**Usage:**
- **GET request** — works as an image pixel or redirect URL
- **POST request** — works as a webhook/postback
- Replace `{{saleId}}` with the sale ID from the lead delivery response
- The `key` parameter is optional — if the buyer has a postback key, it must match; if no key is set, the request works without authentication

**Changing the key:**
- Edit the key on the **buyer settings page** (not per-contract)
- Changing the key invalidates all previously shared URLs for that buyer
- Share the new URL with the buyer from any of their contract CPA sections

### Call Recording {#call-recording}

When recording is enabled on the contract's call settings, the system records the bridged portion of the call. Recordings are accessible from the call detail page in both admin and buyer dashboard views.

### Call Settings on Contracts {#call-settings-on-contracts}

**Location:** Contract Detail > **Calls** tab

The Calls tab appears once **Call Tracking** is enabled on the contract's Details tab (Capabilities card). Per-contract call configuration:
- **Destination Number**: Buyer's phone number to bridge calls to
- **Whisper Enabled**: Toggle whisper prompt before connect
- **Whisper Message**: Text-to-speech message for the buyer
- **Recording Enabled**: Record the call
- **Call Billing Rules**: Buyer charge rules configured from the contract's Advanced Call Billing Rules panel
- **Timeout**: Seconds to wait for buyer answer
- **Max Simultaneous Calls**: Live-call concurrency cap for this contract — see [Concurrency Caps](#concurrency-caps)

#### Premium Destination Transfer Numbers {#premium-destination-numbers}

Some US area codes — 605, 712, 218, 641 and similar "access-stimulation" areas — terminate calls at several times the normal per-minute carrier cost. To keep that cost visible instead of silently absorbed, transfer numbers (**Transfer Number** and **Backup Number** on the contract's Calls tab, and the transfer-number step in call onboarding wizards) are classified when saved:

- **Normal / toll-free numbers** save as before. Any previously stamped surcharge is cleared automatically when a premium number is changed back to a normal one.
- **Premium-rate numbers** show an amber warning and require an explicit acknowledgment checkbox — *"I understand calls transferred to this number bill an additional N credits/min"* — before the save is allowed. Saving without the acknowledgment fails with `PREMIUM_ACK_REQUIRED`. On acknowledgment, a **premium destination surcharge** (default **3 credits/min**) is stamped on the contract and billed on that contract's transferred minutes alongside the base call-tracking usage charge.
- **Non-US numbers are not supported** as transfer destinations and are rejected at save time, with or without acknowledgment.

Classification uses a tiered rate lookup — an imported carrier rate deck, then the Twilio Pricing API (cached 30 days), then the static high-cost area-code list — so a premium destination the static list misses can still be caught server-side; the UI then reveals the same acknowledgment checkbox.

The stamped rate is shown read-only in the Calls tab's Phone Numbers section. Premium termination rates vary by destination carrier, so a daily cost sweep compares each contract's observed per-minute cost against its stamp and alerts (tech digest) when a surcharge is missing or undersized — **superadmins can adjust or override the per-contract rate**. A superadmin-set rate (including an explicit removal) always takes precedence over the automatic default. The surcharge never re-bills past minutes; it applies from the save forward. Surcharges are also **not refunded** when a call is adjusted or credited — they cover carrier termination cost the platform actually incurred on the transferred minutes.

<!-- react-flow:premium-destination-flow -->

#### Call Caps on Contracts {#contract-call-caps}

When a contract's **Call Cap Mode** is set to `separate`, individual call cap fields appear:
- **Daily Call Cap**: Max calls per day
- **Weekly Call Cap**: Max calls per week
- **Monthly Call Cap**: Max calls per month

These caps are independent of lead caps. When any cap is reached, the contract is excluded from call routing until the cap period resets.

#### Concurrency Caps (Max Simultaneous Calls) {#concurrency-caps}

Concurrency caps limit how many **live calls** a contract or a buyer can hold at the same time — the live-call analog of the daily/weekly/monthly volume caps above. They exist at two levels, and both apply independently (the stricter one wins):

- **Contract cap** — **Max Simultaneous Calls** in the Call Caps section of the contract's Calls tab. Applies in both cap modes (shared and separate).
- **Buyer cap** — **Max Simultaneous Calls** in the Concurrency section of the buyer detail page's Calls tab. One shared counter across **all** of the buyer's contracts — a live leg on any contract counts against it.

**Value semantics:**

| Value | Meaning |
|-------|---------|
| Empty (default) | Unlimited |
| `0` | Blocks all new calls |
| `N` | At most N simultaneous live legs |

**Counting starts at dial.** A call occupies a slot from the moment the buyer's leg is dialed — both **ringing** and **answered** legs count. The slot frees when the leg reaches a terminal state (completed, missed, failed, declined). Test calls never count, and a zombie call stops counting after the live-call max age (4 hours); the stale-call sweep terminalizes leftovers, so a crashed leg can never hold a slot forever.

**At the cap: skip, never queue.** A contract at its cap — or whose buyer is at the buyer cap — is skipped in the routing waterfall with reason `Concurrency Cap Reached`, and the call falls to the next eligible contract. Callers are never held in a queue and never hear a busy signal, matching Ringba and Retreaver concurrency behavior. The lead/caller affinity fast-paths pass the same gate, so an affinity-matched contract at its cap is skipped too. Lowering a cap below the current live count blocks new calls until enough legs drain — live calls are never cut off.

**Hard cap under race.** Beyond the fast eligibility check, the engine re-counts both scopes under a per-buyer lock immediately before bridging each leg (including failover re-bridges). Two calls racing for the last slot cannot both win — the loser falls to the next waterfall candidate through the normal failover machinery.

**Presence gate consolidation.** For contracts with **Require agent availability** on, the "buyer already on a live call" check runs on this same live-leg counter as an effective buyer cap of 1. A **ringing** leg now blocks a presence-gated buyer (previously only an answered call did) — an agent is never double-dialed. Presence-driven blocks keep the `agent_busy` / "Agent already on a live call" reason and are always enforced, independent of the rollout flag below.

**Buyer self-serve.** With **Buyer Can Set Own Cap** enabled (buyer detail > Calls tab > Concurrency), the buyer can edit their own Max Simultaneous Calls from the buyer portal **Settings** page. The admin-set **Self-Serve Cap Floor** is the lowest value the buyer may choose — clearing the cap to unlimited is always allowed, since the floor exists to stop a buyer throttling their call flow below the agreed level. Every portal change is audit-logged. Without the permission the field is hidden and the API rejects the write with `403`.

**Rollout: observe, then enforce.** New-cap enforcement ships in **observe mode** by default: the engine computes every cap decision and writes a trace annotation on the call's routing waterfall, but does not skip contracts or block bridges. Setting the `CONCURRENCY_CAP_ENFORCE` environment flag (`1` or `true`) turns on real blocking, after the observe soak confirms the counts look right. The presence (`agent_busy`) path is exempt from the flag — it is pre-existing live behavior and always enforced.

### Number Pool Management {#number-pool-management}

The system maintains a pool of tracking numbers to optimize telephony costs and automate number provisioning. Instead of manually purchasing and assigning numbers, the pool handles the lifecycle automatically.

#### How It Works

1. When a campaign enables call tracking, a number is automatically assigned from the pool
2. Numbers are assigned **longest-to-renewal first** — maximizing usage before the next billing cycle
3. When call tracking is disabled or a campaign is deactivated, the number returns to the pool
4. A daily cron job identifies idle numbers approaching their renewal date and releases them to stop billing
5. If no pool numbers are available, a new number is purchased from Telnyx automatically

#### Pool Settings

**Location:** [/admin/settings](/admin/settings) > Calls > Phone Numbers

> **One inventory surface.** The former **Tracking Numbers** and **Number Pool** sub-tabs are merged into a single **Phone Numbers** tab. Every tenant number — a campaign's fixed (static) number, campaign-owned DNI pool numbers, and shared pool inventory — lives in one table, with the pool stats and settings on top. Old `?tab=tracking-numbers` / `?tab=number-pool` links still resolve here.

Configure pool behavior:
- **Default Area Code**: System-wide 3-digit US area code used when buying new tracking numbers from the telephony provider. Required — if blank, every Call Enabled toggle returns `AREA_CODE_NOT_CONFIGURED` (422) and the partner campaign UI shows "No default area code is configured." Each campaign can override this on its own detail page.
- **Release Threshold**: Days before renewal date to auto-release idle numbers (default: 7). Lower values keep numbers longer; higher values save costs on unused numbers. The renewal cull only takes **shared** pool numbers — campaign-owned reserve numbers are excluded and exit via idle decay first (see [number lifecycle](#dni-number-lifecycle)).
- **Idle decay (days)**: Days a campaign-owned pool number can sit unused (no visitor assignment) before it decays back to the shared tenant pool (default: 7, 0 = never). Tenant setting overrides the platform default; campaigns can override both in the DNI panel. See [number lifecycle](#dni-number-lifecycle).
- **Lease idle timeout (min)** / **Max lease length (min)**: How long a visitor's DNI lease lives. The snippet heartbeats every 15 seconds while the tab is open, sliding the lease forward by the idle timeout (1–120 min, default 10) — when heartbeats stop (crash, laptop sleep) the number returns to the pool after that timeout. The max lease length (5–240 min, default 30) is the absolute per-visitor ceiling from first assignment; heartbeats never extend past it. Tenant setting overrides the platform default.
- **Minimum shared available**: A tenant-wide **warm buffer** (0–50, default 1) that keeps at least this many **shared** DNI numbers available in the pool at all times, so a visitor is never made to wait on a provider purchase at impression time. Unlike the per-campaign **Minimum available numbers** floor (which buys into a single campaign's reserve), this buffer buys **untagged shared** numbers any campaign can draw from. It refills automatically in the background — every hour by the pool-maintenance job and opportunistically after each DNI impression — using the **Default Area Code** above. Fills are clamped by the tenant daily auto-buy limit (default 10/day), which shared purchases count against alongside campaign auto-buys, so a large buffer fills over several days rather than all at once. The buffer only buys while at least one non-inactive campaign (active, paused, or testing) has DNI enabled — an account with DNI fully off never warm-buys. Set to 0 to disable.
- **Max auto-bought numbers**: A tenant-wide hard cap (1–200, default 20) on the **total** auto-purchased numbers the tenant can hold across all campaigns — campaign auto-buys, per-campaign floors, and the shared warm buffer all stop provisioning once the tenant owns this many auto-bought numbers (skip reason `tenant_cap_reached`). Manually purchased numbers don't count toward the cap. This bounds monthly number rent no matter how many campaigns have auto-buy on.

The settings page also displays pool stats:
- **Assigned**: Numbers currently in use by campaigns (shows a "provisioning" count when auto-buys are mid-purchase)
- **Available**: Numbers in the pool ready for assignment
- **Utilization**: Share of pool numbers currently assigned (assigned ÷ (assigned + available)). High utilization means the pool is close to exhaustion.
- **Pool Miss (24h)**: Percentage of DNI impressions in the last 24 hours that could not get a pool number and fell back to the static campaign number. Turns red above 10%.
- **Estimated Monthly Cost**: Projected cost based on current pool size (shows how many numbers were auto-bought today)

The **Phone Numbers** table lists every tenant number with these columns:

- **Phone** — the number, linking to its detail page.
- **State** — a single plain-language chip collapsing raw status, ownership, and lease flag (see below).
- **Attached to** — the campaign the number belongs to (static/campaign-pool) or is currently borrowing it (a shared number serving a live visitor); blank for idle shared inventory. (Buyer-attached numbers will appear here as the inventory grows.)
- **Use** — what the number is for: **Static** (a campaign's fixed number), **DNI pool** (campaign-owned dynamic number), or **Shared pool** (tenant inventory any campaign can lease).
- **Returns to pool** — for an idle campaign-owned pool number, the date it decays back to the shared pool if it stays unused; blank for static/shared/in-use numbers.
- **Renews** — the monthly renewal (billing) date. Manually added numbers are stamped one month out on creation, matching auto-bought numbers.
- **Provider** and **Actions**.

The **State** chip reads:

- **Serving visitor** (green, with a live pulse) — leased to an active DNI visitor session right now, so inbound calls attribute back to that visitor.
- **Provisioning** — being purchased from the carrier.
- **Static** — a campaign's fixed assigned number (its DNI fallback), bound to that one campaign.
- **Campaign pool** — a campaign-owned DNI number sitting in reserve (auto-bought for that campaign; only it may lease the number).
- **Shared pool** — tenant inventory any campaign can lease, currently idle.
- **Released** — returned to the carrier.

Available numbers can be **Released to Provider** (permanently returned to the carrier); assigned pool numbers can be **Returned to Pool** (unassigned from the campaign and made available for reuse). Per-number call handling — **IVR config, whisper, and call recording** — lives on each number's detail page (click the number or **Edit**), not on the inventory list.

#### Pool Auto-Scaling {#pool-auto-scaling}

DNI always uses a per-visitor number pool (matching Ringba/Retreaver semantics — there is no separate "pool" setting to turn on). When DNI is enabled, each visitor session is assigned a temporary pool number so inbound calls attribute back to the impression. If the pool is exhausted the visitor falls back to the static campaign number (a **pool miss**) — attribution is lost for that session.

**Auto-buy** grows the pool automatically instead of failing silently. It is configured per campaign in the campaign detail page's **Call Tracking → Auto-buy pool numbers** section (shown when DNI is on). **On by default for new DNI campaigns:** turning DNI on for a campaign that has no auto-buy config seeds auto-buy enabled with a max pool size of 10 — turn it off there if you want a fixed pool. Campaigns that already had DNI on keep whatever they had.

- **Auto-buy pool numbers**: When a visitor hits an exhausted pool, the system buys a new number in the background. The current visitor still gets the static number (the purchase never blocks the page), but the pool is larger for the next visitor.
- **Max pool size**: Ceiling for auto-buying (1–50). Auto-buy stops once the tenant has this many pool-managed numbers (available + assigned + provisioning).
- **Minimum available numbers**: A floor (0–10 slider, 0 = off, must be ≤ max pool size) that keeps at least this many pool numbers ready to assign at all times. The floor is **tenant-pool-first**: shared available numbers in the tenant pool count toward it, and only the remainder the shared pool can't cover is auto-bought into this campaign's own pool. Provisioning fires **immediately when you save the campaign** — an idle campaign with zero traffic gets its reserve up front — and again in the background whenever assignments or carrier releases drop availability below the floor. Replenishment is bounded by the max pool size ceiling and the tenant daily buy limit. (Two campaigns may count the same shared numbers toward their floors; if a burst actually exhausts the pool, the miss-triggered auto-buy still covers it.)
- **Preferred area code**: Optional 3-digit US area code for auto-bought numbers. Leave blank to automatically match the campaign's assigned tracking number's area code (e.g. a campaign on a 619 number auto-buys 619 pool numbers); set a 3-digit code to override. Precedence: explicit code → the campaign's saved area-code override → the assigned number's area code. If no numbers are available in the preferred area code, the system retries without it (any available area code) so the pool never starves.

Guardrails:
- A tenant-wide **daily purchase limit** (default 10 numbers/day) caps runaway spend even if many campaigns have auto-buy on.
- A tenant-wide **Max auto-bought numbers** cap (default 20 — see [Pool Settings](#pool-settings)) bounds the total auto-purchased numbers held across all campaigns and the shared buffer.
- Pool numbers are **tenant-shared** — a number auto-bought because one campaign missed is available to every campaign in that tenant.
- Auto-bought numbers land on the tenant's own telephony account, so the tenant pays the carrier directly.

Watch the **Pool Miss (24h)** stat: if it stays above 10%, raise the max pool size or check the daily limit.

#### DNI Number Lifecycle: Idle Decay {#dni-number-lifecycle}

Auto-bought numbers belong to the campaign that bought them (its **reserve** — the "Own" chip in the Pool numbers block). Without decay, a paused or dead campaign would hold — and keep paying monthly renewal on — its reserve forever. Idle decay closes that loop. The full lifecycle:

1. **Campaign reserve** — auto-buy (miss-triggered or floor replenish) purchases a number tagged to the campaign. It renews monthly while held; that's the cost of a reserve.
2. **Idle decay** — an hourly maintenance job returns any campaign-owned number with **no visitor assignment for N days** to the **shared tenant pool**: the campaign tag is cleared, the number keeps billing, and every campaign in the tenant can reuse it (it simply shows as "Shared" from then on).
3. **Tenant pool** — the decayed number serves any campaign's DNI or call-tracking needs like any other shared number.
4. **Renewal release** — if it is *still* unused when its renewal date comes within the Release Threshold, the daily release cron returns it to the carrier and billing stops. (Campaign-owned numbers are never culled by this cron — decay always happens first.)

Configuring the window:

- **System default**: 7 days. Change it (or set a tenant-wide value) under System → Call Settings → Number Pool → **Idle decay (days)**. `0` turns decay off platform/tenant-wide.
- **Per-campaign override**: campaign DNI panel → Advanced → Pool automation → **Idle decay override** (0–90). Blank inherits the system/tenant value; `0` means this campaign's numbers never decay.

**Floor protection**: while a campaign is live for DNI (DNI on + auto-buy enabled), decay never drops its own available numbers below the **Minimum available numbers** floor — the freshest numbers are kept and the oldest-idle decay first, so the reserve never churns through decay-then-rebuy. If DNI or auto-buy is off, the floor is moot and all idle numbers decay.

The same hourly job also runs the lease/zombie sweep and the idle-floor replenish, so a zero-traffic campaign both holds its reserve topped up and sheds numbers it stopped using. Every decay writes a `pool-decayed` audit log entry with the campaign, idle window, and destination.

#### Callbacks to a Released Pool Number {#dni-released-number-callback}

A visitor's pool number is leased for ~30 minutes. People routinely save or screenshot the number and call back hours later — long after the lease has expired and the number went back to the pool.

That callback still connects. When a released pool number is dialed, the system remembers which campaign last held the lease and routes the call under that campaign's **No-impression policy** (DNI panel → Advanced), exactly as it would for a call with no active impression:

- **Route to static** (the default) — the call routes normally with the original campaign's context.
- **Reject** — the caller hears the rejection message and the call is recorded as failed.

Either way a call record now exists — previously these callbacks got no answer at all and left nothing behind. The expired lease is called out on the call detail page's **Routing Trace** tab, under Final Outcome. Rejected ones additionally carry the reason **`dni_number_released`** in the Calls list's failure-reason column, distinguishing an expired lease from a missing tracking pixel (`dni_no_active_impression`).

Two guardrails:

- **The pointer expires on re-lease.** Once the number is leased to a new visitor, the new campaign owns it outright — a stale callback never attributes to the previous campaign or partner.
- **Numbers are handed out least-recently-used.** The pool prefers the number that has sat idle longest, widening the window before a saved number is recycled to someone else.

A number released *before* this behavior shipped has no stored pointer, so callbacks to it are not routable until it is leased and released once more.

#### DNI Snippet Behavior {#dni-snippet-parity}

The DNI script swaps visible tracking numbers on the landing page. Two settings control *how* and *where* it swaps — both are in the campaign's **Call Tracking** section and apply whenever DNI is on.

**Format-preserving swap** — The swapped-in pool number always keeps the page's own formatting. If the page shows `(619) 330-5765`, the visitor sees `(619) 330-5768`, not the raw `+16193305768`. Dash, dot, bare-10-digit, country-code, and E.164 shapes are all mirrored; anything unrecognized falls back to `(NNN) NNN-NNNN`. `tel:` links always use the dialable E.164 form regardless of the displayed shape. This is automatic — no configuration needed.

**Anti-flicker cloak** — Prevents the visible "flash" where the static number paints first and then swaps to the pool number after the impression call returns.

- Add the class `lr-number` or the attribute `data-lr-number` to any element that shows a number you want cloaked (for example `<span class="lr-number">(619) 330-5765</span>`). The snippet hides only those elements (using `visibility`, so there is no layout shift) until the swap resolves, then reveals them. Pages that don't opt in are unaffected.
- **Anti-flicker cloak** toggle: on by default when DNI is enabled.
- **Cloak timeout (ms)**: failsafe reveal window (300–5000, default 1500). If the impression request stalls, the number is revealed anyway so it is never left permanently hidden. The cloak also reveals immediately on a successful swap, a pool miss, or a request error.

**Attribution capture (no setup required)** — The snippet captures tracking parameters from the landing-page URL automatically whenever DNI is on. There is no toggle for it, because there is nothing to decide: it captures the platform's standard keys (all `utm_*`, click IDs such as `gclid`/`fbclid`/`msclkid`, and the full sub convention `sub1`–`sub10` with its `subid{n}`/`s{n}` aliases) plus **every field name declared on this campaign's vertical or campaign field list**. Declared fields need no listing — declaring the field is what makes it captured.

- **Allowed custom keys** is for the leftovers only: third-party parameters nobody declared, like a tracker's `cpid` or a CDN's `cf_state`. It is purely additive. You never need to list a standard key or one of your own fields here, and listing them does nothing.
- Anything not in that combined set is dropped, so a stray or hostile query parameter is never stored.
- **Capture form tags** is a separate, opt-in setting and stays off by default. Form *values* can be personal information, so capturing them is a deliberate choice rather than a default.

> Before 2026-08-06 a "Capture URL tags" toggle gated all of the above. A campaign could have correct fields and correct defaults and still record nothing if that switch was off — which is exactly what happened to one live campaign for six days. The toggle is gone; existing campaigns need no action.

**Path filters** — Restrict the swap to specific landing-page paths so the number only changes where you expect it to.

- Enter one path per line (max 20) in the campaign's **Path filters** box. Leave it empty to swap on every page.
- Matching supports exact/prefix paths (`/lp` matches `/lp` and `/lp/quote`), glob suffixes (`/lp/*` matches nested paths), and a bare `*` for everything. Query strings are ignored.
- On an off-scope path the snippet skips the impression call entirely (keeping the static number), and the server also declines pool assignment (recorded as a pool miss) — so an out-of-scope page never burns a pool number.

**Panel layout** — In the campaign's Call Tracking section, the DNI panel starts compact: a "Build your lander with AI" row and the **Dynamic Number Insertion (DNI)** switch (whole row is clickable). Because the tracking script is only needed once DNI is on, turning the switch on reveals — in order — an **Embed Script** block (a copyable, ready-to-paste `<script src="…/dni/<token>.js" async></script>` tag; the **Copy script** button copies the full tag, not just the URL, plus placement tips) and then a collapsed **Advanced DNI settings** accordion. Everything else — Replace numbers, Path filters, Allowed custom keys, Capture form tags, No-impression policy, Anti-flicker cloak, and Auto-buy — lives inside that accordion and stays collapsed until you expand it. The old separate **Number pool** setting is gone: the pool is part of DNI itself, so turning DNI on is all it takes. All boolean settings render as toggles.

**Pool numbers** — When DNI is enabled, a read-only **Pool numbers** block appears at the top level of the DNI panel (directly below the Embed Script, no need to open Advanced) listing the phone numbers backing this campaign's DNI: an **Own** chip marks numbers auto-bought for this campaign (any status, including *provisioning*), a **Shared** chip marks the tenant's unclaimed inbound pool this campaign can draw from, and a summary line reads e.g. "2 in campaign pool · 3 shared available". With no numbers yet it either notes that auto-buy provisions on demand (when Auto-buy is on) or points to System → Call Settings → Number Pool. Manage the shared pool there — this block is view-only. A leased row (one currently serving a visitor, with a "returns to pool in Xm" countdown) also has a chevron that expands lease details: the visitor's IP address, when the lease started, the last heartbeat ping from the page, and the exact time the number returns to the campaign pool.

**Releasing reserved numbers** — A pool number shows status **assigned** (a *reserved* number) while it is leased to an active visitor session. Leases release themselves automatically about 30 minutes after the visit, returning the number to the available pool, so you rarely need to act. When you do — after a test run, or to free numbers immediately for a nearly-exhausted pool — the Pool numbers card header has a **Release reserved** button (with the reserved count). It opens a confirm dialog and forces every active lease on this campaign to expire now; a per-row **Release** link does the same for a single number. Releasing does not delete or unassign the number from the campaign — **Own** numbers stay in the campaign pool, they just become available again. Partners have the same control on the campaign's **Call Tracking** card ("Release reserved numbers"); it is always safe to run and reports how many were released (0 if none were held).

**Testing your setup** — The bottom of the DNI panel (and the partner Call Tracking card) has an **Open test page** button linking to a public page at `/dni-test/<specToken>`. The page loads this campaign's live DNI snippet exactly as a real landing page would and reports what happened: whether the script loaded, whether an impression was created, which pool number was assigned (or a **pool miss** when none was available), whether attribution capture was active, and whether the on-page number actually swapped. Each visit creates a real pool reservation — it auto-expires in ~30 minutes, or release it from the Pool numbers card above. The test-page links include demo URL parameters (`utm_source`, `gclid`, …) so the **Captured impression tags** box demonstrates attribution capture. The **Open example quiz** button opens the same page in quiz mode — a short questionnaire that always starts with a ZIP-code question, followed by questions built from this campaign's **required** fields (tick **Required** on a field in the vertical/campaign Lead Fields table to include it; PII, hidden, and contact fields are always excluded), ending on a call-to-action step that shows the assigned pool number to dial. A live debug rail shows each impression update and which answer tags were captured. If the campaign has no required capturable fields of its own, the quiz falls back to a few illustrative example questions and notes that some of those keys may be filtered out because they are not in the campaign's allowed keys.

#### DNI Captured Data & Attribution {#dni-captured-data}

When **Capture URL/form tags** is on (DNI panel → Advanced), the snippet captures tag values from the visitor's landing-page session and stores them on the inbound call. Every captured key is classified into one of two groups at call time:

- **Attribution** — marketing identifiers: UTM parameters, click IDs, sub IDs, and any extra keys you allow. Partner-confidential — never shown to buyers or returned by buyer/partner APIs.
- **Captured fields** — values matching the campaign's vertical or campaign field names (e.g. `state`, `age`). These are validated and type-coerced against the field definitions, and are available to call routing filters and billing rules like posted lead fields.

**Allowed custom keys** (DNI panel → Advanced) extends the built-in capture allow-list with your own tag keys (e.g. a network's custom click ID). Keys added here classify as attribution.

**Where it shows up:** the admin call detail page renders a **DNI Attribution** card with the two groups — Attribution as chips, Captured fields typed like lead fields. The matched person record also carries **first-touch** (recorded once, from the earliest matched call, never overwritten) and **last-touch** (updated on every subsequent matched call) attribution, shown as a Marketing Attribution card on the person detail page — attribution identifiers only, never captured form values.

**Caller geo vs captured geo:** the caller's state/zip from the phone carrier wins when the carrier knows it; when telephony reports nothing, a form-captured state/zip is used instead for call routing eligibility — captured geo is no longer discarded when the carrier lookup comes back empty.

**PII handling:** captured values for fields flagged as PII are encrypted at rest, decrypted only for authorized admin views (masked for sessions without PII access), and never exposed through buyer or partner APIs.

#### Campaign Call Tracking

On the campaign detail page, a **Call Tracking** section lets you enable or disable call tracking for that campaign.

- The section is greyed out if the parent offer does not have calls enabled
- **Enabling** call tracking auto-assigns a pool number to the campaign
- **Disabling** call tracking releases the number back to the pool
- Configure partner call payout rules from the campaign's Advanced Call Billing Rules panel
- **Number area code**: the tracking number's area code comes from the system default (Resolution order: tenant systemSetting → global systemSetting → fail with `AREA_CODE_NOT_CONFIGURED`), set under System > Calls > Number Pool. The old per-campaign "Area Code Override" input has been removed from the Call Tracking UI to keep the basic view simple; any `callTrackingAreaCode` value previously saved on a campaign is still honored server-side (campaign override → tenant → global). To pick an area code for auto-bought pool numbers on a specific campaign, use **Preferred area code** in the DNI panel's Advanced settings (Auto-buy).

#### Offer Call Settings {#offer-call-settings}

On the offer detail page, the **Call Routing** tab controls call routing for the entire offer; the enable toggle is the **Call Tracking** capability on the Overview tab.

The **Call Tracking** toggle is gated — it is disabled (greyed out) when no contracts linked to this offer have `callEnabled=true`. A message reads: "No call-enabled contracts available. Enable calls on a contract first."

When call routing is enabled, configure:

- **Lead Affinity**: Off / Same Contract / Different Contract — matches caller phone to person records linked to leads on this offer
- **Caller Affinity**: Off / Same Contract / Different Contract — matches repeat callers to their previous contract
- **Affinity Lookback Window**: Number of days to search for affinity matches (default 30)
- **Call Distribution Rule**: Price, Priority, Weight, Round Robin, or Simultaneous
- **Connection Mode** (formerly "Distribution Type"): Sequential (one at a time) or Non-Sequential (all at once) — hidden when Simultaneous is selected
- **Call Affinity Fallback**: Controls behavior when an affinity match resolves to a contract that can't take calls (disabled, no phone number, capped out). Two modes:
  - **None** (default) — call is not routed; caller hears hangup
  - **Next Match** — skip the matched contract and fall through to standard distribution, routing to the next eligible buyer

The **Only route leads to call-capable buyers** filter (formerly "Require Call-Enabled Contracts") now lives on the **Lead Routing** tab — it's a lead filter: when enabled, lead routing only distributes to contracts that have calls enabled AND a destination phone number set. It does not affect call routing (which already requires call capability).

**Disabling** calls on an offer cascades to all its campaigns — any assigned tracking numbers are released back to the pool

#### Contract Call Warning

When disabling calls on a contract, if it is the **last call-enabled contract** for its offer, a warning displays how many campaigns currently have tracking numbers assigned. Those campaigns will still have numbers but no contracts will accept calls — the numbers will be idle until calls are re-enabled or the campaigns release them.

#### Contract Call Disable Guard {#contract-call-disable-guard}

When the offer has **Only route leads to call-capable buyers** turned on, disabling calls on the **last call-enabled contract** triggers a confirmation modal. The modal warns that disabling calls will leave no call-capable contracts for the offer, which means lead routing will skip all contracts (since none meet the call requirement). You must confirm before proceeding. This prevents accidentally breaking lead distribution by removing all call-enabled contracts from an offer that requires them.

### Buyer Dashboard — Calls

**Location:** [/buyer/calls](/buyer/calls)

Buyers see their call activity in the dashboard:
- **Call List**: All calls routed to this buyer with status, duration, caller ID, and charges
- **Call Detail**: Full call timeline including ring time, connect time, duration, recording playback, and disposition

---

## Messaging {#messaging}

<!-- react-flow:messaging-flow -->

The Messaging module provides email and SMS marketing capabilities integrated directly into the lead routing platform. Messaging lives under the **Nurture** group in the sidebar (alongside AI Dialer). Opening **Messaging** takes you to its dashboard, where a horizontal tab bar (Dashboard, Contacts, Segments, Templates, Flows, Campaigns, Analytics, Revenue, Deliverability, Suppression) navigates between messaging pages. Every tab is a real page link, so bookmarks and deep links still work.

### Dashboard {#messaging-dashboard}

The messaging dashboard shows key metrics at a glance:
- **Total Contacts** — number of contacts in your messaging database
- **Active Segments** — audience segments available for targeting
- **Templates** — email and SMS templates created
- **Messages Sent (30d)** — messages sent in the last 30 days
- **Recent Activity** — latest messages with delivery status

### Contacts {#messaging-contacts}

Contacts are people in your messaging database. They may be linked to leads in the routing system.

**Where contacts come from.** Routing only creates a contact from a lead when that lead's **offer is opted into Messaging** — the switch on the offer's Integrations tab ([Messaging opt-in](#offer-messaging-optin)), which is **off by default on every offer**. If a flow never fires for leads you expected, check that switch first: an offer that was never opted in produces no contacts at all, silently and by design. Imports and manually created contacts are unaffected.

**List view**: Search by email/name, filter by email status (active, bounced, complained, unsubscribed) or SMS status (active, opted out). Paginated table with 20 per page.

**Import**: Click "Import CSV" to bulk-upload contacts. CSV must include `email` column; optional columns: `firstName`, `lastName`, `phone`, `tags`.

**Create**: Add individual contacts with email (required), name, phone, and tags.

**Detail**: View/edit contact info, engagement stats (sent/opened/clicked), tags, and full message history timeline.

### Segments {#messaging-segments}

Segments define audiences for campaigns and flows.

**Types**:
- **Dynamic** — automatically includes contacts matching rule criteria (re-evaluated periodically)
- **Static** — manually selected contact list

**Rule Builder**: Dynamic segments use a visual rule builder with AND/OR logic groups. Supported field types include string matching, numeric comparisons, date ranges, tag presence, and custom properties.

**Preview**: Before saving, preview how many contacts match the current rules.

### Templates {#messaging-templates}

Templates define the content for email and SMS messages.

**Email templates**: Use the drag-and-drop template builder with block types:
- Text, Header, Image, Button, Divider, Columns, Social links, Spacer, Raw HTML
- Merge tags for personalization: `{{firstName}}`, `{{email}}`, custom fields
- Desktop/mobile preview toggle

**SMS templates**: Simple text editor with character/segment counter and merge tag support.

**Actions**: Duplicate templates, toggle status (draft/active/archived), view version history.

### Flows {#messaging-flows}

Flows are automated sequences triggered by events.

**Trigger types**:
- **Event** — fires on a system event (e.g., lead.created, lead.sold)
- **Segment Entry** — fires when a contact enters a segment
- **Manual** — triggered via API call
- **Date Property** — fires relative to a contact's date field (e.g., 3 days before birthday)

**Step types**: Send Email, Send SMS, Delay, Condition (branch), A/B Split, Update Property, Add/Remove Tag, Webhook.

The flow detail page has two tabs:

- **Builder** — the visual canvas for designing flow steps with branching logic. Add a step from the palette, click a node to configure it in the right-hand panel, and save or delete each step individually (steps persist as you edit them). The trigger node and add-step affordance anchor the graph.
- **Settings** — the flow's name, description, trigger type and trigger-specific configuration (event name, segment selection, or date property + offset), and the attribution window. Settings edits collect into a single **Save Changes** bar that appears only when something changed and saves everything in one write; **Cancel** reverts to the last saved values. Draft and paused flows are editable; active and archived flows are read-only.

**Executions**: View execution history per flow — see which contacts entered, their progress through steps, and completion status.

### Campaigns {#messaging-campaigns}

Campaigns are one-time or scheduled bulk sends to a segment.

**Create**: Select channel (email/SMS) → choose template → choose segment → schedule or send immediately.

**A/B Testing**: Split audience between variant templates, set test percentage and winner metric (open rate, click rate, revenue), auto-select winner after specified hours.

**Stats**: Track sent, delivered, opened, clicked, bounced, complained counts with real-time progress during sends.

**Lifecycle**: Draft → Scheduled → Sending → Completed (or Paused/Cancelled at any point).

### Analytics {#messaging-analytics}

**Overview**: Date-range filtered dashboard with KPI cards (sent, delivered %, opened %, clicked %, bounced %, complained %), daily trend chart, channel breakdown, and top 5 campaigns.

**Campaign Analytics**: Sortable table of all campaigns with delivery metrics. Click through to campaign detail.

**Flow Analytics**: Sortable table of all flows with execution metrics (started, completed, avg duration, messages sent).

### Deliverability Monitoring {#deliverability-monitoring}

Tracks email delivery health across providers and recipient ISPs. Accessed from **Messaging > Analytics > Deliverability** or directly at `/admin/messaging/analytics/deliverability`.

#### Overview Tab

Aggregate delivery metrics across all providers:

- **KPI cards**: Delivery Rate, Hard Bounce Rate, Soft Bounce Rate, Complaint Rate, Unique Opens
- **Provider breakdown**: Per-provider stats (SendGrid, SES, etc.) showing volume and rates
- **Daily trends**: Line chart of delivery/bounce/complaint rates over the selected date range

#### Mailbox Providers Tab

Delivery metrics broken down by recipient ISP (Gmail, Yahoo, Microsoft Outlook, Other Providers):

- **Volume per provider**: How many emails each ISP received
- **Daily delivery rate trend**: Line chart per provider showing acceptance rates over time
- Identifies which ISPs are accepting vs rejecting your emails — useful for diagnosing domain/IP reputation issues with specific providers

#### Bounced & Blocked Tab

Separates bounces from blocks — two different failure modes:

- **Bounces**: Recipient-level rejections (invalid address, mailbox full, quota exceeded)
- **Blocks**: Provider-level rejections (IP/domain reputation, policy violations, blacklisting)
- **Period-over-period comparison**: Shows whether rates are improving or worsening vs the prior period
- High block rates from "Other Providers" often indicate IP reputation issues requiring attention

#### Spam & Unsubscribes Tab

Complaint tracking by ISP:

- Shows which providers' users are marking your emails as spam
- **Complaint rates above 0.1% risk provider suspension** — Gmail and Yahoo enforce strict thresholds
- Monitor trends after campaign sends to catch reputation damage early

#### Key Concepts

| Term | Definition |
|---|---|
| **Unique Opens** | Emails opened at least once ÷ total delivered (not sent). Some clients block tracking pixels, so this undercounts. |
| **Blocks vs Bounces** | Blocks = provider rejects your server (reputation/policy). Bounces = recipient mailbox issue (bad address, full). |
| **ISP Classification** | Based on recipient email domain. Subdomains resolve to parent ISP (e.g., `worldnet.att.net` → Yahoo). |

### Messaging Settings {#messaging-settings}

Located under **System > Messaging Settings** in the sidebar.

**General**: Default sender name, sender email, reply-to address, timezone.

**Providers**: Configure email (SES, SendGrid) and SMS (Twilio, Bandwidth) providers with credentials and daily send limits. See the provider setup guides below for step-by-step instructions.

#### Amazon SES Setup {#ses-setup}

Amazon SES is best for high-volume sending at low cost (~$0.10/1K emails). Setup is more involved than SendGrid but worth it if you're already on AWS or sending 50K+ emails/month.

**Step 1: Create an AWS Account**
- Go to aws.amazon.com and create an account (or use existing)
- SES free tier: 62K emails/month when sending from EC2
- Navigate to the SES console: Console > Services > Simple Email Service
- Select your preferred region (e.g. `us-east-1`) — all SES resources are region-specific

**Step 2: Verify Your Sending Domain (Recommended)**
Verifying a domain lets you send from any address @yourdomain.com.

1. SES Console > **Identities** > **Create Identity**
2. Select **Domain**, enter your domain (e.g., `yourdomain.com`)
3. Optionally check **"Use a custom MAIL FROM domain"** (improves deliverability)
4. SES provides DNS records to add:

| Record Type | Purpose |
|---|---|
| **CNAME x3** | DKIM signing (proves emails are legit) |
| **MX** | Custom MAIL FROM (optional) |
| **TXT** | SPF record (optional) |

5. Add these records in your DNS provider (Cloudflare, Route53, etc.)
6. Wait for verification — usually 5-30 minutes

**Step 2b: Verify a Single Email (Alternative)**
If you can't verify a whole domain:
1. **Identities** > **Create Identity** > **Email Address**
2. Enter the email (e.g., `leads@yourdomain.com`)
3. Click the verification link sent to that inbox

**Step 3: Request Production Access (Exit Sandbox)**
New SES accounts are in **sandbox mode** — you can only send to verified emails.

1. SES Console > **Account Dashboard** > **Request Production Access**
2. Fill out:
   - **Mail type:** Transactional
   - **Website URL:** your app URL
   - **Use case description:** "B2B lead notification emails to buyers and partners"
   - **Expected daily volume:** your estimate
3. AWS reviews in 24-48 hours. You'll get an email when approved.

**Step 4: Create IAM Credentials**
The system uses the AWS SDK (`@aws-sdk/client-sesv2`), which needs an Access Key.

1. AWS Console > **IAM** > **Users** > **Create User**
2. Name: `iscale-ses-sender`
3. Attach policy: **`AmazonSESFullAccess`** (or create a custom policy with just `ses:SendEmail`, `ses:SendRawEmail`)
4. **Security Credentials** tab > **Create Access Key**
5. Select **"Application running outside AWS"**
6. Save these two values — you'll need them in Step 7:

| Asset | Where It Goes |
|---|---|
| **Access Key ID** | Provider credentials `accessKeyId` |
| **Secret Access Key** | Provider credentials `secretAccessKey` |
| **Region** | Provider config `region` (e.g. `us-east-1`) |

**Step 5: Set Up SNS Webhooks (Bounce/Complaint/Delivery Tracking)**
This enables delivery status tracking — bounces, complaints, opens, clicks all flow back into iSCALE's message table.

1. AWS Console > **SNS** > **Topics** > **Create Topic**
   - Type: **Standard** (not FIFO)
   - Name: `iscale-ses-notifications`
2. **Create Subscription** on that topic:
   - Protocol: **HTTPS**
   - Endpoint: `https://theleadrouter.com/messaging/api/webhooks/ses`
   - Confirm the subscription (SES sends a confirmation request to the endpoint)
3. Go to **SES** > **Configuration Sets** > **Create Configuration Set**
   - Name: `iscale-tracking`
4. Add **Event Destination** on that configuration set:
   - Events: Bounce, Complaint, Delivery, Open, Click
   - Destination: SNS Topic > select `iscale-ses-notifications`

**Step 6: Generate SMTP Credentials (for SMTP interface)**
If using the SMTP tab in Settings instead of the API provider:

1. SES Console > **SMTP Settings** > **Create SMTP Credentials**
2. This creates a special IAM user — note: SMTP username/password are *different* from your IAM access keys
3. Save the SMTP username and password

**Step 7: Add Provider in iSCALE**

*Option A — via Messaging Provider API (recommended):*
- Go to Messaging > Settings > Providers tab
- Click "Add Provider"
- Type: Email
- Provider: SES
- Name: "Amazon SES" (or any label)
- Credentials:
```json
{
  "accessKeyId": "AKIA...",
  "secretAccessKey": "your-secret-key",
  "region": "us-east-1"
}
```
- Config — set your from address:
```json
{
  "fromEmail": "notifications@yourdomain.com",
  "fromName": "Your Company Name"
}
```
- Daily Send Limit: Set based on your SES sending quota (check SES Console > Account Dashboard)
- Mark as Default if this is your primary email provider
- Click Save

*Option B — via Settings > SMTP tab:*
- Email Address: `leads@yourdomain.com`
- Host: `email-smtp.us-east-1.amazonaws.com`
- Username: Your **SMTP username** (from Step 6, not your IAM access key)
- Password: Your **SMTP password** (from Step 6)
- Port: `587`
- SSL: Enabled

**Step 8: Send a Test Email**
- Click the "Test" button next to the provider
- Enter a recipient email
- Check your inbox for the test message

**SES Assets Checklist:**
- [ ] AWS account created
- [ ] Domain or email verified in SES
- [ ] Production access granted (out of sandbox)
- [ ] IAM user created with SES permissions
- [ ] Access Key ID + Secret Access Key saved
- [ ] SMTP credentials generated (if using SMTP interface)
- [ ] SNS topic created for webhooks
- [ ] SES Configuration Set with event destination
- [ ] DNS records added (DKIM CNAME x3, SPF TXT, MAIL FROM MX)
- [ ] Webhook endpoint URL configured and subscription confirmed

---

#### SendGrid Setup {#sendgrid-setup}

SendGrid is the fastest path to sending emails — simpler dashboard-driven setup, no sandbox period, and a generous free tier. Best for quick setup or lower volume.

**Step 1: Create a SendGrid Account**
- Go to sendgrid.com and sign up
- Free tier: 100 emails/day. Essentials plan: ~$20/mo for 100K emails/month.

**Step 2: Authenticate Your Sending Domain (Recommended)**
Domain authentication proves to inbox providers that you're authorized to send from your domain.

1. **Settings** > **Sender Authentication** > **Authenticate Your Domain**
2. Select your DNS host (Cloudflare, etc.)
3. Enter your domain
4. SendGrid provides **3 CNAME records**:

| Record | Purpose |
|---|---|
| `s1._domainkey.yourdomain.com` | DKIM signing key 1 |
| `s2._domainkey.yourdomain.com` | DKIM signing key 2 |
| `em####.yourdomain.com` | Return path / SPF alignment |

5. Add these in your DNS provider
6. Click **Verify** in SendGrid — all 3 should show green checkmarks

**Step 2b: Single Sender Verification (Quick Start Alternative)**
If you can't set up domain authentication yet:
1. **Settings** > **Sender Authentication** > **Single Sender Verification**
2. Enter from name, from email, reply-to, company address
3. Click verification link in your inbox

**Step 3: Create an API Key**
1. **Settings** > **API Keys** > **Create API Key**
2. Name: `iscale-lead-routing`
3. Permissions: **Restricted Access**
   - **Mail Send** > Full Access
   - **Tracking** > Read Access (for webhook events)
4. Copy the key (starts with `SG.`) — **you only see it once**

| Asset | Where It Goes |
|---|---|
| **API Key** (`SG.xxx...`) | Provider credentials `apiKey` |

**Step 4: Set Up Event Webhooks (Delivery Tracking)**
This enables bounce, open, click, and complaint tracking in iSCALE.

1. **Settings** > **Mail Settings** > **Event Webhook**
2. HTTP Post URL: `https://theleadrouter.com/messaging/api/webhooks/sendgrid`
3. Select events to track:
   - **Delivered** ✓
   - **Opened** ✓
   - **Clicked** ✓
   - **Bounced** ✓
   - **Spam Report** ✓
   - **Unsubscribed** ✓
   - **Dropped** ✓
   - **Deferred** ✓
4. Enable **Signed Event Webhook** for security
5. Copy the **Verification Key** — needed for webhook signature verification

| Asset | Where It Goes |
|---|---|
| **Webhook Signing Key** | Provider config or env var for webhook verification |

**Step 5: Add Provider in iSCALE**

*Option A — via Messaging Provider API (recommended):*
- Go to Messaging > Settings > Providers tab
- Click "Add Provider"
- Type: Email
- Provider: SendGrid
- Name: "SendGrid" (or any label)
- Credentials:
```json
{
  "apiKey": "SG.your-api-key-here"
}
```
- Config:
```json
{
  "fromEmail": "notifications@yourdomain.com",
  "fromName": "Your Company Name"
}
```
- Daily Send Limit: 100 (free tier) or your plan's limit
- Mark as Default if this is your primary email provider
- Click Save

*Option B — via Settings > SMTP tab:*
- Email Address: `leads@yourdomain.com`
- Host: `smtp.sendgrid.net`
- Username: `apikey` (literally the word "apikey")
- Password: Your API key (`SG.xxx...`)
- Port: `587`
- SSL: Enabled

**Step 6: Send a Test Email**
- Click the "Test" button and verify delivery

**SendGrid Assets Checklist:**
- [ ] SendGrid account created
- [ ] Domain authenticated (3 CNAME DNS records added + verified green)
- [ ] OR single sender verified
- [ ] API key created with Mail Send permissions
- [ ] API key saved (`SG.xxx...`)
- [ ] Event webhook URL configured with all events selected
- [ ] Webhook signing key saved
- [ ] DNS records propagated and verified

---

#### SES vs SendGrid Comparison {#ses-vs-sendgrid}

| | **SES** | **SendGrid** |
|---|---|---|
| **Cost** | ~$0.10/1K emails | Free 100/day, ~$20/mo for 100K |
| **Setup complexity** | Higher (IAM, SNS, Config Sets) | Lower (dashboard-driven) |
| **Sandbox period** | Yes, must request production access | No sandbox |
| **Webhook setup** | SNS topic + subscription | Single URL in dashboard |
| **Credentials needed** | Access Key ID + Secret Access Key | Single API key |
| **DNS records** | 3 CNAME (DKIM) + optional MX/TXT | 3 CNAME |
| **Best for** | High volume, already on AWS | Quick setup, lower volume |

#### DNS Records for Email Deliverability {#email-dns}

Regardless of which provider you choose, add these DNS records to improve deliverability:

**SPF Record** — add to your existing TXT record on the root domain:
```
v=spf1 include:amazonses.com include:sendgrid.net ~all
```

**DMARC Record** — create a TXT record on `_dmarc.yourdomain.com`:
```
v=DMARC1; p=none; rua=mailto:dmarc-reports@yourdomain.com
```

Start DMARC with `p=none` (monitor only). After confirming no legitimate emails fail, tighten to `p=quarantine`, then eventually `p=reject`.

---

#### Twilio SMS Setup {#twilio-sms-setup}

**Step 1: Create a Twilio Account**
- Go to twilio.com and sign up
- After signup, you'll get trial credits ($15)
- Note your Account SID and Auth Token from the Console Dashboard

**Step 2: Buy a Phone Number**
- Go to Phone Numbers > Manage > Buy a Number
- Search by area code or capabilities
- Ensure the number has SMS capability
- Click Buy ($1/month for local numbers)

**Step 3: Configure the Status Callback**
- Go to Phone Numbers > Manage > Active Numbers > click your number
- Under Messaging > "A MESSAGE COMES IN":
  - Webhook URL: `https://theleadrouter.com/messaging/api/webhooks/twilio`
  - HTTP Method: POST

**Step 4: Upgrade from Trial (for production)**
- Trial accounts can only send to verified numbers
- Go to Billing > Upgrade to add a payment method
- This unlocks sending to any phone number

**Step 5: Add Provider in iSCALE**
- Go to Messaging > Settings > Providers tab
- Click "Add Provider"
- Type: SMS
- Provider: Twilio
- Name: "Twilio SMS" (or any label)
- Credentials:
```json
{
  "accountSid": "AC...",
  "authToken": "your-auth-token",
  "fromNumber": "+12345678901"
}
```
- Daily Send Limit: Set based on your Twilio plan
- Mark as Default for SMS
- Click Save

**Step 6: Send a Test SMS**
- Click the "Test" button and enter a phone number

---

#### Bandwidth SMS Setup {#bandwidth-sms-setup}

Bandwidth provides direct-to-carrier SMS with high deliverability and competitive per-message pricing.

**Prerequisites:**
- A Bandwidth account at [dashboard.bandwidth.com](https://dashboard.bandwidth.com)
- A Messaging Application created in the Bandwidth dashboard
- At least one phone number assigned to the messaging application

**Step 1: Get API Credentials**
- Log into the Bandwidth dashboard
- Go to Account > API Users
- Note your **Account ID**, **API Token**, and **API Secret**

**Step 2: Create a Messaging Application**
- In the Bandwidth dashboard, go to Applications > Messaging
- Create a new Messaging Application
- Set the **Callback URL** to your messaging webhook endpoint
- Set **Inbound/Outbound Callback Credentials** (username/password) for webhook authentication
- Note the **Messaging Application ID**

**Step 3: Add the Provider in iSCALE**
- Go to Admin > System > Messaging Settings > Providers
- Click "Add Provider"
- Select **Bandwidth** as the provider type
- Enter credentials:
  - **Account ID** — from Bandwidth dashboard
  - **API Token** — API user token
  - **API Secret** — API user secret
  - **Messaging Application ID** — from step 2
  - **Callback Username** — matches dashboard callback credentials
  - **Callback Password** — matches dashboard callback credentials
- Set daily send limit
- Click Save

**Step 4: Send a Test SMS**
- Click the "Test" button and enter a phone number

**Tracking**: Custom tracking domain, tracking base URL, unsubscribe page URL.

**API Keys**: Create and manage API keys for programmatic access to the messaging API.

### Messaging Suppression {#messaging-suppression}

Messaging suppression prevents emails or SMS from being sent to specific addresses or phone numbers. Unlike the routing suppression list (which blocks leads from being processed), messaging suppression blocks message delivery only.

**Adding a suppression entry:**

| Field | Description |
|-------|-------------|
| Channel | `email` or `sms` — which channel to suppress |
| Value | The email address or phone number to suppress |
| Reason | Why suppressed: `bounce`, `complaint`, `unsubscribe`, or `manual` |

Suppressed contacts remain in the contact database but will not receive messages on the suppressed channel.

---

## Consent Tracking {#consent-tracking}

Consent tracking provides TCPA compliance verification by capturing proof that users actively interacted with partner lead forms before submitting. An embedded JavaScript pixel records keystrokes, mouse movements, clicks, scrolls, form interactions, and DOM snapshots, producing a consent certificate for each session.

### Enabling Consent

Consent is controlled by a per-tenant feature flag. A superadmin enables it via **Superadmin > Tenants > [tenant] > Feature Flags > Consent Tracking**. Once enabled, consent-related UI and API endpoints become available for that tenant.

On the vertical level, toggle **Require iSCALE Consent** in the vertical's Settings tab to enforce that all leads in that vertical must include a `consentCertUrl`.

### Installing the Pixel {#consent-pixel}

The consent pixel is a JavaScript snippet installed on partner lead form pages.

1. Go to **Partners > Campaigns > [campaign] > Posting Specs**
2. Copy the pixel script from the **Consent Pixel** section
3. The partner adds the script to their form page's HTML
4. When a user interacts with the form, the pixel captures interaction data and injects a hidden `consentCertUrl` field into the form submission

The pixel snippet is also visible on the **Partner Portal > Offers** page so partners can self-serve.

<!-- react-flow:consent-flow -->

### Posting Spec Consent Fields {#consent-posting-spec}

When consent is enabled for your tenant, the posting spec API response (`GET /api/v1/campaigns/:campaignId/posting`) automatically includes consent-related fields:

- **consentCertUrl** appears in `requiredFields` — partners must include the consent certificate URL in lead submissions
- **consent** object in the response includes:
  - `accountKey` — the tenant's consent tracking account key
  - `pixelScript` — ready-to-embed `<script>` tag with the account key pre-filled
  - `hiddenFieldName` — the hidden field name (`iscConsentCertUrl`) that the pixel auto-populates
  - `note` — setup instructions for partners
- **sampleBody** and **sampleCurl** include the `consentCertUrl` field

Partners can use the posting spec response to see exactly what's required and get the pixel snippet without visiting the admin UI.

### Viewing Consent Certificates {#consent-certificates}

**Location:** Compliance > Consent Certificates (admin sidebar)

The certificate list shows all captured consent sessions with:
- Certificate ID and creation date
- Claim status (Active, Claimed, Expired, Revoked)
- Associated lead (if claimed)
- Tenant

**Filters:** Filter by claim status or date range. The list is paginated.

Click a certificate to view its detail page.

### Certificate Detail {#consent-interaction-summary}

The detail page shows:

- **Metadata**: Certificate ID, created date, claim status, expiry, associated lead
- **Interaction Summary**: Aggregated counts of keystrokes, mouse movements, clicks, scrolls, form interactions, and DOM mutations
- **Snapshots**: DOM state captures taken at key moments during the session
- **Claim History**: Audit trail of when the certificate was claimed by a lead submission

### Consent on Lead Detail

**Location:** Leads > [lead] > Consent tab

If a lead was submitted with a `consentCertUrl`, the Consent tab shows:
- Certificate metadata and claim status
- Interaction summary counts
- Link to the full certificate detail page

If no consent certificate is associated, the tab shows an empty state.

### Filtering Leads by Consent {#lead-consent-status}

On the leads list, use the **Consent Status** filter to find:
- **Has Consent** — leads with an associated consent certificate
- **Missing Consent** — leads without a consent certificate

A consent badge column on the leads table shows whether each lead has consent.

### Certificate Statuses {#consent-claim-status}

| Status | Meaning |
|--------|---------|
| **Active** | Certificate created, awaiting claim by a lead submission |
| **Claimed** | Certificate verified and associated with a lead |
| **Expired** | Certificate was not claimed within the expiry window |
| **Revoked** | Certificate manually invalidated by an admin |

### Consent Indicators on Other Pages

- **Person Detail**: The leads table on a person's detail page shows consent badges for each lead
- **Call Detail**: Calls linked to leads with consent show a consent indicator

---

## Multi-Tenant Data Isolation {#multi-tenant-data-isolation}

All data in the system is isolated by tenant. Each tenant can only see, modify, and interact with their own data.

**What's protected:**
- **Lead routing** — campaigns, offers, buyers, and contracts are scoped to your tenant
- **Webhooks** — events only fire to your tenant's configured webhook URLs
- **Financial operations** — CPA conversions, charges, and credits only affect your tenant's buyers
- **Person/contact resolution** — person matching and merging is tenant-scoped
- **Deduplication** — duplicate detection only checks within your tenant's leads
- **Lead delivery** — delivery context and token data is tenant-isolated
- **Call routing** — tracking numbers, campaigns, and offers are filtered by tenant

**Superadmin access:** Superadmin users can view data across all tenants for administrative purposes.

---

## Superadmin {#superadmin}

The superadmin panel at [/superadmin](/superadmin) provides cross-tenant management for platform operators.

### Tenants {#tenants}

**Location:** [Superadmin](/superadmin) > **[Tenants](/superadmin/tenants)**

The tenant list is the roster of accounts on the platform. The **create** form takes only five fields:

| Field | Notes |
|---|---|
| Name | Display name, 255 characters max. Mirrored onto the paired organization record |
| Slug | URL-safe identifier — lowercase letters, numbers, hyphens. **Unique across all tenants** and mirrored onto the organization row. It does not appear in admin URLs; its one URL role is the public `?tenant=<slug>` parameter, which resolves a tenant on unbranded public pages |
| Plan | `starter`, `standard`, or `enterprise`. Recorded on the tenant and mapped onto the paired organization record at creation. It is **not** what grants services — per-service access follows the Service Access overrides below, and credit entitlements come from the tenant's subscription plan, not this field |
| Currency | `usd`, `gbp`, `eur`, or `cad`. Non-USD is rejected unless the deployment has non-USD tenants enabled |
| Domain | Optional custom domain (e.g. `custom.domain.com`). Blank uses the platform default domain |

Everything else — phone display format, default post-out phone format, abbreviation, feature flags, service overrides — is set **after** creation on [Tenant Detail](#tenant-detail):

| Field | Notes |
|---|---|
| Phone display format | How phone numbers render in that tenant's UI — formatted `(555) 123-4567`, 10 digits, or E.164 |
| Default phone format | Format used for buyer post-outs when a contract does not override it |

Deleting a tenant is a soft-delete to `inactive`. Related: [Per-Tenant Feature Flags](#per-tenant-feature-flags), [Tenant Approval](#tenant-approval-superadmin), and [Multi-Tenant Data Isolation](#multi-tenant-data-isolation).

### Tenant Detail {#tenant-detail}

**Location:** [Superadmin](/superadmin) > [Tenants](/superadmin/tenants) > (a tenant)

Everything on the tenant record plus the edit-only fields and the per-service overrides.

**Abbreviation** (edit-only — not on the create form) — a short label (max 8 characters) used in auto-generated artifacts, most visibly Google Sheet titles: with `abbreviation = QI`, a sheet for buyer John Doe on the IUL vertical is named `QI - John Doe - IUL`. Tenants without one fall back to the first two letters of the tenant name, uppercased.

**Service access** — a per-service override that decides how that tenant reaches a platform service:

| Setting | Effect |
|---|---|
| Default | Follows the plan / pay-as-you-go billing rules |
| Granted | Forces access on |
| Revoked | Blocks the service |
| BYO | Tenant uses their own credentials, billed at the reduced BYO rate instead of the standard per-minute rate — **not zero** |

Billing consequences of each setting, including how BYO usage still writes a ledger entry, are in [Superadmin: system config & service access](#platform-services-superadmin).

### Abandoned Signups {#abandoned-signups}

**Location:** Superadmin > Tenants (section below the tenant list)

The `/get-started` signup wizard silently captures progress as prospects move through it — company name, slug, chosen plan, promo code, preferred contact channel, name, email, and phone are saved on each step transition and when the email field loses focus, along with the deepest step reached (1–4). Rows are keyed to the visitor's browser, so refreshes update the same record instead of creating duplicates. The **Abandoned Signups** table lists these captures: by default it shows only **open** rows (drop-offs to follow up on), newest first; completing signup automatically flips a row to **converted** (new account) or **existing user** (linked to an existing login), and the status filter can show those or **dismissed** rows. Click **Dismiss** after following up to hide a row from the open list — dismissal is a soft status and never deletes the record. This data is pre-consent prospect PII: it is superadmin-only, for internal follow-up, and must never feed automated marketing.

Each capture also records **first-touch attribution**: the visitor's browser (parsed server-side from the request user agent — never trusted from the client), the referrer and full landing URL, and up to ten sub IDs read from the landing URL's query string (`subid1`–`subid10`, with `sub1` and `s1` style aliases accepted). Attribution is pinned to the *first* visit — the wizard snapshots it once and later visits from a different source never overwrite it. The table shows the **Browser** and **Referrer** columns (hover the referrer for the full URL) plus a sub-ID count pill whose tooltip lists each `subId=value` pair. When a captured prospect completes signup, the attribution is stamped onto the new tenant's settings as `signupAttribution` (referrer, landing URL, browser, sub IDs, and when it was captured), so the traffic source that produced a paying tenant stays queryable after conversion.

---

### Marketing Analytics {#superadmin-marketing-analytics}

**Location:** Superadmin > Analytics

First-party analytics for the theleadrouter.com marketing site. Visitors to marketing pages get two first-party cookies: a **visitor id** (13 months) and a **session id** (rolling 30-minute expiry); a lightweight beacon records a pageview on every marketing route change. The Analytics page pivots those pageviews over a date range (default last 7 days, capped at 92 days, **UTC days**) by Day, Path, Referrer Host, Browser, Sub ID 1, or UTM Source, with four metrics per row plus a totals row: **Pageviews**, **Unique Visitors** (distinct visitor ids), **Wizard Starts** (get-started wizard captures linked by visitor id), and **Signups** (those captures that converted). The same visitor/session ids flow into abandoned-signup captures, onto the converted tenant's `signupAttribution`, and out through affiliate postback macros (`{visitor_id}`/`{session_id}`), so an affiliate's Everflow transaction id and our marketing user id can be matched in both directions.

**Cookie / PII note:** tracking is first-party only — no third-party pixels and no cross-site anything. The beacon payload carries no identifiers (ids are read server-side from the cookies, so they cannot be spoofed per event), bot user agents are discarded, and visitor IP addresses are stored only as SHA-256 hashes — the raw IP is never persisted.

---

### Affiliate Referrals {#affiliate-referrals}

**Location:** Superadmin > Referrals

Platform affiliates are people or companies who refer **tenants** into Lead Router (distinct from the tenant-level referrer program, which refers buyers into a tenant). Each affiliate has a globally unique 8-character referral code — a visitor arriving with `?ref=CODE` on any marketing page is tracked with a 30-day cookie, and completing signup stamps the new tenant with the referring affiliate. Payout terms are per affiliate, chosen at creation and editable later: a **one-time signup fee** (launch preset $50, paid when the referred tenant's *first* subscription payment clears — not on mere signup), a **monthly recurring fee** (launch preset $20, accrued on each *paid* renewal while the tenant stays active — past-due or canceled tenants accrue nothing), and an optional **rev-share** in basis points on all ex-tax platform revenue from referred tenants (subscriptions, credit purchases, auto-recharge; default off, reserved for volume-partner deals). Commissions accrue to the affiliate's balance; a monthly payout cycle sweeps positive balances into pending payout rows, and refunds/disputes claw accrued commissions back. Pausing an affiliate keeps attribution working but stops all accrual; deactivating stops the code resolving entirely.

**Funnel postbacks** let an affiliate's own tracking platform (e.g. Everflow) receive server-side GET pings as referred visitors move through the signup funnel. Configure per-event postback URLs on the affiliate's Settings tab — one row per stage (`wizard_started`, `email_captured`, `plan_selected`, `signup_completed`, `first_payment`), each with an on/off toggle and a **Test** button that fires the URL with sample values. Postbacks are fire-and-forget: they never delay or fail a signup, and every attempt (success or failure, with the final fired URL) is logged. Stages fire exactly once per prospect — re-posting the same step, email, or plan never re-fires. Macros in the URL template fill per fire; unknown macros are left as-is so template typos are visible in the log:

| Macro | Fills with |
|---|---|
| `{transaction_id}` | The visitor's `subId1`, falling back to the wizard client key (Everflow convention) |
| `{sub1}` … `{sub10}` | Sub IDs captured from the affiliate's landing link (`subid1`–`subid10`) |
| `{client_key}` | The wizard's browser-scoped capture key |
| `{email}` | The captured email (URL-encoded) |
| `{step}` | Deepest wizard step reached (1–4) |
| `{tenant_id}` | The new tenant's ID (`signup_completed` onward) |
| `{amount_cents}` | First payment amount in cents (`first_payment` only) |

The public program pitch lives at [/affiliates](/affiliates); prospective affiliates apply by email and are set up manually by a superadmin (no self-serve portal in v1).

---

### Per-Tenant Feature Flags {#per-tenant-feature-flags}

**Location:** Superadmin > Tenants > [tenant name] > Home tab > Feature Flags

Feature flags control which modules are available to each tenant. The system uses a two-level merge:

1. **Global setting** (Settings > System Settings) — master switch for the entire platform
2. **Tenant override** (Superadmin > Tenant detail) — per-tenant toggle

A feature is enabled for a tenant only when **both** conditions are met:
- The global system setting is ON
- The tenant's flag is explicitly set to ON

If a tenant flag is unset (never toggled), the feature defaults to **OFF**. Tenants must explicitly opt in to each feature.

**Available feature flags:**
- **Finance** — transactions, refunds, and payout management
- **Compliance** — suppression lists, block lists, and data deletion
- **Reports** — lead reports, revenue reports, and custom analytics
- **Messaging** — email/SMS marketing module in the sidebar
- **Calls** — call tracking, recordings, and call routing (filters from People submenu in admin and top-level in buyer dashboard)
- **Consent** — TCPA consent tracking via embedded pixel on partner lead forms

**To toggle a feature for a tenant:**
1. Go to **Superadmin > Tenants**
2. Click the tenant name
3. On the Home tab, find the **Feature Flags** section
4. Toggle the switch for the desired feature
5. The change takes effect on the tenant's next login

**Note:** Toggling a feature OFF at the tenant level hides the module from that tenant's sidebar and disables related API endpoints, even if the global setting is ON. To disable a feature for ALL tenants at once, turn off the global system setting instead.

---

### SMS Provider Resolution {#sms-provider-override}

**Location:** System > Providers (credentials + per-tenant capability assignments)

By default, every tenant's outbound contract SMS deliveries flow through the platform's shared **system Twilio** account. A tenant that runs its own SMS infrastructure can be pointed at its own **SignalWire** credential instead, without affecting any other tenant.

<!-- react-flow:sms-provider-resolution -->

**How resolution works:**

When a lead is routed to a contract that has an SMS delivery action, the engine resolves the tenant's `sms_delivery` credential:

1. If the tenant has an **active** tenant-scoped `signalwire` credential with a `config.spaceHost`, SignalWire is used — POST to `https://{spaceHost}/api/laml/2010-04-01/Accounts/{projectId}/Messages.json`
2. If no tenant credential exists or it is **inactive**, the engine falls back to the system Twilio default with no behavior change
3. If neither is configured, the delivery is logged as `SMS_PROVIDER_MISCONFIGURED` and fails

Resolution is invisible to tenants and to the contract — there is no per-contract or per-offer setting. The credential and its `providerAssignment` row are the entire switch.

**Which assignment wins:** the unified resolver selects the `providerAssignment` marked `isDefault = true` with `status = 'active'` for that (tenant, capability) pair. An assignment with `isDefault = false` is never selected, regardless of scope.

**Security:**

- Credential secrets are stored encrypted at rest using AES-256-GCM (`iv:tag:cipher` format). They are never returned in plaintext from the API — masked previews show only the first/last few characters
- Mutations are tracked in `auditLog` with `entityType='telephonyCredential'`, `action='create'|'update'|'delete'`, the acting user's ID, and the credential ID
- Test-send endpoints are rate-limited to 10 calls per minute per user to prevent accidental abuse

**Disabling or rolling back:**

Set the tenant's credential (or its assignment) to **inactive**. The tenant's SMS returns to the system Twilio default on the next delivery — there is no caching layer to flush and no deploy required.

**Privacy / compliance note:**

When a tenant-scoped SignalWire credential is active, the recipient's phone and the message body pass to that tenant's own SignalWire project for fulfillment, and the platform's shared Twilio is no longer in the path for that tenant's SMS. Either way the sending number must carry its own A2P 10DLC brand and campaign registration — unregistered traffic is filtered by the carriers.

---

## Tenant Signup {#tenant-signup}

New customers can self-register as tenants through the signup wizard. Superadmins control whether signup is enabled and how new tenants are handled.

### Self-Service Signup

The signup page at `/get-started` walks new customers through a 4-step wizard:

1. **Company** — enter company name and URL slug. The slug is auto-generated from the company name, can be edited manually, and checks availability in real time. This step also picks verticals and a preferred contact channel — choosing **Slack**, **Telegram**, or **Discord** asks for the matching handle (Slack account email, @username, or Discord username) so the team can reach you there; **Email** reuses the account email.
2. **Plan** — the **Lead Router** plan: **$97/month with 1,000 credits/month included**, alongside a what-credits-buy rates table (see [Tenant Plan, Credits & Auto-Recharge](#tenant-plan-credits)). A **promo code** field accepts platform coupon codes with an **Apply** button that validates instantly — a valid code shows its discount (e.g. "$50 off your first month"), an unknown code shows an error but never blocks signup. Codes can also arrive pre-filled via the link (see below).
3. **Account** — create the tenant's first admin user (name, email, and a **required** phone number).
4. **Review** — confirm all details and submit.

On submission, the system creates the tenant, the admin user, and logs the user in (or shows a pending-approval message if manual approval is required).

**Existing email?** If the email already has a Lead Router login, signup doesn't reject it — the new company is created and linked to the existing account as its owner (no duplicate user, no automatic login). The user receives a "your company is ready — sign in" email; after signing in they finish the subscription on the standalone **Activate your workspace** page. The product shell stays locked until checkout completes.

### Plan Checkout at Signup {#signup-plan-checkout}

Right after account creation (auto-approve mode), the wizard sends the new admin straight into **Stripe subscription checkout** to enter a card for the $97/month plan. Paying activates the tenant's monthly credit balance — the payment webhook grants the 1,000 included **subscription credits** and resets them on each paid invoice (see [Two credit pools](#tenant-plan-credits)).

- **After payment** — a short one-time onboarding sequence: pick your **auto-recharge bundle** (required — see [Auto-recharge](#auto-recharge)), choose a **[support tier](#support-tiers)** (Community is free), then land on the **setup checklist** at `/admin/setup`.

- **Promo codes** — a valid code entered in the wizard (or carried on the signup link as `/get-started?promo=CODE`) is auto-applied as a discount at checkout. The wizard's **Apply** button pre-validates the code so users see the discount before submitting. The Stripe checkout page also accepts codes typed directly. An invalid code never blocks signup — checkout simply proceeds without the discount.
- **Abandoned checkout** — closing Stripe leaves authentication available but keeps the tenant outside the product. Any `/admin` page redirects to `/activate-account`; canceling Stripe returns there with the selected plan and promo code preserved.
- **Manual approval mode** — no checkout happens pre-approval. After approval, the admin signs in and completes checkout on `/activate-account`.
- **Shareable activation link** — send `https://theleadrouter.com/activate-account?promo=CODE`. Signed-out admins authenticate first, then return to the same coupon-prefilled activation page. Only successful subscription activation unlocks the product.

<!-- react-flow:tenant-billing-flow -->

### Signup Settings (Superadmin)

Manage signup behavior at **Superadmin > Settings**:

- **Enable signup** — toggle self-service signup on or off. Disabled by default.
- **Approval mode** — **Auto** (tenants are activated immediately on signup) or **Manual** (tenants are created with `pending` status and require superadmin approval).
- **Require terms** — when enabled, the signup form shows a terms-of-service checkbox that must be accepted.
- **Terms URL** — the link to your terms of service page, shown alongside the checkbox.

### Tenant Approval (Superadmin)

When approval mode is set to **Manual**:

1. New signups create a tenant with `pending` status.
2. The user sees a "pending approval" confirmation screen and cannot log in until approved.
3. Go to **Superadmin > Tenants** to see pending tenants (indicated by a yellow badge).
4. Click **Approve** to activate the tenant, or **Reject** to deactivate it.
5. Rejecting or deactivating a tenant also deactivates all users within that tenant.

### Signup URL

The signup URL is displayed in two places:

- **Superadmin > Tenants** — a banner at the top of the tenant list with a Copy button.
- **Superadmin > Settings** — in the signup settings card (visible when signup is enabled).

Share this URL with potential customers to let them self-register.

### AI Model Watchdog (Superadmin) {#ai-model-watchdog}

The platform pins its Anthropic model ids in one central registry (dateless aliases, so snapshot releases apply automatically). A weekly watchdog cron (Mondays 09:00 UTC) checks every pinned model against Anthropic's live model list and posts a Slack alert if a pinned model has been retired or a newer generation in the same family is available. The watchdog never switches models on its own — alerts are informational, and model changes are made deliberately in code (registry + pricing table together).

---

## Glossary

| Term | Definition |
|------|------------|
| **Vertical** | Category of leads (e.g., Auto Insurance) |
| **Vertical Group** | Grouping of verticals (e.g., "Insurance"). Managed at [/admin/vertical-groups](/admin/vertical-groups). Each group owns its own disposition codes and scopes intake center disposition mappings. |
| **Partner** | Partner/source generating leads |
| **Campaign** | Partner program sourcing leads |
| **Buyer** | Company purchasing leads |
| **Contract** | Buyer agreement for lead delivery |
| **Offer** | Distribution rules linking campaigns to contracts |
| **Distribution** | Process of routing leads to buyers |
| **Intake Center** | Call center entity bundling delivery config + disposition mappings |
| **Disposition** | Lead status/outcome |
| **Dedup** | Duplicate detection to prevent repeat purchases |
| **TCPA** | Telephone Consumer Protection Act compliance |
| **Cap** | Limit on leads or spending |
| **Cherry Pick** | Manual lead assignment |
| **Token Interpolation** | Placeholder syntax (`{token}`) in delivery templates replaced with real data at send time |
| **Response Mapping** | Rules that interpret buyer HTTP response content into delivery outcomes |
| **Delivery Outcome** | Classification of a buyer's response (success, reject, duplicate, etc.) |
| **Tracking Number** | Phone number purchased from Twilio/Telnyx, assigned to a campaign and offer for inbound call routing |
| **IVR** | Interactive Voice Response — automated phone menu that collects DTMF keypress input before routing |
| **Whisper** | Audio prompt played to the buyer before connecting with the caller; buyer can accept or reject |
| **Bridge** | Two-leg call connection between caller and buyer |
| **Call Billing Rule** | Advanced rule that controls buyer charges or partner payouts for calls |
| **People Hub** | Unified person view aggregating leads, calls, and revenue across verticals |
| **Lead Session** | Progressive lead capture session that accumulates data across multiple funnel steps before submission |
| **Cross-Partner Dedup** | Prevents the same person from being sold when submitted by different partners |
| **Multi-Source** | Badge indicating a person was submitted by more than one partner |
| **Tenant Isolation** | Row-level data separation ensuring each tenant can only access their own records |
| **Consent Certificate** | TCPA compliance record proving a user actively interacted with a lead form before submitting |
| **Consent Pixel** | JavaScript snippet on partner forms that captures interaction data and generates consent certificates |
| **Interaction Summary** | Aggregated counts of user interactions (keystrokes, clicks, scrolls, etc.) in a consent session |

---

## Audit Trail / Activity Log {#audit-trail--activity-log}

### Global Audit Log

Navigate to **Admin > Audit Log** to see all system activity. Filter by entity type or action.

### Per-Entity Activity

Every detail page (Buyers, Contracts, Partners, Campaigns, Offers, Verticals, Leads, Webhooks, People, Users, Contacts, Integrations, Refunds, Transactions, Payouts) includes an **Activity** tab or section showing the audit trail for that specific record.

- **Timestamp**: When the action occurred
- **Action**: Created, Updated, Deleted, Status Changed
- **User**: Who performed the action
- **Details**: Click to expand the full JSON details of what changed

All mutations (creates, updates, deletes, status changes) across the system are automatically logged.

---

## Email Send Log & Delivery Tracking {#email-send-log}

Every transactional email the platform sends (login links, invitations, notifications) is recorded in the **Messaging Log** report with its full lifecycle status.

### Statuses

| Status | Meaning |
|---|---|
| `queued` | Send attempt recorded, provider not yet confirmed |
| `sent` | The email provider (Resend/SMTP) accepted the message |
| `delivered` | Resend confirmed the recipient's mail server accepted it |
| `opened` | The recipient opened the email (Resend open tracking) |
| `clicked` | The recipient clicked a link in the email — implies opened |
| `bounced` | The recipient's mail server rejected it — the rejection reason is stored on the row |
| `complained` | The recipient marked it as spam |
| `suppressed` | Resend refused to attempt delivery — the address is on its suppression list (usually after an earlier bounce) |
| `failed` | The send failed — provider config rejected it, or Resend reported a post-accept failure (e.g. invalid recipient) |

`delivered`, `opened`, `clicked`, `bounced`, `complained`, `suppressed`, and post-accept `failed` come from Resend delivery webhooks — they reflect what actually happened after the provider accepted the email, not just that we handed it off. If Resend reports a delivery delay, the reason is stamped on the row's error message (`Delivery delayed: …`) without changing the status. Note: click tracking is intentionally OFF for the transactional domain — login links must not be link-rewritten.

### Bounce Alerts & Suppression

When an email bounces, gets a spam complaint, or is suppressed, a Slack alert fires immediately with the recipient, template, and reason.

**Why this matters:** after a hard bounce, Resend automatically adds the address to its suppression list — every future email to that address (including login links) is dropped while our send API calls still succeed. Those drops now show as `suppressed` rows in the Messaging Log. To fix one, remove the address from the suppression list in the Resend dashboard. There is no API for suppression removal — it is dashboard-only.

### User Activity Timeline {#user-activity-timeline}

Every user detail page (**Admin > Users > [user]** and **Superadmin > Users > [user]**) has an **Activity** tab — a single timeline merging three sources, newest first:

- **Logins** — every login with browser + IP, including **failed attempts** (expired or already-used login links). Magic-link, Google, and passkey logins all appear.
- **Emails** — every email sent to the user's address with its true delivery status (`delivered`, `bounced`, `suppressed`, …) and the failure reason when there is one.
- **Changes** — audit events about the user (role changes, session revocations, login-link events) and actions the user performed.

Filter chips narrow the timeline to one source. Use this tab first when a user reports login or email trouble — a `bounced`/`suppressed` email row or a string of failed login attempts tells you the story immediately.

---

## Postman Collection

**Location:** Help > Postman Collection tab

Downloadable Postman collections are available for testing the API, scoped per role: partners see the Partner API collection, buyers see the Buyer API collection, and admins see both. Each collection card also offers **Production Environment** and **Local Environment** file downloads with `baseUrl` pre-filled.

### How to Use

1. Go to **Help** and click the **Postman Collection** tab
2. Click **Download Collection** to get the JSON file (optionally download an environment file too)
3. Import into Postman (File > Import)
4. Set collection variables: `baseUrl` (your server URL) plus the auth key for your collection — partner collection uses the posting key (`pk_xxx`) for lead submission and a partner API key for `/api/partner/*`; buyer collection uses a portal key (`lr_xxx`, created under Settings > API Keys) as `portalKey`
5. All field names and list values are case-sensitive

### Included Endpoints

The partner collection covers lead submission (submit/ping/post) plus the `/api/partner/*` surface; the buyer collection covers the full `/api/buyer/*` surface (calls, availability, contracts, delivery log, people, posting log, engagements, SKUs, subscriptions, users, webhook endpoints, API keys, lead completion). Each request includes example bodies and parameter descriptions.

### OpenAPI Spec Downloads

Partner and buyer portal users also get **YAML** and **JSON** download buttons at the top of Help > API Reference — a machine-readable OpenAPI spec scoped to their own API surface, suitable for codegen tools, Postman import, or AI agents. All spec and collection downloads require a signed-in session (unauthenticated requests return 404).

---

## Posting Log {#posting-log}

The Posting Log records every lead submission attempt across all API endpoints, whether it succeeds or fails. This gives you complete visibility into what partners are sending and what happened to each request.

### What Gets Logged

Every POST to these endpoints is captured:
- **/api/v1/leads/submit** — Partner lead submission with a `pk_xxx` posting key
- **/api/v1/leads/ping** — Ping requests (check matches without committing)
- **/api/v1/leads/post** — Post requests (convert a ping into a lead)

Each log entry records: the endpoint called, HTTP status, response time, API key used, campaign/partner/offer context, the full request and response payloads, and the outcome.

### Outcomes

- **success** — Lead was created and sold to one or more buyers
- **rejected** — Lead was valid but no buyers matched (or cross-partner dedup)
- **validation_error** — Request had bad data (missing TCPA, suppressed email/phone, Zod validation failure)
- **auth_error** — Invalid or inactive API key
- **server_error** — Unexpected failure during processing

### Admin View

Navigate to **System > Posting Log** in the admin sidebar. The list view shows all submissions with filters for endpoint, outcome, date range, and error search. Click any row to see full request/response JSON, caller IP, user agent, and links to related entities (campaign, partner, offer, lead).

### Resending a Failed Lead

Lead-type entries with an actionable outcome (`rejected`, `validation_error`, `auth_error`, `server_error`) show a **Resend** button on the detail page. Resend re-runs the original request body through the routing pipeline, generating a brand-new posting log entry — the original is preserved and gets a `resend` row in its Change History so the action is auditable.

The Resend modal lets you edit the request body before resending, and exposes one validation override:

- **Skip TrustedForm validation** — bypasses the "TrustedForm certificate required" gate when the vertical's TCPA settings require a TrustedForm cert URL but the lead doesn't have one. Use this only when you've verified consent through another channel (e.g., the partner sent a different consent cert, or you've manually reviewed the call recording). Every other gate — TCPA consent flag, Jornaya, iScale consent, suppression, schema, dedup, filters — still runs.

When the override is used, both the audit log entry on the original posting log (`details.skipTrustedFormValidation: true`) and the new posting log entry (yellow "Admin override applied on resend" banner above the Request Body card) record the bypass. Partner-facing wire responses are unaffected; the override marker is internal only.

Call-type entries cannot be resent — the Resend button is hidden.

### Partner Portal View

Partners see their own posting log at **Posting Log** in the partner sidebar, automatically filtered to their offers. The log renders an "Offer" column resolved through the underlying campaign's linked offer. This helps partners debug integration issues and see exactly what their systems are sending.

### Call Log

The Call Log records every inbound call webhook event, including calls that fail before a call record is created. Access it from **Logs > Call Log** in the sidebar, or from the Posting Log page by filtering to **Type: Call**.

Each entry captures:
- **Outcome** — success (call record created), rejected (tracking number not found, duplicate), auth_error (invalid webhook signature), validation_error (malformed payload), or server_error
- **Provider** — Twilio, Telnyx, or Bandwidth
- **Caller Number** and **Tracking Number** — the phones involved
- **Call link** — direct link to the call record (when successfully created)
- **Full webhook payload** — the raw request body from the telephony provider

Use the Call Log to:
- Debug why an expected call didn't appear in the system
- Identify invalid webhook signatures from misconfigured providers
- Spot tracking numbers that aren't assigned or recognized
- Track duplicate webhook deliveries from telephony providers

---

## Default Lead Fields

### Field Name Customization {#field-name-customization}

The Default Lead Fields settings page lets admins customize the field names that partners use when submitting leads via the API.

**How it works:**
- Each field has an **Internal Field** (the database column name, e.g. `firstName`) and a **Partner Field Name** (what partners send in their API payload, e.g. `first_name`)
- When a partner submits a lead, the system automatically remaps partner field names to internal names before processing
- Core fields (firstName, lastName, email, phone) always map to their database columns — you can only change the partner-facing name
- Custom fields (added by admins) are stored in the lead's custom data bucket

**Example:**
If you set the partner field name for `firstName` to `first_name`, partners can POST:
```json
{ "first_name": "John", "email_address": "john@test.com" }
```
The system remaps this to `{ "firstName": "John", "email": "john@test.com" }` internally.

**Validation rules:**
- Partner field names cannot duplicate each other
- Partner field names cannot use system field names (verticalId, tcpaConsent, sub1-sub10, etc.)
- The posting documentation endpoint automatically shows the correct partner field names

---

### Setup Token (AI Onboarding) {#setup-token}

Setup tokens let you onboard partners with a single line. Instead of sending API keys, campaign IDs, and integration docs separately, generate a one-use link that does everything:

1. Go to **Partners > Campaigns > [Campaign] > Posting** tab
2. Click **Generate Setup Link** (after loading posting specs)
3. Copy the one-liner and send it to your partner
4. Their AI coding assistant fetches the URL and receives:
   - Auto-provisioned API key (scoped to partner)
   - Campaign ID and field mapping
   - Field mapping with sample payloads
   - Test commands to verify integration
5. Token is consumed on first use (single-use, 24h expiry)

**What the partner sees:** Plain markdown with credentials, field tables, sample JSON, and cURL commands. Any AI tool (Claude Code, Cursor, Windsurf) can parse and execute it.

**Revoking:** If you revoke a setup token, its provisioned API key is also revoked.

**Expiry:** Unused tokens expire after 24 hours. The cleanup cron handles this automatically.

---

## Use Cases & Configuration Guide {#use-cases}

This section contains 100 real-world scenarios organized by category. Each use case describes a business situation, the recommended system configuration, and why those settings work. Use this as a reference when setting up new verticals, offers, or contracts — or share it with an AI assistant to get configuration recommendations for your specific situation.

**How to use this section:** Find the category that matches your situation, then look for the use case closest to yours. The recommended settings are starting points — adjust based on your specific business rules.

---

### Distribution & Routing {#uc-distribution}

**Use Case 1: Exclusive legal leads — highest bidder wins**

> A personal injury vertical where each lead goes to exactly one law firm. The firm willing to pay the most gets the lead.

| Setting | Value | Why |
|---|---|---|
| Offer > Distribution Type | `exclusive` | One buyer per lead |
| Offer > Distribution Rule | `price` | Highest-paying contract wins |
| Offer > Max Sales | `1` | Enforce exclusivity |

---

**Use Case 2: Multisell home services — sell to up to 4 contractors**

> A home improvement vertical (roofing, HVAC) where homeowners expect multiple quotes. Sell the same lead to up to 4 contractors.

| Setting | Value | Why |
|---|---|---|
| Offer > Distribution Type | `multisell` | Multiple buyers per lead |
| Offer > Max Sales | `4` | Cap at 4 buyers |
| Offer > Distribution Rule | `price` | Highest payers get first access |

---

**Use Case 3: Round-robin distribution for equal partners**

> An insurance vertical with 3 buyers who all pay the same price. Distribute leads evenly so no one buyer gets more than the others.

| Setting | Value | Why |
|---|---|---|
| Offer > Distribution Type | `exclusive` | One buyer per lead |
| Offer > Distribution Rule | `round_robin` | Alternates between contracts in rotation |
| Contract A/B/C > Price Per Lead | Same amount | Equal pricing keeps it fair |

**Note:** The system tracks a `roundRobinIndex` per offer. If a buyer pauses their contract, they're skipped (not counted) — when they unpause, they resume their position.

---

**Use Case 4: Weighted distribution — 60/40 split between two buyers**

> You have two buyers. Buyer A wants 60% of volume, Buyer B wants 40%. Both pay the same price.

| Setting | Value | Why |
|---|---|---|
| Offer > Distribution Rule | `weight` | Probabilistic distribution |
| Offer-Contract link > Buyer A Weight | `3` | 3 out of 5 = 60% |
| Offer-Contract link > Buyer B Weight | `2` | 2 out of 5 = 40% |

**Note:** Weights are ratios, not percentages. 3:2 = 60:40. Over small volumes the split may vary; it converges over hundreds of leads.

---

**Use Case 5: Priority-based distribution — preferred buyer gets first look**

> You have a premium buyer who should always get leads first. If they can't accept (capped, paused, filtered), fall through to backup buyers.

| Setting | Value | Why |
|---|---|---|
| Offer > Distribution Rule | `priority` | Highest priority contract evaluated first |
| Contract (Premium Buyer) > Priority | `100` | Evaluated first |
| Contract (Backup Buyer 1) > Priority | `50` | Second choice |
| Contract (Backup Buyer 2) > Priority | `10` | Last resort |

---

**Use Case 6: Hybrid distribution — exclusive first, then multisell overflow**

> Your top buyer wants exclusive access. Any leads they reject or can't accept (caps full) should be sold to multiple backup buyers.

| Setting | Value | Why |
|---|---|---|
| Offer > Distribution Type | `hybrid` | Exclusive first pass, multisell fallback |
| Offer > Max Sales | `3` | If exclusive buyer passes, sell to up to 3 others |
| Contract (Exclusive Buyer) > Priority | `100` | Gets first look |
| Contract (Backup Buyers) > Priority | `50` | Evaluated after exclusive pass |

---

**Use Case 7: Simultaneous call routing — all buyers ring at once**

> An inbound call center where you want all available buyers' phones to ring simultaneously. First to answer wins.

| Setting | Value | Why |
|---|---|---|
| Offer > Call Distribution Rule | `simultaneous` | All contracts ring at once |
| Offer > Call Distribution Type | `sequential` | N/A for simultaneous |
| Contract > Call Ring Timeout | `30` seconds | How long to ring before giving up |

---

**Use Case 8: Sequential call routing — try buyers in order**

> Premium buyers should get calls first. If they don't answer within 15 seconds, try the next buyer.

| Setting | Value | Why |
|---|---|---|
| Offer > Call Distribution Rule | `priority` | Try highest priority first |
| Offer > Call Distribution Type | `sequential` | One at a time |
| Contract > Call Ring Timeout | `15` seconds | Short timeout to try next buyer quickly |

---

**Use Case 9: Caller affinity — return callers go to same buyer**

> If a caller previously connected with Buyer A, route their next call to Buyer A again for continuity.

| Setting | Value | Why |
|---|---|---|
| Offer > Call Caller Affinity Mode | `same` | Same caller → same buyer |
| Offer > Call Affinity Window Days | `30` | Look back 30 days for prior calls |

---

**Use Case 10: Caller exclusion — never route to same buyer twice**

> You want callers to always reach a new buyer (e.g., getting multiple quotes by phone).

| Setting | Value | Why |
|---|---|---|
| Offer > Call Caller Affinity Mode | `exclude` | Same caller → different buyer |
| Offer > Call Affinity Window Days | `90` | Exclude for 90 days |

---

### Pricing & Revenue {#uc-pricing}

**Use Case 11: Fixed pricing — flat rate per lead**

> Every lead costs the same regardless of buyer, state, or data quality.

| Setting | Value | Why |
|---|---|---|
| Contract > Pricing Model | `fixed` | Static price |
| Contract > Price Per Lead | `$25.00` | Flat rate |

---

**Use Case 12: Ping-post pricing — buyer sets price per lead**

> Buyers evaluate each lead and bid a price. The system collects bids from ping/post contracts, combines them with fixed-price contracts and estimated CPA revenue, then ranks all contracts by total auction price to determine the winner.

<!-- react-flow:ping-post-auction -->

| Setting | Value | Why |
|---|---|---|
| Contract > Pricing Model | `ping_post` | Two-step: ping for bid, post on acceptance |
| Contract > Ping Delivery Config | URL, auth, field mapping | Separate endpoint for ping |
| Contract > CPA Lookback Days | `30` | Window for avg CPA revenue per lead |
| Contract > CPA Lookback Exclude Days | `2` | Exclude recent days (conversion lag) |
| Offer > Distribution Rule | `price` | Highest auction price wins |

---

**Use Case 13: Exclusive premium with multiplier**

> A buyer pays $20/lead normally (multisell) but $50 for exclusive access.

| Setting | Value | Why |
|---|---|---|
| Contract > Price Per Lead | `$20.00` | Base price |
| Contract > Exclusive Multiplier | `2.5` | $20 x 2.5 = $50 for exclusive |
| Offer > Distribution Type | `hybrid` | Supports both exclusive and multisell |

---

**Use Case 14: CPA-based revenue — pay on conversion**

> Buyer pays nothing upfront but pays $500 when a lead converts (e.g., signs a retainer). Track via disposition postback.

| Setting | Value | Why |
|---|---|---|
| Contract > Price Per Lead | `$0.00` | No upfront cost |
| Contract > Revenue On CPA | `true` | Revenue when CPA fires |
| Contract > CPA Amount | `$500.00` | Conversion payout |
| Contract > CPA Window Days | `90` | Must convert within 90 days |
| Vertical Group Disposition > Triggers CPA | `true` on the "Signed" disposition | Marks the CPA event |

CPA conversions are reversible — see [CPA Reversal on Return](#cpa-reversal-on-return). When a later disposition with `triggersReturn=true` fires (e.g., "Unsigned" or "Refunded"), the system credits the buyer back the `cpaAmount`, debits the partner the `cpaPayout`, decrements `lead.cpaRevenue`/`lead.cpaPayout`, and writes offsetting `cpa_reversal` rows to the transaction ledger. The full audit trail lives on the lead row (`leadSale.cpaStatus = 'reversed'`, original `cpaAmount`/`cpaPayout` preserved) and in the transaction list filtered by `type=cpa_reversal`. Use the disposition's **Return Scope** select to scope the reversal to CPA only, CPL only, or both.

---

**Use Case 15: Hybrid pricing — upfront fee + CPA bonus**

> Buyer pays $15/lead delivered, plus an additional $200 bonus if the lead converts.

| Setting | Value | Why |
|---|---|---|
| Contract > Price Per Lead | `$15.00` | Upfront per-lead fee |
| Contract > Revenue On Acceptance | `true` | Book $15 on delivery |
| Contract > Revenue On CPA | `true` | Book additional on conversion |
| Contract > CPA Amount | `$200.00` | Conversion bonus |

---

**Use Case 16: Partner payout — fixed cost per lead submitted**

> Pay your partner $5 for every lead they submit, regardless of whether it sells.

| Setting | Value | Why |
|---|---|---|
| Campaign > Pricing Model | `fixed` | Static cost |
| Campaign > Cost Per Lead | `$5.00` | Partner earns per submission |

---

**Use Case 17: Partner payout — revenue share**

> Partner gets 40% of whatever the lead sells for.

| Setting | Value | Why |
|---|---|---|
| Campaign > Pricing Model | `revshare` | Percentage-based |
| Campaign > Cost Per Lead | `0.40` | 40% of sale price |

---

**Use Case 18: Partner CPA payout**

> Partner gets paid only when their lead converts, not on submission.

| Setting | Value | Why |
|---|---|---|
| Campaign > Pricing Model | `fixed` | Base model |
| Campaign > Cost Per Lead | `$0.00` | No upfront cost |
| Campaign > CPA Cost | `$100.00` | Paid on CPA conversion |
| Campaign > CPA Pricing Model | `fixed` | Flat CPA payout |

---

**Use Case 19: Balance enforcement — buyers must prepay**

> Only route leads to buyers who have sufficient balance. No credit.

| Setting | Value | Why |
|---|---|---|
| System Settings > Balance Check Enabled | `true` | System-wide enforcement |
| Offer > Balance Check Enabled | `true` | Per-offer enforcement |
| Buyer > Balance | Must be > 0 | Prepaid balance |
| Buyer > Credit Limit | `$0.00` | No credit allowed |

---

**Use Case 20: Auto-recharge buyer accounts**

> Buyer's account auto-refills when balance drops below a threshold.

| Setting | Value | Why |
|---|---|---|
| Buyer > Auto Recharge | `true` | Enable auto-refill |
| Buyer > Auto Recharge Threshold | `$100.00` | Trigger when below $100 |
| Buyer > Auto Recharge Amount | `$1,000.00` | Add $1,000 per recharge |

---

### Caps & Volume Control {#uc-caps}

**Use Case 21: Daily lead cap — buyer wants max 50 leads/day**

| Setting | Value | Why |
|---|---|---|
| Contract > Daily Lead Cap | `50` | Hard stop at 50/day |

**Note:** Cap resets at midnight in the contract's schedule timezone.

---

**Use Case 22: Spend cap — buyer wants to spend max $1,000/day**

| Setting | Value | Why |
|---|---|---|
| Contract > Daily Spend Cap | `$1,000.00` | Stops routing when daily spend hits $1K |

At $20/lead, this effectively caps at 50 leads — but adapts automatically if pricing changes.

---

**Use Case 23: Monthly budget with weekly pacing**

> Buyer has a $10,000/month budget but wants leads spread across the month, not front-loaded.

| Setting | Value | Why |
|---|---|---|
| Contract > Monthly Spend Cap | `$10,000.00` | Total monthly budget |
| Contract > Weekly Spend Cap | `$2,500.00` | ~$10K/4 weeks pacing |
| Contract > Daily Lead Cap | `25` | Smooth daily pacing |

---

**Use Case 24: Campaign-level caps — partner submits max 100/day**

> Limit how many leads a partner can submit per day (prevent flooding).

| Setting | Value | Why |
|---|---|---|
| Campaign > Daily Cap | `100` | Max 100 submissions/day |

---

**Use Case 25: Separate call caps from lead caps**

> A buyer wants 50 leads/day AND 30 calls/day — counted independently.

| Setting | Value | Why |
|---|---|---|
| Contract > Daily Lead Cap | `50` | Lead volume limit |
| Contract > Daily Call Cap | `30` | Call volume limit |
| Contract > Call Cap Mode | `separate` | Counted independently |

---

**Use Case 26: Shared caps — calls count toward lead cap**

> A buyer wants 50 total contacts/day — leads and calls combined.

| Setting | Value | Why |
|---|---|---|
| Contract > Daily Lead Cap | `50` | Total contact limit |
| Contract > Call Cap Mode | `shared` | Calls count toward the 50 |

---

**Use Case 27: Monthly cap with no daily limit**

> Buyer wants 500 leads/month but doesn't care about daily pacing. They'll accept bursts.

| Setting | Value | Why |
|---|---|---|
| Contract > Monthly Lead Cap | `500` | Monthly total |
| Contract > Daily Lead Cap | `0` (unlimited) | No daily restriction |

---

**Use Case 28: Weekend/holiday blackout via schedule**

> Buyer only accepts leads Monday-Friday, 8am-6pm EST.

| Setting | Value | Why |
|---|---|---|
| Contract > Schedule Timezone | `America/New_York` | EST/EDT |
| Contract > Schedule Hours | Mon-Fri: `08:00-18:00` | Business hours only |

Leads arriving outside schedule are passed to the next eligible contract (or rejected if no one is available).

---

**Use Case 29: Time-limited contract — auto-expires after 90 days**

| Setting | Value | Why |
|---|---|---|
| Contract > Expires At | 90 days from now | Auto-pause on expiry |

The contract status changes to `expired` automatically. No more leads routed.

---

**Use Case 30: Ramp-up — start slow, increase over time**

> New buyer starts with 10 leads/day for the first week, then 25, then uncapped.

This is a manual process:
1. Set `Daily Lead Cap: 10` initially
2. After 1 week, update to `25`
3. After 2 weeks, set to `0` (unlimited)

**Tip:** Note the ramp schedule in the contract's internal notes field so you remember to update it.

---

### Filtering & Targeting {#uc-filtering}

**Use Case 31: State-level geo filter — buyer only wants California + Texas**

| Setting | Value | Why |
|---|---|---|
| Contract > Geo Filters > States | `["CA", "TX"]` | Only these states |

---

**Use Case 32: Zip code targeting — local service area**

> A plumber only serves specific zip codes in the Phoenix metro area.

| Setting | Value | Why |
|---|---|---|
| Contract > Geo Filters > Zip Codes | `["85001","85002","85003",...]` | Specific service area |

---

**Use Case 33: State exclusion — everywhere except New York**

> Buyer accepts leads from all states except NY (licensing issue).

| Setting | Value | Why |
|---|---|---|
| Contract > Geo Filters > States | All states except NY | Include list |
| OR: Contract > Geo Filters > Exclude | `true` + States: `["NY"]` | Exclude mode |

---

**Use Case 34: Age filter — Medicare leads (65+)**

> Buyer only wants leads for people 65 and older.

| Setting | Value | Why |
|---|---|---|
| Contract > Demographic Filters > Age | `greater_than: 64` | 65+ only |

---

**Use Case 35: Custom field filter — only homeowners**

> Insurance buyer only wants leads where `homeowner = true`.

| Setting | Value | Why |
|---|---|---|
| Contract > Custom Filters | `{ field: "homeowner", operator: "equals", value: "true" }` | Custom field match |

Requires `homeowner` to be defined as a vertical field.

---

**Use Case 36: Multi-value filter — specific insurance types**

> Buyer only handles auto and home insurance, not life or health.

| Setting | Value | Why |
|---|---|---|
| Contract > Custom Filters | `{ field: "insuranceType", operator: "in", value: ["auto","home"] }` | Match any in list |

---

**Use Case 37: Exclusion filter — no renters**

| Setting | Value | Why |
|---|---|---|
| Contract > Custom Filters | `{ field: "homeowner", operator: "not_equals", value: "true" }` | Exclude specific value |

---

**Use Case 38: Income range filter — affluent leads only**

| Setting | Value | Why |
|---|---|---|
| Contract > Custom Filters | `{ field: "annualIncome", operator: "greater_than", value: "100000" }` | High-income leads |

---

**Use Case 39: Combination filter — CA homeowners 30-65 with auto insurance**

> Complex targeting: California, homeowners, age 30-65, auto insurance type.

| Setting | Value | Why |
|---|---|---|
| Contract > Geo Filters > States | `["CA"]` | California only |
| Contract > Demographic Filters > Age | `30-65` | Age range |
| Contract > Custom Filter 1 | `homeowner equals true` | Homeowners |
| Contract > Custom Filter 2 | `insuranceType equals auto` | Auto insurance |

All filters are AND — the lead must match every condition.

---

**Use Case 40: No filters — buyer accepts everything**

> A high-volume buyer who takes any lead that passes basic validation.

| Setting | Value | Why |
|---|---|---|
| Contract > Geo Filters | Empty | No geo restriction |
| Contract > Demographic Filters | Empty | No demographic restriction |
| Contract > Custom Filters | Empty | No custom restriction |

The lead still goes through system-level validation (required fields, dedup, caps).

---

### Deduplication {#uc-dedup}

**Use Case 41: Buyer-level dedup — no duplicate leads within 30 days**

> A buyer doesn't want to receive the same person twice within a month.

| Setting | Value | Why |
|---|---|---|
| Buyer > Dedup Enabled | `true` | Activate dedup |
| Buyer > Dedup Fields | `["email", "phone"]` | Match on email OR phone |
| Buyer > Dedup Window Days | `30` | 30-day lookback |

---

**Use Case 42: Cross-partner dedup — prevent same person sold by different partners**

> Two partners both submit leads for "John Smith." Without cross-partner dedup, the same person gets sold twice.

| Setting | Value | Why |
|---|---|---|
| System Settings > Cross-Partner Dedup | `true` | Global enforcement |
| System Settings > Cross-Partner Window | `30` days | Lookback period |

---

**Use Case 43: Cross-partner dedup including calls**

> If someone calls in AND submits a web form, count the call as prior activity.

| Setting | Value | Why |
|---|---|---|
| System Settings > Cross-Partner Include Calls | `true` | Calls count for dedup |

---

**Use Case 44: Per-offer dedup override — disable dedup for high-volume offer**

> Global dedup is on, but one specific offer should allow duplicates (e.g., a survey offer where repetition is OK).

| Setting | Value | Why |
|---|---|---|
| Offer > Cross-Partner Dedup | `false` | Override global for this offer |

---

**Use Case 45: Aggressive dedup — 90-day window, email + phone + address**

> Legal vertical where duplicate leads cause compliance issues. Maximum protection.

| Setting | Value | Why |
|---|---|---|
| Buyer > Dedup Enabled | `true` | Active |
| Buyer > Dedup Fields | `["email", "phone"]` | Multi-field match |
| Buyer > Dedup Window Days | `90` | 3-month lookback |
| System > Cross-Partner Dedup | `true` | Cross-partner |
| System > Cross-Partner Window | `90` | Match the buyer window |

---

**Use Case 46: Dedup by email only — phone changes frequently**

> In a vertical where people change phones often but keep the same email (e.g., younger demographics).

| Setting | Value | Why |
|---|---|---|
| Buyer > Dedup Fields | `["email"]` | Email only |

---

**Use Case 47: No dedup — buyer wants all leads**

> High-volume buyer in a commodity vertical. They handle their own dedup internally.

| Setting | Value | Why |
|---|---|---|
| Buyer > Dedup Enabled | `false` | No buyer-level dedup |
| Offer > Cross-Partner Dedup | `false` | No cross-partner for this offer |

---

### Person Matching & People Hub {#uc-people}

**Use Case 48: Phone-first matching for a call-heavy vertical**

> Most leads come via phone calls. Email is secondary.

| Setting | Value | Why |
|---|---|---|
| Tenant > Person Match Priority | `phone` | Phone checked first |

Choose this during initial tenant setup. Cannot be changed later.

---

**Use Case 49: Email-first matching for web-form-heavy vertical**

> Most leads come via web forms where email is always collected.

| Setting | Value | Why |
|---|---|---|
| Tenant > Person Match Priority | `email` | Email checked first (default) |

---

**Use Case 50: Managing a household with shared phone**

> A family of four shares a home phone. Each family member submits leads with their own email but the same phone.

**System behavior (email-first):**
- Lead 1: `dad@email.com` + `555-HOME` → Person A (Dad)
- Lead 2: `mom@email.com` + `555-HOME` → No email match. Phone matches Person A. Different email → **low confidence** → Person B (Mom)
- Lead 3: `kid1@email.com` + `555-HOME` → Same logic → Person C (Kid 1)
- Lead 4: `kid2@email.com` + `555-HOME` → Person D (Kid 2)

**Result:** 4 separate people, all Related via shared phone. Correct behavior — no action needed.

---

**Use Case 51: Repeat customer with new email**

> John used `john@oldco.com` last year, now uses `john@newco.com` with the same phone.

**System behavior:** Phone matches John's record, but different email → low confidence → new person created. Both show as Related.

**Admin action:** Review Related People, confirm it's the same John, merge records. John now has both emails.

---

**Use Case 52: Business phone shared by a sales team**

> Five salespeople at a company all use the main office number `555-MAIN`. Each has their own email.

**System behavior:** Five separate person records, all Related via `555-MAIN`. Correct — they're different people.

---

**Use Case 53: One person, multiple form submissions over months**

> Sarah submits leads on 3 different websites over 6 months, always using `sarah@gmail.com` but sometimes with different phones (cell, work, home).

**System behavior:** Email match every time → high confidence → all leads link to one Person. All 3 phones added as secondary numbers. One clean record.

---

**Use Case 54: Spouse uses partner's email by mistake**

> Linda accidentally enters her husband Robert's email on a form. Robert has a person record with that email.

**System behavior:** Email matches Robert's record → high confidence → Linda's lead links to Robert's person. Linda has no separate record.

**Admin action:** Admin notices two different lead names on one person. Creates a new Person for Linda, reassigns her lead. Optional — depends on data cleanliness requirements.

---

**Use Case 55: De-duplicating after a data migration**

> You imported 10,000 records from a legacy system. Many have overlapping phone numbers.

**Admin action:** Use the "Shared contacts only" filter on People list. Review related people in batches. Merge true duplicates, leave legitimate separate people (families, coworkers) as-is.

---

### Case Splits & Intake {#uc-case-splits}

**Use Case 56: Single intake discovers two cases (standard mother-daughter)**

See [Use Case 1 in Automated Case Splits](#case-splits) for the full walkthrough.

---

**Use Case 57: Intake finds three family members**

See [Use Case 3: Family Plan in Automated Case Splits](#case-splits) — dad, mom, and adult child each get separate leads and person records.

---

**Use Case 58: Intake corrects a name (NOT a case split)**

> Intake realizes the lead's name was misspelled and posts a disposition with corrected spelling but same case ID.

**System behavior:** Same `externalId` → not a new case → normal disposition update. No case split.

---

**Use Case 59: Intake posts same case ID twice (retry)**

> Network error caused the first disposition to fail. Intake retries.

**System behavior:** Same `externalId` + same name → no case split. Idempotency prevents duplicate processing.

---

**Use Case 60: Intake discovers co-plaintiff in legal case**

> A mass tort intake center finds that the lead's neighbor also wants to join the lawsuit.

**Intake action:** Post disposition with new `externalId: "CASE-B"` + `firstName: "Neighbor Name"`.

**System behavior:** Case split fires → new lead + person created → new sale for same buyer. Both people appear as Related since they share the original lead's contact info (which the admin can update with the neighbor's actual info later).

---

### Delivery & Integration {#uc-delivery}

**Use Case 61: HTTP POST delivery — standard JSON webhook**

> Buyer has an API endpoint that accepts JSON lead data.

| Setting | Value | Why |
|---|---|---|
| Contract > Delivery Config > Type | `http_post` | POST request |
| Contract > Delivery Config > URL | Buyer's endpoint URL | Where to send |
| Contract > Delivery Config > Format | `json` | JSON body |
| Contract > Delivery Config > Timeout | `30` seconds | Wait for response |
| Contract > Delivery Config > Retries | `3` | Retry on failure |

---

**Use Case 62: Delivery with authentication — Bearer token**

| Setting | Value | Why |
|---|---|---|
| Contract > Delivery Config > Auth Type | `bearer` | Token auth |
| Contract > Delivery Config > Auth Credentials | `{ token: "buyer-api-key" }` | Buyer's API key |

---

**Use Case 63: Delivery with custom field mapping**

> Buyer's CRM expects `contact_email` instead of `email`, and `contact_phone` instead of `phone`.

| Setting | Value | Why |
|---|---|---|
| Contract > Delivery Config > Field Mapping | `[{ from: "email", to: "contact_email" }, { from: "phone", to: "contact_phone" }]` | Remap field names |

---

**Use Case 64: Email delivery — no API, buyer wants leads by email**

> Small buyer without a CRM. Wants leads emailed to their sales inbox.

| Setting | Value | Why |
|---|---|---|
| Contract > Delivery Config > Type | `email` | Email delivery |
| Contract > Delivery Config > Email To | `sales@buyerco.com` | Recipient |
| Contract > Delivery Config > Email Subject | `New Lead: {{firstName}} {{lastName}}` | Dynamic subject |

---

**Use Case 65: Delivery to intake center CRM**

> Buyer uses a third-party intake center (like a legal intake platform).

| Setting | Value | Why |
|---|---|---|
| Contract > Intake Center ID | Select from list | Pre-configured CRM |
| Contract > Intake Account ID | Buyer's account number | CRM account |
| Contract > Intake API Key | Encrypted key | CRM authentication |

---

**Use Case 66: Response parsing — capture buyer's lead ID**

> After delivery, the buyer's API returns their internal ID. Capture it for disposition tracking.

| Setting | Value | Why |
|---|---|---|
| Contract > Delivery Config > Response Mapping | `[{ source: "response.data.id", target: "buyerLeadId" }]` | Map response field to buyerLeadId |

---

**Use Case 67: Multi-delivery — HTTP + email notification**

> Post to buyer's API AND send an email notification to their sales team.

| Setting | Value | Why |
|---|---|---|
| Contract > Delivery Config | Array with 2 actions: HTTP POST + Email | Multiple delivery targets |

---

### Call Routing {#uc-call-routing}

**Use Case 68: Basic IVR — press 1 for sales, 2 for support**

| Setting | Value | Why |
|---|---|---|
| IVR Config > Greeting | "Press 1 for sales, 2 for support" | Menu prompt |
| IVR Config > Menu Option 1 | Digit: `1`, Action: `route` | Route to sales offer |
| IVR Config > Menu Option 2 | Digit: `2`, Action: `transfer`, Target: support number | Transfer out |
| IVR Config > Timeout | `10` seconds | Wait for input |
| IVR Config > Max Retries | `2` | Replay menu twice |

---

**Use Case 69: Call recording for compliance**

> Legal vertical requires all calls to be recorded for compliance purposes.

| Setting | Value | Why |
|---|---|---|
| Tracking Number > Record Calls | `true` | Record all calls |

---

**Use Case 70: Whisper message — announce lead info to buyer**

> Before connecting the caller, play a whisper to the buyer: "Incoming auto insurance lead from California."

| Setting | Value | Why |
|---|---|---|
| Contract > Call Whisper Message | Dynamic template | Info for the buyer |
| Contract > Call Whisper Required | `true` | Must play before connecting |

---

**Use Case 71: Short ring timeout for fast fallthrough**

> Try Buyer A for 10 seconds, then Buyer B for 10 seconds, then Buyer C.

| Setting | Value | Why |
|---|---|---|
| Offer > Call Distribution Rule | `priority` | Try in order |
| Contract A > Call Ring Timeout | `10` seconds | Short timeout |
| Contract B > Call Ring Timeout | `10` seconds | Quick fallthrough |
| Contract C > Call Ring Timeout | `30` seconds | Last buyer, longer ring |

---

**Use Case 72: Toll-free tracking number for TV ads**

| Setting | Value | Why |
|---|---|---|
| Tracking Number > Number Type | `tollfree` | 800/888 number |
| Tracking Number > Provider | `twilio` or `telnyx` | VoIP provider |
| Tracking Number > Campaign ID | Link to campaign | Attribution |

---

**Use Case 73: Call payout — duration-based billing**

> Partner gets paid $20 for connected calls.

| Setting | Value | Why |
|---|---|---|
| Campaign > Call Billing Rules > Payout Type | `fixed` | Fixed payout when conversion criteria is met |
| Campaign > Call Billing Rules > Payout Amount | `$20.00` | Partner payout amount |
| Campaign > Call Billing Rules > Conversion Event | `Call Length` | Duration-based conversion |
| Campaign > Call Billing Rules > Start Call Length On | `connect` | Connected talk time |

---

**Use Case 74: Minimum call duration for billing**

> Buyer only pays for calls lasting 90+ seconds (filters out tire-kickers).

| Setting | Value | Why |
|---|---|---|
| Contract > Call Billing Rules > Conversion Event | `Call Length` | Requires a minimum connected duration |
| Contract > Call Billing Rules > Length | `90` seconds | Minimum qualified duration |
| Contract > Call Billing Rules > Payout Amount | `$15.00` | Price per qualified call |

---

### Partner & Campaign Management {#uc-partner}

**Use Case 75: New partner onboarding — single campaign**

> A new partner will submit leads for one offer via API.

Setup:
1. Create Partner (status: `active`)
2. Create Campaign linked to Partner + Offer (posting key auto-provisioned)
3. Share the posting key and public spec link with partner
4. Partner uses `POST /api/v1/leads/submit` with `Authorization: Bearer pk_xxx` and `campaignId` in the body

---

**Use Case 76: Partner with multiple campaigns across verticals**

> A large publisher generates leads across auto insurance, home insurance, and Medicare.

Setup:
1. One Partner record
2. Three Campaigns — one per vertical/offer
3. Partner uses one posting key with `campaignId` in each request to target the right campaign
4. Partner sends `campaignId` based on lead type

---

**Use Case 77: Progressive lead capture (multi-step forms)**

> Partner's website has a 3-step form. Capture partial data early (in case the user drops off), then complete on final submit.

| Setting | Value | Why |
|---|---|---|
| Campaign > Type | Standard | Uses session API |

**Partner integration:**
1. `POST /api/v1/sessions` → create session
2. `PATCH /api/v1/sessions/:id` → add fields at each step
3. `POST /api/v1/sessions/:id/submit` → final submission triggers routing

The session captures data progressively. If the user abandons at step 2, you have partial data.

---

**Use Case 78: Co-registration — attach to partner's existing form**

> Partner already has a form (e.g., sweepstakes entry). Add a checkbox: "Also get insurance quotes?" If checked, submit to your system.

**Partner integration:**
1. Partner form has opt-in checkbox
2. On form submit (if checked): `POST /api/v1/leads/submit` with `Authorization: Bearer pk_xxx` and the user's data
3. Lead routes normally

See [Co-Registration](#coreg) section for JavaScript snippet.

---

**Use Case 79: Partner volume cap — prevent flooding**

> A new partner tends to submit bursts of low-quality leads. Cap them at 50/day until quality improves.

| Setting | Value | Why |
|---|---|---|
| Campaign > Daily Cap | `50` | Limit submissions |

Increase the cap as quality metrics improve.

---

**Use Case 80: Auto-disable campaign when offer pauses**

> If an offer is paused (no buyers available), automatically disable the partner's campaign so they stop submitting.

This is built-in behavior. When an offer is paused, linked campaigns are auto-disabled. The `autoDisabledByOfferId` field tracks which offer caused the pause. When the offer reactivates, campaigns restore to their previous status.

---

**Use Case 81: Partner portal — self-service**

> Partners log in to view their leads, earnings, and posting specs without contacting you.

| Setting | Value | Why |
|---|---|---|
| Portal Settings > Partner Portal Access | `true` | Enable portal |
| Portal Settings > Sign Up Enabled | `true` (optional) | Allow self-registration |
| Portal Settings > Sign Up Approval | `manual` | Admin approves new partners |

---

**Use Case 82: Partner portal keys — programmatic access**

> Partner wants to pull lead status and earnings via API instead of checking the portal.

Setup:
1. Partner creates a portal API key from Settings (or admin creates from Partner detail > API Keys)
2. Key is scoped to `routing` (portal access only — not for lead submission)
3. Partner uses `Authorization: Bearer lr_xxx` for `/api/partner/*` endpoints
4. For lead submission, use the separate posting key shown on campaign Posting Specs

---

### Compliance & Blocking {#uc-compliance}

**Use Case 83: Block a known fraud IP range**

| Setting | Value | Why |
|---|---|---|
| Blocked IP Ranges > From/To | IP range | Block submissions from this range |
| Blocked IP Ranges > Reason | `fraud` | Category |

---

**Use Case 84: Block a spam referrer domain**

| Setting | Value | Why |
|---|---|---|
| Blocked Referrers > Value | `spamsite.com` | Block leads from this referrer |

---

**Use Case 85: Block a robocaller ID**

| Setting | Value | Why |
|---|---|---|
| Blocked Caller IDs > Value | `555-0000` | Known robocall number |

---

**Use Case 86: Suppression list — honor DNC requests**

> A person requests to never be contacted again.

Add their phone/email to the suppression list:
- **Type:** `phone` or `email`
- **Scope:** `global` (all buyers) or `buyer` (specific buyer)
- **Reason:** `dnc_request`

Suppressed contacts are rejected before routing — they never reach a buyer.

---

**Use Case 87: TCPA consent tracking**

> Every lead must have documented TCPA consent before routing to buyers.

| Setting | Value | Why |
|---|---|---|
| System Settings > Consent Enabled | `true` | Enable consent tracking |

Lead submission must include `tcpaConsent: true`, `consentTimestamp`, and `consentIp`. Leads without consent can be rejected or flagged.

---

### Dispositions & Returns {#uc-dispositions}

**Use Case 88: Auto-return on "Bad Data" disposition**

> If intake marks a lead as "Bad Data," automatically return it to the routing pool for re-sale.

| Setting | Value | Why |
|---|---|---|
| Vertical Group Disposition > "Bad Data" > Category | `returned` | Return category |
| Vertical Group Disposition > "Bad Data" > Triggers Return | `true` | Auto-return |
| Vertical Group Disposition > "Bad Data" > Return Reason | `bad_data` | Reason code |

---

**Use Case 89: CPA conversion disposition**

> When intake marks a lead as "Signed Retainer," it should trigger CPA revenue.

| Setting | Value | Why |
|---|---|---|
| Vertical Group Disposition > "Signed Retainer" > Category | `converted` | Conversion category |
| Vertical Group Disposition > "Signed Retainer" > Triggers CPA | `true` | Fires CPA event |

---

**Use Case 90: Working disposition — no financial impact**

> Intake marks a lead as "In Review" — just a status update, no revenue or return.

| Setting | Value | Why |
|---|---|---|
| Vertical Group Disposition > "In Review" > Category | `working` | Working status |
| Vertical Group Disposition > Triggers CPA | `false` | No revenue |
| Vertical Group Disposition > Triggers Return | `false` | No return |

---

**Use Case 91: Rejection disposition — lead didn't qualify**

> Intake determines the lead doesn't qualify. Mark as rejected, but DON'T return to pool.

| Setting | Value | Why |
|---|---|---|
| Vertical Group Disposition > "Not Qualified" > Category | `rejected` | Rejected |
| Vertical Group Disposition > Triggers Return | `false` | Don't re-sell |

---

**Use Case 92: Contract-specific disposition overrides**

> A specific buyer has custom disposition labels that differ from the vertical defaults.

Create contract-level dispositions that override the vertical group defaults. The buyer's intake uses contract disposition slugs in their postbacks.

---

### System Configuration {#uc-system}

**Use Case 93: Enable call tracking for your account**

| Setting | Value | Why |
|---|---|---|
| System Settings > Features > Calls Enabled | `true` | Unlock call features |

This enables: tracking numbers, IVR config, call routing, call reports, call caps on contracts.

---

**Use Case 94: Enable messaging (email/SMS campaigns)**

| Setting | Value | Why |
|---|---|---|
| System Settings > Features > Messaging Enabled | `true` | Unlock messaging features |

---

**Use Case 95: Enable consent tracking**

| Setting | Value | Why |
|---|---|---|
| System Settings > Features > Consent Enabled | `true` | Unlock consent features |

---

**Use Case 96: Configure SMTP for email delivery**

> You want to send lead delivery emails from your own domain.

| Setting | Value | Why |
|---|---|---|
| Settings > SMTP > Host | Your SMTP server | Email server |
| Settings > SMTP > Port | `587` | TLS port |
| Settings > SMTP > Username | SMTP user | Authentication |
| Settings > SMTP > Email Address | `leads@yourco.com` | From address |
| Settings > SMTP > SSL Enabled | `true` | Encryption |

---

**Use Case 97: Custom domain for partner portal**

> Partners access the portal at `partners.yourcompany.com` instead of the default URL.

| Setting | Value | Why |
|---|---|---|
| Settings > Domains > Domain | `partners.yourcompany.com` | Custom domain |
| Settings > Domains > SSL Enabled | `true` | HTTPS |

Point DNS to Vercel, then add the custom domain in settings.

---

**Use Case 98: Global redirect for unmatched leads**

> Leads that don't match any contract should redirect to a fallback page.

| Setting | Value | Why |
|---|---|---|
| System Settings > Redirect Enabled | `true` | Enable redirect |
| System Settings > Global Redirect URL | `https://yoursite.com/sorry` | Fallback page |

---

**Use Case 99: Buyer portal with self-service sign-up**

> Buyers can register for an account and browse available offers.

| Setting | Value | Why |
|---|---|---|
| Portal Settings > Buyer Portal Access | `true` | Enable portal |
| Portal Settings > Sign Up Enabled | `true` | Self-registration |
| Portal Settings > Sign Up Approval | `manual` | Admin reviews before approval |
| Portal Settings > Require Terms | `true` | Must accept T&C |

---

**Use Case 100: Multi-vertical setup — one account, three verticals**

> You run lead gen for auto insurance, home insurance, and Medicare. Each has different fields, buyers, and disposition flows.

Setup:
1. Create 3 **Verticals** — each with their own custom fields
2. Create a **Vertical Group** per vertical — with dispositions matching that industry
3. Create **Offers** per vertical — with distribution rules tuned to each market
4. Create **Contracts** per buyer per vertical — filters, caps, and pricing specific to each
5. Create **Campaigns** per partner per vertical — partner uses `campaignId` in each request

Each vertical operates independently: separate fields, filters, dispositions, caps, and reporting. But the People Hub unifies person records across verticals — if John has an auto insurance lead AND a home insurance lead, both appear on his person record.

---

### Choosing the Right Configuration — Quick Reference {#uc-quick-ref}

**"I have one buyer per lead"** → Use Case 1 (exclusive + price)

**"I sell to multiple buyers"** → Use Case 2 (multisell) or Use Case 6 (hybrid)

**"I want equal distribution"** → Use Case 3 (round-robin) or Use Case 4 (weighted)

**"Buyer has a budget"** → Use Case 22 (spend cap) + Use Case 23 (pacing)

**"Buyer only wants certain states"** → Use Case 31 (geo filter)

**"I need to prevent duplicates"** → Use Case 41 (buyer dedup) + Use Case 42 (cross-partner)

**"Leads come mostly by phone"** → Use Case 48 (phone-first matching)

**"Buyer doesn't have an API"** → Use Case 64 (email delivery)

**"Partner sends too many bad leads"** → Use Case 79 (volume cap)

**"I need call routing"** → Use Case 93 (enable calls) + Use Case 7 or 8 (routing strategy)

**"Intake finds multiple cases"** → Use Case 56-60 (case splits)

**"I have multiple verticals"** → Use Case 100 (multi-vertical setup)

---

## AI-Powered Features {#ai-powered-features}

### Anomaly Detection & Alerts

<!-- react-flow:anomaly-detection -->

The platform continuously monitors your lead routing operations for anomalies. Every 15 minutes, the system compares current-hour metrics against your 7-day baseline and flags deviations.

**Monitored signals:**
- **Acceptance rate drop** — Fewer leads being sold than expected
- **Delivery failure rate** — Buyer endpoints failing more than usual
- **Volume anomaly** — Unusual lead volume (too high or too low)
- **Margin erosion** — Revenue minus cost dropping below baseline
- **Invalid field spike** — More leads arriving with missing email or phone

**Severity levels:**
- **Warning** — Metric deviated from baseline beyond threshold
- **Critical** — Metric deviated severely (2x the warning threshold) — may trigger auto-pause

**Alert lifecycle:** Open → Acknowledged → Resolved (or Open → Dismissed)

Navigate to **AI > Alerts** to view and manage alerts.

### Lead Scoring

Every lead receives a conversion score (0-100) at submission based on six factors:
- **Field completeness** (0-20) — How many contact fields are filled
- **Source quality** (0-15) — Campaign's historical acceptance rate
- **Geo demand** (0-15) — Number of active buyers in the lead's state
- **Recency** (0-15) — How fresh the lead is (decays over 24 hours)
- **Dedup signal** (0-15) — Whether this person has been seen before
- **Demographic match** (0-20) — How many buyer contracts this lead qualifies for

Scoring is non-blocking — it never slows down lead submission.

### Campaign Optimization

Navigate to **AI > Optimization** for AI-generated suggestions to improve performance. The system analyzes:
- Revenue per contract (flags underperformers)
- Cap utilization (flags under/over-utilized caps)
- Distribution rule simulation (projects revenue under alternative rules)

Click **Apply** on any suggestion to make the change. All changes are reversible.

### Revenue Forecast

Navigate to **AI > Forecast** to see projected revenue for the next 7, 14, or 30 days. The forecast uses a weighted moving average with day-of-week seasonality patterns from your historical data.

Filter by offer, buyer, or partner to see entity-specific projections.

### Smart Deduplication

Beyond exact-match dedup, the system now detects:
- **Normalized email matches** — Strips Gmail dots and +tags (j.doe+test@gmail.com = jdoe@gmail.com)
- **Normalized phone matches** — Strips formatting and country code
- **Fuzzy name matches** — Catches typos and variations (Jon vs John)
- **Household detection** — Same last name + zip code = same household

### Self-Healing Delivery

When buyer delivery endpoints fail, the system:
1. Classifies the failure (timeout, server error, connection refused, etc.)
2. Alerts you via the anomaly detection system
3. Auto-pauses contracts after repeated critical failures
4. Suggests remediation actions in the alert details

### AI Dialer {#ai-dialer}

Powered by ElevenLabs. After a lead is delivered, the AI Dialer calls the lead, qualifies them, and bridges a connected call directly to the buyer contract's Transfer Number. AI Dialer lives under the **Nurture** group in the sidebar (alongside Messaging). Once enabled, opening it lands on the Call Log, where a horizontal tab bar (Call Log, Agents, Cadences) navigates between its pages; each tab is a real page link. While AI Dialer is not yet enabled the tab bar is hidden and you see the enable landing instead.

**Enabling AI Dialer.** AI Dialer is a [Platform Service](#platform-services) — it runs on our voice credentials and bills per minute in credits. Turn it on yourself from the locked **AI Dialer** item under **Nurture** in the sidebar (opens a landing page with the credit rate and an **Enable** button) or from **Billing → Plans & Services**. A payment method is required: enabling with no card on file returns a billing prompt to add one first. No ElevenLabs key or Integrations setup is needed on your side — the platform key is managed centrally by the operator. Disabling AI Dialer pauses its outbound dialing; re-enable it anytime.

**Cadence types** (Cadences → *cadence* → Cadence Type):
- **Immediate** — dial right after the sale completes (legacy behavior). The contract's "Delay Before First Call" controls how soon.
- **Call Assist** — wait N days post-sale (configurable per cadence via "Trigger After Days") and only dial leads still in *working* disposition (not converted, rejected, or returned). Useful for follow-up on stalled leads. A dedicated cron checks every 5 minutes; eligible leads queue and dial within the cadence and TCPA windows. Idempotent — at most one Call Assist contact per (sale, cadence) pair.

**Default cadences (seeded per tenant).** Every tenant starts with 3 ready-to-use cadences, seeded automatically when the account is created (and back-filled once for older accounts). They're tenant-scoped, editable copies — change or delete them freely; a tenant that already has any cadence is never re-seeded. Attempt caps are research-backed: ~93% of leads that convert are reached by the 6th attempt, and attempts past that convert 45% less often, so the defaults cap at 6 (3 for revival). All three call weekdays only (Mon–Fri) and stay inside federal TCPA calling hours (8am–9pm in the lead's local time).

- **Speed to Lead (High-Intent)** — immediate, 6 attempts. The first attempt is a **double tap**: the dialer places the call and, if there's no pickup, redials about 75 seconds later. Crucially, the first dial leaves **no voicemail** — a completed voicemail ends the call, which is exactly what cancels the iOS "repeated calls from the same number break through Do Not Disturb / Focus" behavior. Leaving no voicemail on the first dial keeps that breakthrough alive for the immediate redial. After the double tap comes a same-day 45-minute follow-up (still 3 dials on day one, which keeps you inside stricter state caps like Florida's ~3/24h), then next-day morning, day 5, day 14, and day 15.
- **Standard Follow-Up** — immediate, 6 attempts, single dials each: right away, 45 minutes, 2 hours, day 5, day 14, day 15. No double tap.
- **Aged Lead Revival** — call-assist, fires 7 days after the lead is created, 3 attempts: midday day 0, day 3, and day 7 (voicemail left on the final attempt).

**Agent voicemail configuration** (AI Dialer → Agents → *agent* → Conversation → Voicemail). Each agent decides how it behaves when it reaches an answering machine, via **Voicemail Mode**:
- **Leave message** — always read the **Voicemail Message** script, then end the call.
- **Never leave message** — end the call silently on machine detection; the Voicemail Message field is disabled because it is never used.
- **Cadence controlled** (default) — honor the per-call `{{leave_voicemail}}` variable. Double-tap cadences set `leave_voicemail=false` on the first dial so the immediate ~75-second redial can break through the iOS repeated-call Do Not Disturb behavior (a completed voicemail would cancel it); later attempts leave the message normally.

The mode and the Voicemail Message script are baked into a **Voicemail Handling** section of the agent's generated system prompt. Editing them takes effect after you **Save** (stores the config) and **Sync to ElevenLabs** (regenerates the prompt from the vertical + voicemail config and pushes it to the remote ElevenLabs agent). Because ElevenLabs decides voicemail behavior agent-side — there is no dial-time voicemail flag on the outbound-call API — this prompt-level branching is how per-call suppression is honored.

**Provisioning** (the ElevenLabs agent behind the record). Every agent row needs a matching agent on ElevenLabs before it can take calls. The agents list and the agent form show a **Provisioned** / **Not provisioned** badge — provisioned means the row has an `elevenLabsAgentId`.
- **Creating an agent** provisions it automatically when ElevenLabs is configured. If provisioning fails at create time (e.g. ElevenLabs is briefly unreachable) the record is still saved and you get a warning to retry via Sync.
- **Sync provisions unprovisioned agents.** Clicking **Sync to ElevenLabs** on a *Not provisioned* agent creates the ElevenLabs agent and stores its id (in addition to regenerating and pushing the prompt). On an already-provisioned agent, Sync just pushes the regenerated prompt. If ElevenLabs rejects the call the prompt is still saved locally, and Sync returns an error so you can retry.
- **The phone number ID is manual.** Outbound calls need *both* an `elevenLabsAgentId` (provisioning) and an **ElevenLabs Phone Number ID** (pasted into Voice & Transfer). The form shows an amber hint next to that field when it is empty, and Sync warns you when the agent is provisioned but still has no phone number ID.
- **Unprovisioned agents are skipped by the dialer** — the outbound processor logs a skip and moves on rather than dialing an agent that has no ElevenLabs id.

<!-- react-flow:call-assist-flow -->

**Per-contract config** (Contracts → *contract* → **Calls** tab → Phone Numbers) is the new primary location for AI Dialer config:
- **Transfer Number** — the phone number AI bridges the connected call to. This is the contract's `callPhoneNumber` and is also used for normal inbound call routing.
- **Enable AI Dialer** — opt this contract in. Disabled (greyed) until a valid Transfer Number is entered.
- **AI Dialer Agent** — the ElevenLabs agent. Its outbound tracking number is the caller ID.
- **AI Dialer Cadence** — pick an Immediate or Call Assist cadence.
- **AI Dial Delay (seconds)** — only shown when an Immediate cadence is selected; ignored for Call Assist (which fires from the cron, not at sale time).

**Campaign requirement:** the lead's source campaign must have a tracking number assigned. If it doesn't, AI Dialer is skipped entirely and the reason is logged. The campaign's tracking number is stamped on the AI Dialer contact record for reporting attribution.

**Eligibility states on the contract:**
- **Ping/post contracts** — not eligible
- **Fixed-price contracts with no Transfer Number** — not eligible (set Transfer Number first)
- **All other fixed-price contracts** — Enabled/Disabled based on the per-contract toggle

**Dial flow:**
1. Lead is delivered to contract.
2. If the lead's source campaign has a tracking number AND the winning contract has `aiDialerEnabled = true` AND a valid `callPhoneNumber` → AI dial is scheduled.
3. For multisell offers, only the first (highest-priority) successful sale triggers AI Dialer; other contracts are delivered normally with no AI call.
4. On connect, AI bridges the call directly to the contract's `callPhoneNumber`.
5. Call attempts, outcomes, and transfers are logged under Call Tracking. AI Dialer contacts are attributed to `campaignTrackingNumberId` for reporting.

**Identifying AI transfers in the call log:** when the AI agent qualifies a lead and transfers the live leg, the resulting inbound call gets `call.aiDialerCallId` stamped on it, linking back to the originating AI conversation. In **Calls** (admin/calls), transferred inbounds show a purple **AI Transfer** badge next to the caller number so you can distinguish them from organic inbounds at a glance.

Defaults: `aiDialerEnabled` is OFF on all contracts. You must explicitly opt in per contract — no mass-enable when the offer's AI Dialer is turned on.

---

## AI Agent Skills

iSCALE provides AI agent skill endpoints that return personalized markdown prompts. These help third-party AI agents (like BuiltWithAI) set up and integrate correctly — preventing common mistakes like duplicating system fields or using wrong field names.

### Setup Skill (Admin)

**Endpoint:** `GET /api/v1/skills/setup-vertical-campaign`

Guides an AI agent through creating a vertical, campaigns, partners, buyers, offers, and contracts. The prompt includes:
- **System fields blocklist** — fields that must NOT be recreated as vertical fields (firstName, lastName, email, phone, etc.)
- **Existing entities** — verticals, partners, and buyers already in your account
- **Step-by-step workflow** — all API calls needed to set up a complete routing pipeline

**Usage:** Set `ISCALE_API_URL` and `ISCALE_API_KEY` (admin key) in `.env.local`, then fetch the skill endpoint. The AI agent follows the returned instructions.

### Partner Integration Skill

**Endpoint:** `GET /api/v1/skills/partner-integration?campaignId={id}`

Guides a partner's AI agent through integrating with a specific campaign. The prompt includes:
- **Field mapping** — partner-facing field names from the posting spec (NOT camelCase internals)
- **Sample payloads** — ready-to-use test body and curl command
- **Session flow** — create, patch, submit, and coreg endpoints for progressive capture
- **Integration checklist** — verification steps to confirm everything works

**Usage:** Set `ISCALE_API_URL`, `ISCALE_API_KEY`, and `ISCALE_CAMPAIGN_ID` in `.env.local`, then fetch the skill endpoint with `?campaignId=`.

### Static Templates

Reference templates are also available without API access:
- `skills/setup-vertical-campaign.md` — generic setup guide
- `skills/partner-integration.md` — generic integration guide

These are useful for reading offline but don't include personalized data (existing entities, campaign-specific fields).

## AI Assistant {#ai-assistant}

The AI Assistant is a built-in chat panel that helps you manage leads, contracts, offers, and other entities without leaving the page. Open it by clicking the assistant icon in the bottom-right corner of any admin page.

You can type natural-language requests like "show me today's leads," "pause contract #42," or "what buyers are active in California." The assistant understands your system's data and can navigate, explain, and guide you through tasks.

### Voice Input {#voice-input}

Click the **microphone button** in the chat input area to speak your request instead of typing. Your speech is converted to text and sent automatically when you stop talking.

- **Browser support:** Voice input requires the Web Speech Recognition API. Chrome and Edge have the best support. Firefox and Safari may have limited or no support.
- **Permissions:** Your browser will ask for microphone access the first time you use it. Grant permission to enable voice input.
- **Tip:** Speak naturally in full sentences. The assistant handles conversational phrasing well — "show me all leads from this week" works just as well as clicking through filters manually.

### Voice Output {#voice-output}

Toggle the **speaker icon** in the assistant panel header to have responses read aloud using your browser's text-to-speech engine.

- Your preference (on/off) is saved in your browser and persists across sessions.
- Voice output uses your system's default voice and language settings.
- Long responses are read in full — you can toggle the speaker off mid-response to stop playback.

### Interactive Walkthroughs {#interactive-walkthroughs}

The assistant can guide you through multi-step workflows by highlighting elements on the page and explaining what to do at each step. To start a walkthrough, ask something like:

- "Walk me through adding a buyer"
- "Show me how offers work"
- "Guide me through creating a campaign"

The assistant walks you through each step sequentially, highlighting the relevant UI element and explaining its purpose before moving to the next one. You control the pace — each step waits for you to continue.

### Action Cards {#action-cards}

When the assistant suggests actions, they appear as colored cards in the chat:

| Card Color | Action Type | Example |
|---|---|---|
| **Blue** | Navigation | "Go to Buyers page" — click to navigate |
| **Amber** | Highlight | "See the Status column" — click to spotlight an element |
| **Purple** | Walkthrough | "Tour the Contract form" — click to start a guided walkthrough |

Click any action card to execute it. Navigation cards take you to the relevant page. Highlight and walkthrough cards interact with elements on the current page.

### Spotlight Highlights {#spotlight-highlights}

When the assistant highlights a UI element (via an action card or during a walkthrough), a **spotlight overlay** dims the rest of the page and draws attention to the target element with a bright border.

- **Dismiss:** Click the **"Got it"** button on the spotlight overlay, or press **Escape**.
- The spotlight scrolls the target element into view if it's off-screen.
- If the target element can't be found on the current page, the assistant lets you know.

### Web Fetch {#ai-web-fetch}

The AI assistant can fetch content from approved external URLs. This is useful for:

- **Posting specs** — Share a URL to an external posting specification and ask the assistant to map it to a campaign setup
- **Documentation** — Ask the assistant to look up documentation for the platform's tech stack (Next.js, Drizzle, Tailwind, etc.)
- **API references** — Share API documentation URLs for the assistant to reference

**How it works:**
- Only HTTPS URLs from whitelisted domains are allowed
- System administrators manage the global domain whitelist (Settings > AI Configuration)
- Tenant administrators can add up to 50 additional domains for their organization
- HTML content is automatically converted to plain text
- Large pages are truncated to keep responses focused

**Default allowed domains:**
- nextjs.org, tailwindcss.com, neon.tech, neon.com, zod.dev
- orm.drizzle.team, drizzle-team.github.io
- docs.anthropic.com, platform.claude.com
- sdk.vercel.ai, ai-sdk.dev

**Security:** The agent cannot access internal networks, private IPs, or non-whitelisted domains. All fetched content is read-only — the agent cannot submit forms or modify external systems.

### Agent Sessions {#agent-sessions}

The Agent Sessions page (**Admin > Agent > Sessions**) lets you review and analyze all AI assistant conversations.

**Sessions List**
- View all agent chat sessions with date, user, lane (direct/workflow/planner), workflow name, message count, tool calls, duration, and cost
- Filter by user, lane, workflow, date range, or search message content
- Click any row to view the full conversation

**Session Detail**
- **Conversation tab** — full replay of the conversation showing user messages, assistant responses, and tool calls with expandable input/output JSON
- **Metadata tab** — session info, token breakdown (orchestrator, router, main model, workflow), classification details, and selected tools

**Usage & billing.** Each chat turn records its input/output token counts and actual AI cost on the session (visible in the Token Breakdown) and rolls up into daily usage stats. Tenant sessions are billed **per turn from credits, based on actual usage** — exactly like the other AI services (see [AI usage billing](#credits-billing)). These charges appear as the **AI Agent** line on the superadmin AI Costs tab and in the tenant's credit ledger. Superadmin sessions are tracked but never billed, and a failed deduction (e.g. zero balance) never blocks the chat or loses the transcript.

**Analysis Dashboard**
- Switch to the Analysis tab on the sessions list page
- View daily session trends, lane distribution, top workflows by usage, top tools by call frequency
- Monitor average session cost and error rates

## Portal Signup {#portal-signup}

Partners and buyers can self-register via a public signup form instead of being manually created by an admin.

### Signup URLs
- **Partner signup:** `/partner/signup`
- **Buyer signup:** `/buyer/signup`

A "Sign up" link also appears on the login page when signup is enabled.

### How It Works
1. User fills a multi-step form: company info, optional custom questions, password
2. System creates the partner/buyer entity and user account
3. Outcome depends on the approval mode configured in **Settings > Partner Settings** or **Settings > Buyer Settings**

### Approval Modes
| Mode | Behavior |
|------|----------|
| **Auto-approve** | Account created and immediately active. User redirected to their portal. |
| **Manual-approve** | Account created in `pending` status. User sees a "pending approval" screen. Admin reviews in the approval queue. |

### Admin Approval Queue
When manual-approve is enabled, pending signups appear at **System > Pending Signups** (API: `GET /api/admin/pending-signups`). Admins can:
- **Approve** — activates the account; user can log in
- **Reject** — marks the signup as rejected

Slack notifications are sent when a signup requires manual review (if Slack integration is configured).

### Custom Questions
Admins can configure additional questions that appear during signup (e.g., "What verticals are you interested in?"). Answers are stored with the signup and visible in the approval queue.

### Enabling Signup
Toggle self-service signup on/off per entity type in **Settings > Partner Settings** and **Settings > Buyer Settings**. Configure approval mode and optional terms & conditions in the same section.

### TCPA Consent Capture {#tcpa-consent}
All three self-serve signup flows — **Get Started** (`/get-started`, tenant), **Partner Signup** (`/partner/signup`), and **Buyer Signup** (`/buyer/signup`) — require the user to check a **Communications Consent** box below the Phone field. The checkbox is required; the form will not submit without it. The exact disclosure text is a fixed v1 string defined in `src/shared/constants/tcpaConsent.ts` and is rendered verbatim on-screen so the user can read what they are agreeing to.

On successful signup, a `signupTcpaConsent` JSONB snapshot is written to the new record:

| Field | Written to |
|---|---|
| `acceptedAt` | ISO timestamp at submission |
| `ipAddress` | First hop from `x-forwarded-for` / `x-real-ip`, or `null` if unresolvable |
| `userAgent` | Raw browser UA string, or `null` if absent |
| `textVersion` | `"v1"` (bumped when disclosure text changes) |
| `textShown` | The full disclosure text the user saw |

The column lives on `tenant.signupTcpaConsent` (tenant flow), `partner.signupTcpaConsent` (partner portal flow), and `buyer.signupTcpaConsent` (buyer portal flow). Admin-created records (partners/buyers added via `/admin/...`) are not populated — only self-serve signups capture this snapshot.

---

## Deliver-First Pipeline {#deliver-first-pipeline}

The routing engine uses a **deliver-first-charge-after** model: buyers are only charged after the lead is successfully delivered to their endpoint.

**How it works:**
1. Routing engine selects winning buyer(s) based on filters, caps, balance, etc.
2. Lead is delivered to the buyer's HTTP endpoint
3. Response mapping rules evaluate the buyer's response
4. **Only on confirmed success** is the buyer charged and partner credited
5. On delivery failure, the sale is marked failed with no charge

**Exclusive waterfall:** For exclusive offers, if the first buyer's delivery fails, the system automatically tries the next eligible buyer in priority order until one succeeds or all fail.

---

## Products {#products}

**Location:** [/admin/products](/admin/products) — visible when the SKU / Product feature is enabled for your tenant (see [SKU / Product System](#skus-tenant-feature)).

A **product** is the thing a buyer is really buying — "Auto Insurance Premium". [SKUs](#billing-and-lead-skus) are the purchasable bundles of it (100 leads, 500 leads, a monthly subscription). Grouping SKUs under one product is what lets repeat purchases consolidate into a single contract instead of spawning a new one each time.

A product is either a **lead** product or a **call** product; the type is chosen at the top of the form and decides which fields appear (call products add the billable event and call terms; only lead products expose default contract options).

### Product Basics {#product-basics}

| Field | Notes |
|---|---|
| Name | Display name. Used in the admin UI, sent to Stripe as the Stripe Product name, and read by the public checkout page to label what is being bought |
| Slug | URL-safe identifier — lowercase and dashes, unique per tenant. Admin product URLs use the product's UUID (`/admin/products/<uuid>`), not the slug; the slug is the stable per-tenant key used by APIs and imports |
| Tagline | Short marketing line stored on the product. Optional; also used as the Stripe Product description when Description is blank |
| Badge | Short label stored on the product — "Popular", "New", "Premium". Optional |
| Description | Internal/marketing copy stored on the product. It is **not rendered to buyers anywhere today** — the public checkout page reads only the product's name and type. It is passed to Stripe as the Stripe Product description |

### Product Scope & Defaults {#product-defaults}

**Vertical** locks the product to one vertical (insurance, solar, …) and **cannot be changed after creation** — archive the product and recreate it to move verticals.

**Status** — one of `active`, `inactive`, `archived`, or `expired`. **Only `active` products can be bound to a new contract**: a purchase or wizard completion that lands on a product in any other status **fails with a validation error** rather than quietly falling back. So `inactive`/`archived`/`expired` are not just "hidden from buyers" — take a product out of `active` only when you want purchases of it to stop hard. Existing contracts already linked to the product are unaffected.

**Default wizard** is the onboarding wizard buyers run when they purchase any SKU under this product. It is the second step of a four-step chain, resolved at purchase time: the **SKU's own wizard** first, then this product default, then the **offer's** wizard, then the **tenant default**. A wizard that is not `active` (or is the wrong type, or belongs to another tenant) is skipped and resolution continues down the chain. Only wizards matching the product's type (lead or call) can be selected here. See [Onboarding Wizard Builder](#onboarding-wizard-builder).

**Default contract options** (lead products only) are offered to the contract form when an admin picks this product. They are **never applied silently** — choosing the product opens a confirmation modal, and the values land only if you accept it. Two things to know: `contractStatus` in the JSON is **ignored on the admin path** (admin-created contracts always start in `setup`, because an `active` default would trip the activation gate before delivery is configured); it is honored only when a buyer completes the onboarding wizard. The JSON shape matches the wizard's contract-options output, so admin-created and wizard-created contracts stay compatible:

```json
{
  "contractStatus": "setup",
  "sellType": "cpl",
  "geoFilters": { "states": ["CA", "TX"] },
  "demographicFilters": {},
  "customFilters": []
}
```

### Product Billing Link {#product-stripe}

**Stripe product ID** links this product to a Stripe Product — one Stripe Product per product here. Leave it blank and one is created automatically, but only under three conditions: it happens **on create only** (editing a product with a blank ID does not backfill one), your tenant must have a connected Stripe account, and the call is **fail-soft** — if Stripe is misconfigured or errors, the product is still saved with a blank ID and the failure is reported to error tracking rather than blocking you. Paste an ID in later, or recreate, to fix it. Stripe *Prices* are not set here: they stay per-SKU, on the SKU itself.

### Product-Based Contracts {#product-based-contracts}

Attaching a product to a SKU changes what a buyer's repeat purchases consolidate *on*:

- **SKU → Product** (optional) links a SKU bundle to a product. The SKU must belong to the **same vertical** as the product.
- **Contract → Product** tags a contract as the consolidation anchor for that buyer's purchases of the product. Subsequent SKU purchases of the same product **fund that existing contract** instead of creating a new one.

**Repeat purchases already consolidate without a product.** A SKU purchase with no product link looks for a live contract for that **buyer + vertical** and funds it; a new contract is created only when there isn't one. The product link narrows the match key from *(buyer, vertical)* to *(buyer, product)* — which is what you want when one buyer buys two different products in the same vertical and you need them kept on separate contracts.

**Two flags, two different jobs.** The `sku` tenant feature flag is what reveals the Products admin pages. A **separate** `productContracts` rollout flag decides which consolidation code path runs at checkout: with it on (or for any call product), the purchase goes through the product-contract path, which reuses the buyer's existing contract for that product and silently skips the onboarding wizard on the repeat purchase. With it off, the legacy path runs — it still keys on the product when the SKU has one, but with the older status and wizard handling.

Funding mechanics for SKU-backed contracts are under [Billing & Lead SKUs](#billing-and-lead-skus).

---

## Billing & Lead SKUs {#billing-and-lead-skus}

Lead SKUs are self-serve billing products that let buyers purchase a fixed quantity of leads up front. Unlike the legacy balance-based flow (where buyers pre-fund a wallet and pay per lead), a SKU hard-caps both **how many leads** the buyer receives and **which verticals** those leads can belong to. Each purchase creates a `buyerSku` row that the routing engine decrements on every successful delivery and refuses to use once empty.

SKUs are sold through **Stripe Connect Standard**. Each tenant onboards its own Stripe account, so the card statement shows the tenant's brand (not iSCALE). Funds settle directly to the tenant's bank. iSCALE takes a configurable platform fee (default 5%, stored in basis points on the `tenant` record) via Stripe's `application_fee_amount` / `application_fee_percent`.

Three purchase modes are supported:

| Mode | Description | Credits |
|---|---|---|
| **One-time SKU** | Fixed price, fixed lead count, vertical-gated. Stops at 0. | `buyerSku` row with `leadsRemaining` |
| **Subscription SKU** | Same as above but auto-rebills (month/year). Each cycle creates a new `buyerSku` row for audit trail. | New `buyerSku` per cycle |
| **Custom top-up** | Buyer enters an arbitrary dollar amount. Pure balance credit. No vertical gating. | `buyer.balance` increment |

<!-- react-flow:billing-lifecycle -->

### Tenant Plan, Credits & Auto-Recharge {#tenant-plan-credits}

This section covers what **you** (the tenant) pay the platform — separate from the buyer-facing SKU billing above, which is money your buyers pay you.

**The plan.** There is one subscription: **Lead Router — $97/month, 1,000 credits/month included**. New tenants subscribe during signup (see [Plan Checkout at Signup](#signup-plan-checkout)) or from **Billing → Plans & Services**.

**Two credit pools.** Your credit balance is two separate pools:

| Pool | Where it comes from | Expiry | Spend order |
|---|---|---|---|
| **Subscription credits** | Included with your monthly plan, plus any [credit subscription](#credit-subscriptions) tier | **Reset at every renewal** — unused credits do not roll over | Spent **first** |
| **Purchased credits** | Credit bundles — auto-recharge top-ups | **Never expire** | Spent second |

Spending subscription credits first protects the credits you paid extra for: use-it-or-lose-it credits burn down before your never-expiring ones are touched. The **header credits widget** shows both pools on every admin page (e.g. `1,000 plan · 5,000 bought`) with an ⓘ explainer describing how the pools work and your current auto-refill setting; on small screens it collapses to a single total. Clicking it opens **Billing → Plans & Services**.

**The billing page.** **Billing** is organized into five tabs:

- **Overview** — plan status, balance summary, the API Calls This Period card, and payment method/invoice access.
- **Plans & Services** — your plan, [monthly credit subscriptions](#credit-subscriptions), your [auto-recharge](#auto-recharge) bundle and threshold, the Platform Services toggles, and the Service Rates table.
- **Support** — [support tier](#support-tiers) selection.
- **Activity** — the usage summary and full transaction ledger.
- **Diagnostics** — balance reconciliation and billing event history.

There is no one-off "buy credits" button: credits arrive through [auto-recharge](#auto-recharge) top-ups and [credit subscriptions](#credit-subscriptions) (removed 2026-07-31 — old `?tab=credits` bookmarks and emailed links land on Plans & Services).

**Fractional balances.** Credits are tracked to 4 decimal places, so metered usage bills **exactly** — 99 call minutes at a 2.5 credits/min custom rate deducts 247.5 credits, never a rounded-up 248. Your balance may therefore display fractional values like `247.5`; that's normal, not a display bug.

**What credits buy.** 1 credit = $0.01. Metered platform usage draws from your credit balance (subscription credits first, then purchased credits):

| Usage | Rate | Effective cost |
|---|---|---|
| AI Dialer | 50 credits/min | $0.50/min |
| Call Tracking | 10 credits/min | $0.10/min |
| Tracking number lease | 150 credits/number/month | $1.50/number/month |
| Marketing email (campaigns + flows) | 0.1 credits/send | $1.00 per 1,000 emails |
| Marketing SMS (campaigns + flows) | 1 credit/send | $10.00 per 1,000 SMS |
| Lead ping | 0.05 credits/ping | $0.50 per 1,000 pings |
| Lead post (submit) | 0.3 credits/lead | $3.00 per 1,000 leads |
| API request (over the included quota) | 0.01 credits/call | $1.00 per 1,000 calls |

**Lead usage billing (daily aggregation).** Lead pings and lead posts are **not** deducted per event. A daily cron aggregates the previous UTC day's volume and writes one ledger entry per service per day (e.g. "Lead pings 2026-07-07 (48,120 events)"), with the daily total rounded **up** to whole credits. **Call-tracking minutes bill in 5-minute windows** instead: every 5 minutes a cron aggregates the minutes from calls that just ended and deducts the exact amount (minutes × your per-minute rate, fractional credits allowed — no rounding), so your balance is never more than about 5 minutes stale and auto-recharge can react intraday. A nightly sweep re-checks the previous day and bills any window a 5-minute run missed. Test leads are free and never counted. Billing is idempotent — a re-run of the same day never double-charges. If your balance can't cover a day's usage, lead flow is **not** interrupted retroactively: the failure is recorded in the audit log, auto-recharge is nudged, and the unbilled day is re-billed after you top up. Intake itself still requires a positive credit balance — at zero balance new pings and posts are rejected with a 503 `CAMPAIGN_PAUSED` until you add credits.

**Marketing send billing.** Email and SMS **campaign and flow** sends from the Messaging module bill nightly from your credits (one ledger entry per channel per day, e.g. "Marketing email sends 2026-07-06 (12,340 sends)"). You bring your own sending provider — this is the platform fee only. Transactional traffic is free: lead delivery emails, system notifications, and sends made directly through the messaging API are never billed. Insufficient credits never block sends; unbilled days are retried after you top up.

AI-powered features (delivery-spec parsing, AI token mapping, lead enrichment, AI Agent chat) don't use flat rates — they bill **from credits based on actual usage**. See [AI usage billing](#credits-billing) below.

#### API call billing {#billing-api-calls}

Every request you make to the public API (`/api/v1/*`) with an **API key** is metered. Your plan includes a monthly allowance — **5,000 calls** on the Lead Router plan — and only calls beyond that allowance cost credits, at **0.01 credits per call ($1.00 per 1,000 calls)**.

**What counts.** Only API-key traffic to `/api/v1/*`. Three categories never count:

- **Lead posting endpoints** — `/api/v1/leads/ping`, `/api/v1/leads/post`, `/api/v1/leads/submit`, `/api/v1/leads/bulk`, `/api/v1/leads/bulk-import`, and `/api/v1/ping`. Those already bill per ping/lead at the rates above; metering them would double-charge.
- **Dashboard traffic** — anything you or your team do in the admin, buyer, or partner UI while signed in with a session. Browsing the app is always free.
- **Anything outside `/api/v1/`** — webhooks, tracking pixels, and public endpoints.

**How it's billed.** Calls are counted into a daily total per UTC day. Overnight, a cron adds up your calls for the current billing period and bills only the portion that pushed you past the included allowance, rounded **up** to whole credits. Because the calculation is period-to-date, a day only costs you credits once your period total crosses the allowance — and then only for the calls above it. The allowance resets with each billing period.

**Where to see it.** **Billing → Overview** shows an **API Calls This Period** card with your usage against the included allowance, plus how many included calls remain (or how far over you are). Ledger entries appear in the transaction history as `API calls <date> (<n> over-quota requests)` with the service `api_request`.

**If your balance is short.** API access is never cut off retroactively. The failed day is recorded in the audit log, auto-recharge is nudged, and the unbilled day is re-billed once you top up.

**Cutover.** Metering only bills days on or after the platform-wide start date (`credits.apiUsageBillingStartDate`). Calls made before metering went live are never billed.

#### Credit bundles {#credit-bundles}

Credit bundles are the units [auto-recharge](#auto-recharge) buys for you — you pick one in the onboarding funnel and can change it any time under **Billing → Plans & Services → Auto-Recharge**. (One-off "Buy Credits Now" purchases were removed 2026-07-31; bundles are bought exclusively by auto-recharge.)

| Bundle | Credits | Bonus |
|---|---|---|
| $50 | 5,000 | — |
| $500 | 51,000 | +2% free |
| $1,000 | 105,000 | +5% free |

Each bundle card shows an **"Enough for any one of…"** estimate — how many call-tracking minutes, leads, or AI-dialer minutes the bundle covers **at your account's own rates** (the lines are alternatives, not a combined total). The price displayed is always exactly what your card is charged — bonus credits are extra credits on top, never a marked-up sticker price; if your plan carries a credit discount, the discounted price is both what's shown and what's charged.

Bundle credits land in your **purchased pool** — they never expire and are never touched by a renewal reset.

#### Monthly credit subscriptions {#credit-subscriptions}

If you top up every month anyway, a **credit subscription** is cheaper than buying bundles one at a time. It adds a fixed block of credits to your allowance every month and bills as a second recurring line on the plan subscription you already have:

| Tier | Price | Credits per month |
|---|---|---|
| Starter | $45/month | 5,000 |
| Growth | $180/month | 22,000 |
| Scale | $400/month | 60,000 |

**How the credits behave.** Tier credits join your **subscription pool**. Subscribing grants the full tier credits **immediately**, and your first charge is prorated to the days left in the current cycle. At each renewal your subscription pool is **set to** your plan's included credits plus your tier's credits — subscription credits **reset rather than roll over**. Credits you bought as bundles live in the purchased pool and are never touched by a renewal reset.

**Changing tiers.** Moving up grants the difference right away and invoices the prorated upgrade. Moving down grants nothing now and takes effect at your next renewal, when the smaller allowance applies.

**Cancelling.** Cancel from **Billing → Plans & Services**. The recurring line is removed with no proration — you keep the cycle you already paid for, and credits already granted stay on your balance. At the next renewal your pool resets to your plan's included credits alone.

**Estimating what you need.** The [pricing page](https://theleadrouter.com/pricing) has a credit calculator: enter your expected monthly volume and it totals the credits at the published rates, then recommends the smallest tier that covers you. **Pings and posts are separate inputs** — typing a post volume suggests a ping volume at 10:1, but the moment you type your own ping number, that suggestion stops overriding it. If your usage fits inside the credits your plan already includes, the calculator says so instead of recommending a tier; if it exceeds the largest tier, it points you to sales for volume pricing.

**Where it's offered.** Credit subscriptions live on **Billing → Plans & Services**, where you can subscribe, change tier, or cancel at any time — they are a billing-page add-on, not part of the signup funnel. Accounts billed by invoice manage this through their account manager rather than self-serve.

**Superadmin: managing the tiers.** **Superadmin → Billing → Credit Subs** is the CRUD for the global tier ladder. Every save reconciles the tier's Stripe product and recurring price; changing a price mints a new immutable Stripe price and archives the old one, while tenants already subscribed keep renewing on the price Stripe bills them until they change tiers themselves. Deactivating a tier stops it being offered but does not cancel existing subscribers — the editor reports how many tenants that affects. Unlisted tiers are hidden from tenants for negotiated placements.

#### Auto-recharge — always on {#auto-recharge}

Auto-recharge is **built into the platform, not an optional feature** — every self-billed account keeps a recharge bundle configured, and there is no off switch. That's what keeps leads flowing: you never wake up to a zero balance and a stalled campaign.

**How it works.** Pick a **bundle** and a **threshold** under **Billing → Plans & Services → Auto-Recharge** (or during onboarding — the funnel step is required for new tenants). When your combined balance (subscription + purchased) drops below the threshold (default **500 credits**), the platform charges your saved card for the chosen bundle and the credits land in your **purchased pool** (they never expire). The card shows a plain-English summary of exactly what will happen — e.g. "When my balance falls below 500 credits, charge $50 for 5,000 credits."

- **When it checks.** Immediately after any usage deduction, plus a background sweep every 10 minutes — with **at most one automatic top-up per hour**. If one bundle per hour isn't keeping up with your volume, pick a bigger bundle.
- **No card setup needed.** The card from subscription checkout is saved automatically and powers auto-recharge. If no card is on file yet, the Auto-Recharge section shows a warning with an **Add payment method** button that opens the billing portal. Swapping cards in the portal moves auto-recharge to the new card automatically.
- **Declines.** A failed charge sends tenant admins a payment-failed email (once per decline episode), records the failure, and retries automatically on later checks. After **5 consecutive declines**, auto-recharge pauses itself as a card-safety measure: the header widget shows **"Auto-refill paused"**, the Auto-Recharge section shows an amber "not active" notice, and a final update-your-card email goes out. Update the card via the billing portal, then **Save** your bundle settings to resume — recharging picks right back up.
- **Low-balance warning.** When your total balance crosses below the threshold, tenant admins also get a low-balance warning email (once per episode). This fires at the 1,000-credit default even if you never adjusted the threshold.

#### Support tiers {#support-tiers}

Support comes in four tiers. **Community is included free** with every plan and is built around the always-on AI support stack; three paid tiers add humans:

| Tier | Price | What you get |
|---|---|---|
| Community | Free (included) | 24/7 AI support agent built into the platform, a Claude Code skill with API key access, Telegram group support, and the help center |
| Standard | $100/month | Everything in Community + human email support with a 48-hour response time |
| Priority | $500/month | Everything in Community + human chat support during business hours + done-for-you setup (funnels, buyers & integrations) — sized for smaller operations |
| Concierge | $1,000/month | Everything in Community + human chat support + done-for-you setup at scale, with dedicated onboarding for larger operations |

The AI support stack is free on every tier, always — paid tiers add human help on top of it, they never gate it.

**Where to manage it.** Open **Billing → Support** — a four-across grid with your current tier badged in place (including a **Comped** badge if the tier is provided free). Choosing a paid tier opens a confirmation with the prorated charge, then applies it immediately; downgrades and removal (back to Community) run from the same grid. Paid tiers **renew with your monthly plan** and are billed as a second line item on the same subscription. If your account is billed by invoice (not self-serve card), tier changes are handled by your account manager and the button surfaces a message directing you there.

**Support channels.** When your tier includes a direct channel, the links/instructions appear in a **Support channels** panel on the same tab once your account manager has configured them.

**The post-signup funnel order.** New tenants move through a short one-time sequence right after their first plan checkout, each step shown only once:

1. **Plan checkout** — subscribe to the plan (see [Plan Checkout at Signup](#signup-plan-checkout)).
2. **Auto-recharge bundle** (`/welcome/recharge`) — pick the bundle behind your always-on [auto-recharge](#auto-recharge). The $50 bundle comes pre-selected; hit **Continue** to keep it or pick a bigger bundle for bonus credits first. This step is **required** — it's how auto-recharge gets configured; each bundle shows its "Enough for any one of…" coverage at your rates.
3. **Support tier** (`/welcome/support`) — pick a paid tier or continue with free Community.
4. **Setup checklist** (`/admin/setup`) — the guided checklist that takes you from zero to routing leads. If you closed the tab before finishing steps 2–3, the checklist carries **"Auto-recharge — turn on"** and **"Support — add"** items until they're configured.

Superadmins can set or comp a tenant's support tier directly from the tenant detail page (**Superadmin → Tenants → [tenant] → Support Tier**); manual placement there does not create a prorated charge.

#### AI usage billing {#credits-billing}

AI features are billed **from your credit balance based on actual usage** (1 credit = $0.01, minimum 1 credit per charge). There is no flat per-use rate — a cheap call costs a few credits, a heavy one costs proportionally more.

**Which features bill this way:**

| Feature | Where | Billed as |
|---|---|---|
| Delivery-spec AI parsing | Contract Delivery tab → **Analyze Spec** ([Import from Posting Spec URL](#import-delivery-spec)) | One charge per analysis |
| AI token mapper | Template editor → **AI Token Mapping** modal | One charge per mapping run |
| Lead enrichment | [Enrichment integrations](#lead-enrichment-integrations) during lead processing | **One charge per enriched lead**, summing the costs of the providers that actually ran |
| AI Agent chat | [AI Assistant](#ai-assistant) | **One charge per chat turn.** The charge uses the exact cost reported by the AI provider for the turn (including prompt-cache pricing); if that figure is unavailable, the platform computes it from the turn's token counts and the model's per-token rates. Superadmin sessions are tracked but never billed. |

**How the charge is computed.** Every AI call reports its exact usage; the credit charge scales with that real usage — nothing is polled or estimated. (Internally, cost-plus billing applies the superadmin-configured AI markup to the platform's actual provider cost; that computation and the recorded platform cost are visible only on the superadmin AI costs dashboard, never in tenant-facing surfaces.)

**Zero balance blocks AI tools.** The spec parser and token mapper check your balance *before* calling the AI — at zero credits the action is blocked with an insufficient-credits error until you add credits. Enrichment billing is different: it **never blocks or fails a lead** — if the billing write fails, the lead still processes normally.

**Where rates are configured (superadmin).** **Superadmin → Billing → Service Pricing** has two cost-plus controls:

- **AI markup %** — the markup applied on top of actual AI cost for all cost-plus services (default 20%).
- **Enrichment provider costs (USD)** — what each enrichment provider charges the platform per lead. ⚠️ **These ship as placeholders (Versium $0.03, IPQS $0.004, other $0.01) — replace them with your real provider contract prices before relying on enrichment billing.**

The tenant-facing rates listing shows these services with a neutral `per use (variable)` unit rather than a flat credit rate — the provider cost and markup percentage are never shown to tenants.

**Disclosure.** A "Billed from credits per use" note (with a help tooltip) appears next to each billable AI action: the Analyze Spec section on the contract Delivery tab and the AI Token Mapping modal in the template editor.

**Exempt tenants.** Billing-exempt tenants are tracked at 0 credits, but the platform cost is still recorded for margin reporting.

#### Payment methods & invoices {#tenant-plan-payment-methods}

Card and invoice management runs through the **Stripe-hosted billing portal**. Open it from any of three places on **Billing**:

- **Overview** — the **Manage payment methods & invoices** button.
- **Past-due banner** — the **Update payment method** button on the amber "Payment failed" banner.
- **Plans & Services tab** — the **Add payment method** button on the "No payment method on file" warning in the Auto-Recharge section.

In the portal you can **add or swap cards** and **view your invoice and receipt history**. Card changes made in the portal sync back automatically — a swapped card immediately becomes the card auto-recharge charges.

You rarely need to add a card manually: the card used at **subscription checkout** is saved automatically.

#### Cancellation & resume {#tenant-plan-cancel}

**Canceling.** Cancel the plan from **Billing → Plans & Services**. Cancellation takes effect at the **end of the current billing period** — you keep full access until then, and the billing banner switches from "renews {date}" to **"Access until {date}"**. When the period ends, the subscription closes and your remaining **monthly plan credits are zeroed**. **Purchased (bundle) credits are never touched** — anything you bought stays spendable after cancellation, and voluntary cancellation **never locks or pauses services** (see [Payment failures](#tenant-plan-past-due) — locks only apply to unresolved past-due or a passed conversion date). A **cancellation confirmation email** is sent when the subscription fully ends.

**Resuming (un-cancel).** While in the access-until grace window, a **Resume** button replaces Cancel on the billing page. Clicking it un-cancels the subscription: status returns to active, the next renewal proceeds on the normal date, and nothing is charged immediately. Once the period has actually ended, Resume is no longer offered — re-subscribe from the plan card instead.

#### Payment failures (past due) {#tenant-plan-past-due}

If a monthly renewal charge fails, the subscription enters **past due**:

- **Email** — tenant admins get a payment-failed email (sent exactly once per failure transition) with a link to update the payment method.
- **Banner** — an amber **"Payment failed"** banner with a grace countdown shows **across the whole admin app** (not just the billing page) until the card is fixed or a retry succeeds.
- **Retries** — Stripe retries the card automatically on its dunning schedule. A successful retry restores active status; exhausted retries cancel the subscription (plan credits stop; purchased credits remain spendable).

**Grace period → usage-service pause timeline:**

- **Days 0–7 (grace)** — banner and email only. **Every service keeps running normally.**
- **Day 7+ (still unpaid)** — **usage services pause**: AI Dialer stops placing calls, Call Tracking stops accepting **new** calls, and tracking-number provisioning/auto-buy stops. What is **not** affected: **lead routing and ingestion keep flowing**, and the **dashboard stays fully accessible** — you can log in, see everything, and fix the card. Admins get a services-paused email when the pause takes effect, and the banner turns red with an **Update payment method** CTA.
- **Card fixed** — resolving the payment (via the banner button or the [billing portal](#tenant-plan-payment-methods)) **resumes all paused services instantly** — no waiting period, no support ticket.

**Voluntary cancellation is different.** Cancelling your plan yourself **never pauses or locks anything** — a canceled tenant with purchased credits keeps using every service (see [Cancellation & resume](#tenant-plan-cancel): purchased credits are never touched). Pauses apply only to unresolved past-due beyond the 7-day grace, and to a passed [billing-conversion date](#tenant-plan-billing-conversion) without an active plan.

**Disputes.** If a cardholder disputes a credit charge, the disputed credits are clawed back while the dispute is open. If the dispute resolves in your favor (**won**), those clawed-back credits are **restored automatically** — no support ticket needed.

#### Scheduled billing conversion {#tenant-plan-billing-conversion}

Tenants on complimentary (billing-exempt) access can be converted to paid billing on a **scheduled date** instead of an abrupt cutoff.

**What the tenant sees:**

- From the moment the date is set, a tenant-wide **countdown banner** — "complimentary access ends {date}" — shows across the admin app with a **Subscribe** button.
- Tenant admins get reminder emails at three points: **when the date is scheduled**, **7 days before (T-7)**, and **1 day before (T-1)**.
- **On the date**, billing flips on automatically (an hourly job handles it — no one has to click anything) and a conversion-active email is sent. From here, normal credit deductions and billing rules apply.
  - **If you already have an active plan** — nothing pauses; usage simply starts drawing from your credit balance.
  - **If you don't have a plan yet** — usage services lock (same scope as the past-due pause: AI Dialer, new call tracking, number provisioning; **lead routing and the dashboard keep working**) and the banner turns red with a **Subscribe** CTA. **Subscribing unlocks everything immediately.**

**What the superadmin does:**

- **Superadmin → Tenants → [tenant] → Billing** has a **billing starts** date picker. Setting a date schedules the conversion (confirm-modal gated, audit-logged) and immediately triggers the tenant's scheduled email + countdown banner. **Clearing the date** cancels the scheduled conversion — the banner disappears and no flip happens.
- The **Subscription card** on the same page shows the tenant's computed **enforcement state** as a status badge: `ok`, `grace` (past-due within 7 days), `locked_past_due`, `conversion_pending` (date set, not yet reached), or `locked_conversion` (date passed, no active plan).
- On the date, the flip is automatic: the tenant's billing exemption is removed and billing mode switches to self-serve. Setting a date in the past converts immediately on the next hourly run.

**Purchased credits are never a lock trigger.** As with [payment failures](#tenant-plan-past-due), a tenant who voluntarily cancels but still holds purchased credits is **never locked** — those credits stay spendable and services keep running. Locks come only from unresolved past-due beyond grace, or a passed conversion date with no plan.

#### Superadmin: credit adjustments {#billing-adjustments}

**Location:** [Superadmin](/superadmin) → [Billing](/superadmin/billing) → **Adjustments** tab

A manual credit correction against one tenant's balance — for goodwill credits, billing disputes, or fixing a mis-charge.

- **Tenant** — the account to adjust.
- **Credits** — positive adds, negative deducts. **Cannot be zero.**
- **Description** — the reason, and it is **required**: it is stored on the credit transaction so the adjustment is explainable later.

Every adjustment writes an `adjustment`-type credit transaction stamped with the acting superadmin (`performedBy`) plus an audit-log entry recording the credits, reason, and resulting balance. It appears in the tenant's transaction history alongside purchases, deductions, and auto-recharges.

#### Superadmin: plans & coupons {#platform-coupons}

- **Superadmin → Billing → Plans** — create/update subscription plans. Saving a paid plan **auto-creates the Stripe product and price** on the platform account (re-pricing archives the old Stripe price and creates a new one); no manual Stripe setup.
- **Superadmin → Billing → Coupons** — create **$-off promo codes** for the plan subscription: name, code, dollar amount off, and duration **once** (first invoice only) or **forever** (every invoice). Each coupon is backed by a Stripe coupon + promotion code on the platform account and can be deactivated from the same table. Share codes on conference links as `/get-started?promo=CODE`. These platform coupons are distinct from the tenant-level [Referrer Program & Coupons](#referrer-program), which discount your buyers' purchases on your own connected Stripe account.

#### Superadmin: unlisted plans & shareable plan links {#billing-plan-links}

Every plan has a **visibility** setting: **public** (default — listed in the `/get-started` wizard and pricing page) or **unlisted** (hidden from all public lists, but fully purchasable via its link). Unlisted plans are for **one-off deal packages** — e.g. a custom monthly price + credit bundle negotiated with a single prospect — that shouldn't appear next to the standard plan.

- **Shareable link.** Each plan row in **Superadmin → Billing → Plans** shows its signup link — `theleadrouter.com/get-started?plan=<slug or id>` — with a copy button. Set an optional **link slug** on the plan for a friendly URL (unique across plans); with no slug the link uses the plan ID.
- **Wizard behavior.** Opening a plan link shows that plan as the sole, preselected **"Your Custom Plan"** card in the signup wizard (name, price, monthly credits, feature bullets). The selection survives refreshes mid-wizard; opening a different plan link replaces it. An invalid, inactive, or unknown link silently falls back to the normal public plan list — never an error page.
- **Composability.** Plan links combine freely with `?promo=CODE`, `?ref=`, and sub-id parameters on the same URL.
- Flipping a plan to unlisted never affects tenants already subscribed to it — it only hides the plan from new public signups.

#### Superadmin: unlisted credit packages {#billing-package-links}

Credit packages have the same **visibility** setting as plans: **public** (default — offered in tenant-facing bundle pickers: the auto-recharge section and the onboarding funnel) or **unlisted** (hidden from those lists). Use unlisted packages for **one-off deals** — e.g. a custom credit bundle at a negotiated price a superadmin configures for a specific tenant.

- **Share links removed (2026-07-31).** Instant one-off purchases no longer exist, so the per-package `?buy=` share link and its copy button are gone from **Superadmin → Billing → Packages**. Previously emailed `theleadrouter.com/admin/billing?buy=<slug or id>` links still open safely — the customer just lands on their **Billing → Plans & Services** tab; nothing is highlighted and nothing is sold. The optional **link slug** field remains on the package editor only so those legacy URLs keep resolving.
- Flipping a package to unlisted never affects past purchases or an existing auto-recharge configuration pointing at it — it only hides the package from the public pickers.

#### Superadmin: subscription controls {#superadmin-subscription-controls}

**Superadmin → Tenants → [tenant]** has a **Subscription** card showing the tenant's plan, subscription status, current period end, and the computed **enforcement state** badge (see [Scheduled billing conversion](#tenant-plan-billing-conversion)), with two confirm-modal-gated actions:

- **Cancel** — period-end cancel by default, or **immediate** via a toggle in the confirm modal. Immediate cancel ends access right away with no automatic refund (refund manually in the Stripe dashboard if warranted).
- **Resume** — available while the tenant is in the canceling grace window; un-cancels the subscription back to active.

Tenant lifecycle operations handle the Stripe subscription automatically:

- **Deleting a tenant** cancels their Stripe subscription **immediately**.
- **Suspending a tenant** sets a **period-end cancel** — reversible via Resume if the tenant is reinstated (reinstating does **not** auto-resume; click Resume explicitly).
- If the Stripe cancel call fails, the delete/suspend still completes — the failure is audit-logged for follow-up rather than blocking the operation.

All cancel and resume actions write audit log entries.

### Platform Services {#platform-services}

Platform Services are metered add-ons that run on the operator's own credentials — **AI Dialer** (outbound AI voice) and **Call Tracking** (inbound call routing). Unlike Integrations, you don't bring your own keys: the platform provides them and bills you per minute of usage in credits. Manage them under **Billing → Plans & Services** in the **Platform Services** card, or from the locked sidebar item for each service, which opens an enablement landing page.

**Enabling a service.** Each service shows its credit-per-minute rate (credits only — no dollar estimate). Click **Enable** (on the landing page) or flip the toggle in Plans & Services. Enabling a metered service requires a payment method — if none is on file you get a prompt to add one, and the service stays off until you do. You're only charged for minutes actually used; there's no upfront commitment.

**Disabling.** Toggling a service off stops future billing for it. Disabling **AI Dialer** also pauses its active outbound dialing (you'll be asked to confirm), so leads already queued for AI calls won't be dialed until you re-enable it. Re-enabling resumes normal operation.

**Rates** are shown live in-app (both on the enablement landing page and in the Plans & Services card) and are set by the operator — see Superadmin below. Credits arrive via [auto-recharge](#auto-recharge) on **Billing → Plans & Services**.

<!-- react-flow:platform-services-flow -->

#### Call Tracking telephony: platform-provided {#platform-services-byo}

Call Tracking and AI Dialer are **platform-provided only** for tenants — inbound calls route on the operator's telephony and you pay the per-minute credit rate. There is no tenant-facing bring-your-own (BYO) toggle.

For special arrangements, a superadmin can set a per-tenant **service access override** on the tenant detail page — see Superadmin below. On a `byo` override, usage still writes a ledger entry for reporting, but the tenant is billed at the reduced **BYO rate** (default **1 credit/min**, superadmin-configurable per service) instead of the standard per-minute rate — not zero. On an `exempt` billing mode, usage is tracked at 0 credits.

#### Superadmin: system config & service access {#platform-services-superadmin}

Two superadmin surfaces govern Platform Services:

- **Superadmin → AI Dialer** — the platform ElevenLabs config (API Key, Webhook Secret, limits, and the display-only tenant rate/minute) lives here. This moved off the tenant Integrations page; tenants no longer see or manage an ElevenLabs card. Secrets are stored masked and preserved on save unless a new value is entered.
- **Superadmin → Tenants → [tenant] → Service Access** — per-service overrides. Each service can be set to `default` (tenant self-serves normally), `granted` (force-on regardless of billing), `revoked` (force-off, hides the service), or `byo` (bring-your-own credentials, billed at the reduced BYO rate). This lets the operator comp, disable, or grandfather a service for a specific tenant.
- **Superadmin → Billing → Service Pricing** — standard credit rate per service, plus a **BYO rate** (credits/min) for the per-minute platform services (Call Tracking, AI Dialer). BYO tenants are billed at this rate; it defaults to 1 credit/min when unset. The same tab holds the cost-plus controls — **AI markup %** and **Enrichment provider costs (USD)** — for the AI features billed at cost + markup (see [AI usage billing](#credits-billing)).

#### Superadmin → Billing → Costs & Margins {#costs-margins}

**Modeled margin at list & bundle tiers** — answers "are bigger credit bundles still profitable?" *before* you create one. It is **forward-looking and modeled**, distinct from the two backward-looking actuals tabs on the same page: **AI Costs** and **Service Costs** sum the real provider cost recorded on each transaction, while Costs & Margins projects the theoretical margin from the provider rates you enter.

**Provider costs & margin floor.** Enter what each provider charges the platform, grouped by provider:

- **SignalWire** — voice (USD/min), number rental (USD/mo), SMS (USD/msg)
- **ElevenLabs** — conversational agent (USD/min)
- **LLM** — per dialer minute and per agent request (USD)
- **Email** — per send (USD)
- **Margin floor (multiple)** — the minimum acceptable revenue-to-cost ratio. Cells below it show red; just above show amber; healthy show green.

Shipped values are **placeholders** — enter your real contract rates. Saving writes them as `credits.cost.*` platform settings and recomputes the matrix.

**Margin matrix.** Rows are services; columns are **List** (the standard 1 credit = $0.01 rate) followed by each active credit bundle, ordered from the shallowest to the deepest discount (a bundle's effective price per credit is `price ÷ credits`, shown as ¢/cr under its name). Each cell is the modeled **revenue-to-cost multiple** at that tier, colored against the margin floor. A service maps to one or more provider cost components (e.g. **AI Dialer** = ElevenLabs agent + SignalWire voice + LLM/min; **Tracking Number** = SignalWire number rental). Lead intake (ping/post) is pure-margin infra and cost-plus services (AI Agent, Enrichment) show no fixed provider cost (**—**) because their margin is fixed by the AI markup, not a flat rate.

All money in the model is computed in integer micro-USD (µUSD) to avoid rounding drift; the displayed multiples are the only floating-point values. This tab is **display and configuration only** — it never moves money.

#### Superadmin → Billing → System Costs {#system-costs}

**Live platform spend — what the providers actually bill us.** This is the missing aggregate side of "what does it cost to run the whole system." A daily collector cron pulls each provider's real platform-account bill/usage into a time-series, and this tab rolls it up. Do not confuse the three cost surfaces on this page:

- **System Costs (this tab)** — the **actual** dollars providers bill the platform account, pulled live from their billing/usage APIs.
- **Costs & Margins** — **modeled** margin from the provider rates you enter (forward-looking, "are bundles profitable?").
- **AI Costs / Service Costs** — the **per-transaction** cost we recorded on each tenant's usage (`platformCostUsd`), summed.

**Cost tiers.** Every cost carries a tier, the primary roll-up dimension:

- **Tier 1 — Platform overhead**: fixed OpEx we always pay, not tied to any tenant's revenue (Neon, Vercel runtime, Resend notification email).
- **Tier 2 — COGS (billed-for-tenants)**: the platform pays the provider then bills the tenant via credits; scales with tenant usage (shared SignalWire, ElevenLabs). This is the tier the Costs & Margins tool cares about.
- **Tier 3 — BYO**: the tenant runs its own provider account and pays the provider directly — **$0 to the platform, excluded from every sum** (Twilio validation-only + any tenant-scope telephony credential). Shown only as a muted footnote.

`Total System Cost = Tier 1 + Tier 2`. The optional **Gross Margin** stat (toggle "Show gross margin") = platform revenue (credit purchases + subscription MRR) − Tier 1 − Tier 2; its secondary "Tier-2 only" figure reconciles with Costs & Margins.

**Provider breakdown.** Each provider row shows its cost and a status badge: **Live** (pulled from the API), **Manual** (an overlay you entered), **Needs credential: VAR** (names the exact env var to add — e.g. `SIGNALWIRE_SPACE_URL`, `ELEVENLABS_API_KEY`), **Plan-gated — manual $** (Neon consumption API is Scale/Business-only), **Error**, or **BYO — not tracked**. When a live collector has no usable dollar figure, use **+ Manual Entry** to overlay the real bill (provider, category, period, USD amount, tier 1|2 — Tier 3 is rejected). A live `ok` figure always wins over a manual overlay for the same provider/period.

> **Temporarily unavailable (being reworked).** The per-tenant P&L breakdown is currently **turned off** while it is reworked, so it cannot display incorrect per-tenant money figures. The tab shows a "temporarily unavailable" placeholder in its place and the API returns 404 for that breakdown. The platform-level rollup above (tiers, providers, trend, gross margin, manual entry) is fully live. The description below documents the P&L as it will return.

**Per-tenant P&L (the primary view).** A **Tier-2 usage-margin** view — it turns the account-level Tier-2 bills into per-tenant usage revenue vs COGS:

- Each **tenant row** is Usage Revenue | Subscription (flat) | Cost | Usage Margin for the period; expand it for per-(provider, service) line items (e.g. ElevenLabs → AI Dialer, SignalWire → Call Tracking, SignalWire → SMS).
- **Cost** = that tenant's share of the real Tier-2 account bill, split by their measured **usage share** (units) using the largest-remainder (Hamilton) method — so the per-tenant cents sum **exactly** to the account bill and line items sum exactly to the tenant-row cost. BYO exclusion is **provider-scoped**: a tenant on their own carrier drops out of the SignalWire denominator but still counts toward the ElevenLabs one (the agent cost is still ours).
- **Usage Revenue** = credits the tenant consumed on that service × the **effective $/credit they actually paid** — the purchased rate if they bought credits, else the plan-implied rate (so bundle discounts are reflected; this is *not* the list rate). A tenant with no revenue basis shows $0 and a "—" margin.
- **Subscription (flat)** = the tenant's monthly plan fee, shown for context only. It is **excluded** from Usage Revenue and from the margin — the flat fee is recognized *through* consumption at the plan-implied rate, so adding it again would double-count it (a fully-consumed $97 plan reads $97 of usage revenue, never $194).
- **Usage Margin** = (Usage Revenue − Cost) / Usage Revenue, colored against break-even; the table sorts by margin so unprofitable tenants surface first. The footer **Platform Total** reconciles to the tier roll-up.
- **Reconciliation** badges (per Tier-2 provider) compare the **modeled** cost (sum of recorded `platformCostUsd`) against the **actual** live bill. A variance beyond ±15% flags that your configured `credits.cost.*` rates have drifted from what the provider really charges — the live bill is ground truth.

> **Known limitation:** this is explicitly a *usage-margin* view, not a full per-tenant revenue statement. True total-revenue-per-tenant with plan-vs-purchased funding-bucket attribution needs a `fundingSource` tag on credit-deduction rows (not yet in the schema), so the flat subscription fee is reported separately rather than blended into a single revenue figure.

This tab records cost and computes margin; it **never moves money**.

#### Usage P&L — Superadmin → Billing → Tenant Breakdown, and your Billing → Activity tab {#usage-pnl}

**One calculation, two views.** Usage P&L answers "what did each tenant consume, what did it earn, and what did it cost us" straight from the credit ledger — no provider-bill allocation, so it is unaffected by the reworked per-tenant P&L above. The same aggregation function serves both surfaces, which is what makes the numbers agree by construction rather than by convention:

- **Superadmin → Billing → Tenant Breakdown** — one row per tenant with **Credits | Revenue | Cost | Net**, sorted by net ascending so loss-makers appear first. Click the chevron to expand a tenant into its per-service lines (SMS, Email, AI Dialing, …) with units, credits, revenue, cost and net.
- **Admin → Billing → Activity** — the tenant's own view of the same period: **Service | Units | Credits**, plus a totals row (above the transaction ledger).

**Parity is the point.** A tenant's units and credits are identical on both screens for the same period, because both call the same aggregation. Platform cost and net are superadmin-only and are never serialized to a tenant-facing response — the tenant endpoint returns only service, units and credits.

**How the numbers are derived:**

- **Source** — `deduction` and `platform_deduction` rows on the tenant credit ledger, grouped by tenant × service. Purchases, grants, refunds and adjustments are not usage and never appear. Only services in the billable-services registry produce lines, so platform infrastructure (Neon, Vercel) can never surface as a tenant cost.
- **Credits** — the sum of credits consumed. The ledger stores whole credits, so this is exact.
- **Revenue** — credits × 1¢ face value. This is the credit's billing worth, not cash collected; subscriptions are counted separately and are not usage.
- **Cost** — the platform cost recorded on each ledger row. Cost-plus services (AI Agent, Enrichment, AI Dialer) carry a real figure; SMS and Email are 0 by bring-your-own design, and Call Tracking reads 0 until per-row cost is wired. A 0 there is honest, not missing.
- **Net** — revenue − cost. A **billing-exempt tenant shows 0 revenue against real cost, so its net is negative** — that loss view is exactly what the tab is for.

**Period.** Both pickers default to the current calendar month and read the ledger in **UTC** (the ledger's own clock), not tenant-local time — the two views must cover the identical set of rows. The **To** date is inclusive. Daily-billed services (lead posts, lead pings) are totalled by a nightly cron, so the most recent day can lag.

### For Admins {#skus-for-admins}

**Location:** [/admin/settings/billing](/admin/settings/billing) and [/admin/skus](/admin/skus)

#### Connect Stripe

1. Go to **Settings > Billing**
2. Click **Connect Stripe** — this creates a Stripe Connect account link and redirects you to Stripe's hosted onboarding
3. Complete the Stripe onboarding flow (business details, bank account, tax info)
4. Stripe redirects back to `/admin/settings/billing` — the page polls Stripe and shows:
   - **Charges Enabled** (green = ready to accept payments)
   - **Payouts Enabled** (green = Stripe can wire funds to your bank)
   - **Details Submitted** (green = onboarding complete)
5. The `account.updated` webhook keeps these flags in sync if you update info later on Stripe's dashboard

Until **Charges Enabled** is true, all checkout attempts return a `CONNECT_NOT_READY` error.

#### Platform Fee (read-only for tenant admins)

The platform fee is the cut iSCALE takes on every Stripe Connect charge — applied to one-time SKUs, subscription SKUs, and custom top-ups via Stripe's `application_fee_amount` / `application_fee_percent`. It is stored in basis points (`500` bps = 5%, `100` bps = 1%).

As of 2026-04-25 this is **superadmin-only**. Tenant admins see the effective fee on **Settings > Billing** as read-only ("Platform fee: 5.00% (set by platform)") and cannot change it. This prevents a tenant from setting their own fee to 0 to avoid the platform cut.

How the effective rate is resolved (in order):

1. **Per-tenant override** — `tenant.platformFeeBps` if non-NULL. Set by a superadmin from **Superadmin > Tenants > [tenant] > Billing > Platform Fee Override**.
2. **System default** — `systemSetting['billing.defaultPlatformFeeBps']`. Set by a superadmin under **Superadmin > Billing > Platform Fee** tab.
3. **Hardcoded fallback** — `500` bps (5%) if neither is set.

To change the rate for a single tenant, set their override. To change the rate for everyone without an override, edit the system default. Clearing an override (Save with the field blank or click **Clear override**) returns that tenant to the system default.

#### Require SKU for Delivery

Toggle **Require SKU for lead delivery** on the same page. When enabled, the routing engine will **only** deliver leads to buyers who have an active SKU covering the lead's vertical. Buyers without a matching SKU are skipped even if they have balance. When disabled (default), the engine falls back to the legacy balance-based flow if no SKU is active.

This flag is tenant-scoped. Turn it off for grandfathered customers that still pay per-lead against a wallet.

#### Creating a SKU

1. Go to **Finance > SKUs** and click **+ New SKU**
2. Fill the form:
   - **Slug** — URL identifier, e.g. `bronze`, `silver`, `gold`. Must be unique within the tenant.
   - **Name** — display label, e.g. "Bronze SKU"
   - **Tagline** — short marketing copy
   - **Badge** — optional eyebrow label, e.g. "Most Popular"
   - **Price (cents)** — one-time cost or per-cycle cost for subscriptions
   - **Lead count** — hard cap (e.g. 15 for Bronze)
   - **Max lead types** — how many verticals the buyer can pick at checkout (e.g. 1 for Bronze, 3 for Gold)
   - **Allowed verticals** — optional whitelist. If empty, all tenant verticals are eligible.
   - **Subscription** toggle — when on, pick **Billing Interval** (`month` or `year`)
   - **Available Until** — optional promo cutoff date (YYYY-MM-DD, tenant timezone). SKU is available through this date (inclusive) and hidden from the shop the day after. Past-cutoff SKUs return 410 from public checkout endpoints. Leave blank for "no cutoff".
   - **Sort order** — controls display order on the pre-checkout page
   - **Active** — off = retired (no new purchases, existing SKUs unaffected)
3. Click **Save**. On first checkout for this SKU, iSCALE creates the Stripe `Price` and `Product` on the tenant's connected account and caches the IDs on the SKU row.

#### SKU Detail {#sku-detail}

**Location:** [/admin/skus](/admin/skus) > SKU Detail

Three tabs with **one floating save bar** (edits on the Settings tab send only the changed fields; Cancel reverts to the last saved values):

- **Overview** — public signup/checkout URLs (with copy buttons), a summary of the saved configuration, and the delete danger zone. If the SKU is inactive, a note warns that both URLs return 404.
- **Settings** — scope (vertical/product — frozen once a buyer purchase exists), basics, pricing, onboarding wizard/offer pre-assignment, billing mode, and visibility toggles, with a live preview card of your unsaved changes. Price is frozen once a non-refunded purchase exists.
- **Stripe** — the cached Stripe product/price IDs and a **Sync to Stripe** button (applies immediately).

Deep links to the old `?tab=edit` land on Settings automatically.

#### Bulk SKU import {#bulk-sku-import}

Use the **Import** button on `/admin/skus` to provision a batch of SKUs from a JSON list. This replaces the "click through `+ New SKU` six times" flow when standing up a fresh tenant or rolling out a campaign with several pricing tiers.

**When to use:**
- New tenant launch — provision the full catalog (e.g. Silver/Gold/Platinum + weekly variants) in one click.
- Promo refresh — apply corrected per-lead pricing across multiple SKUs.
- Migrating from a spreadsheet/Notion doc that already lists SKU specs.

**JSON format.** Either a bare array, or an object with an `items` array. Each item uses the same shape as the single-SKU create form:

```json
[
  {
    "slug": "life-silver",
    "name": "Silver Life",
    "tagline": "35 leads",
    "priceCents": 129500,
    "leadCount": 35,
    "verticalId": "<life-vertical-uuid>",
    "isSubscription": false,
    "allowPromoCodes": true,
    "active": true,
    "sortOrder": 10
  }
]
```

A starter list for the May Special promo lives at `apps/routing/scripts/data/may-special-skus.json` — copy it, swap the placeholder `verticalId` for the tenant's real Life vertical UUID, and paste.

**Dry-run preview.** Click **Validate (dry run)** first. The endpoint never writes to the DB on dry-run; it returns one row of preview data per item:

| Action  | Meaning                                                  |
|---------|----------------------------------------------------------|
| create  | Slug doesn't exist for this tenant — will INSERT         |
| update  | Slug exists with at least one differing field — will UPDATE; the `diff` shows `from → to` for every changed field |
| skip    | Slug exists and every field matches — no-op              |
| error   | Row failed validation or hit a DB error                  |

**Applying.** Click **Apply N changes** to write. Each row is committed independently — if 1 row fails out of 50, the other 49 still land. The summary toast reports `X created, Y updated, Z skipped, W errors`.

**Limits.** Maximum 100 items per call. Larger batches must be split. The endpoint is idempotent on `(tenantId, slug)`, so re-running the same JSON is safe (you'll see lots of `skip` rows on re-run).

**Stripe.** Imported SKUs are NOT pre-synced to Stripe. The Stripe `Price` + `Product` are created lazily on the first checkout for that SKU (matches the single-SKU create flow). To force the sync earlier, open the SKU detail page and use the **Re-sync** button under the Stripe Sync tab.

**Audit.** Every `create` and `update` writes one row to the audit log with `action: 'sku_create' | 'sku_update'`, `entityType: 'sku'`, and `details.source: 'bulk-import'` so you can grep imports vs. manual edits.

#### SKU Sync Tab

The **Stripe Sync** tab on a SKU detail page shows the cached Stripe `product_id` and `price_id`, plus a **Re-sync** button to regenerate them if needed (e.g. after a price change).

#### Refunding a Buyer's SKU

1. Go to **Buyers > [buyer] > SKUs** tab
2. Find the SKU row and click **Refund**
3. Pick **Full refund** (reverses the latest paid charge and the platform fee pro-rata) or **Partial refund** (prorates based on unused leads)
4. For subscription SKUs, leave **Cancel the recurring Stripe subscription after refunding** checked unless the subscription should stay billable
5. Confirm — the system calls `stripe.refunds.create({ refund_application_fee: true })`, sets `buyerSku.status = 'refunded'`, sets `buyerSku.routeabilityStatus = 'refunded'`, and sets `buyerSku.leadsRemaining = 0`
6. A row is written to the audit log

Refunded SKUs cannot be used by the routing engine.

#### Reissue a SKU {#skus-reissue}

Use **Reissue** when a buyer ended up with the wrong SKU but you don't want to refund — they keep the money on the platform, you just swap the underlying pack. This is an internal ledger move only; **no Stripe charge or refund** is fired.

**When to use:**
- Buyer picked the wrong vertical at checkout (e.g. checked out "Auto" but meant "Life")
- Customer wants to upgrade a tier mid-pack ("I want Gold instead of Silver")
- Admin correction for a mis-provisioned subscription cycle

**Don't use Reissue for refund requests** — Reissue keeps the funds on platform. If the buyer wants their money back, use **Refund** instead.

**How:**
1. Go to **Buyers > [buyer] > SKUs** tab
2. Find the source SKU row and click **Reissue**
3. In the modal, pick the **Target SKU** and decide **Carry consumed leads**:
   - **ON** — the new pack credits the leads already delivered. Example: source had 35 leads with 9 consumed; new pack opens at `targetSku.leadCount − 9` remaining (so a 35-lead target opens at 26).
   - **OFF** — the new pack opens at the full `targetSku.leadCount` (the consumed leads on the source are forgiven).
4. Confirm

**What happens:**
- Source `buyerSku` → `status='reissued'`, `reissuedToBuyerSkuId` set to the new pack
- Source's linked contract → `status='archived'` (it's no longer eligible for routing)
- New `buyerSku` minted: `status='active'`, `reissuedFromBuyerSkuId` set to the source, `leadsRemaining` set per the carry-consumed rule above
- A new contract is minted for the new pack and linked via `contract.buyerSkuId`
- No Stripe API calls. No charge. No refund.
- One row is written to the audit log: `transaction.type='sku_reissue_audit'`, `amount=0`, with `notes` describing the reissue

**Audit trail.** The `reissuedFromBuyerSkuId` / `reissuedToBuyerSkuId` columns form a backward/forward chain — follow them to see every reissue in a pack's history. The zero-amount `sku_reissue_audit` transaction row is grep-able for accounting reconciliation.

**Edge case.** If `carryConsumed=true` and the source has consumed more leads than the target's total (e.g. source consumed 12, target only has 10), the handler rejects with `VALIDATION_ERROR` — pick a larger target or turn off carryConsumed.

<!-- react-flow:sku-lifecycle -->

#### Deleting a SKU

`DELETE /api/admin/skus/[id]` returns `400 VALIDATION_ERROR` with `code: 'SKU_IN_USE'` if any active `buyerSku` references it. To retire a SKU without removing history, set **Active = off** instead — existing `buyerSku` rows continue to drain normally but no new purchases are allowed.

#### Sharing a SKU {#sku-sharing}

On the SKU detail page (**Overview** tab, **Public URLs** card), the **Share** button opens a dialog for sending a SKU link to a specific prospect or buyer.

**Link type.**
- **Buyer portal** (default) — `/buyer/billing?sku=<slug>`. The recipient signs in (login preserves the destination), and the billing page auto-opens checkout with the coupon prefilled. Best for existing buyers. This link works even for **UNLISTED** SKUs (Buyer Portal = Hidden): a hidden SKU stays out of the catalog but a shared deep link resolves it. If the recipient is already logged in as a buyer of this tenant and opens a public checkout link, they are quietly bounced to this in-portal flow.
- **Public checkout** — `/checkout/<slug>?tenant=<slug>`. Goes straight to Stripe with no login, for cold prospects.

**Coupon (optional).** Attach a promotion code from the picker, which lists only active codes whose coupon applies to this SKU. The code is embedded in the link and re-validated when the link is opened, so an expired or disabled code simply won't discount.

**Copy or send.** The dialog shows a live link preview with a **Copy** button. To send directly, pick **Email** or **Text**, enter the recipient, add an optional personal note (up to 500 characters), and **Send**. Sends go out through the platform (system) email and SMS providers — if the platform provider for the chosen channel is not configured, the send is rejected with a clear error. Every send writes a `sku.share_sent` audit row with the recipient **masked** (no raw email or phone is stored). Delivered emails also appear in the email log.

### For Buyers {#skus-for-buyers}

**Location:** [/buyer/billing](/buyer/billing) and [/buyer/skus](/buyer/skus)

#### View Balance + Active SKUs

The **Billing** page shows:
- Current wallet **balance** (used for legacy per-lead pricing and custom top-ups)
- **Active SKUs** grid — each card shows the SKU name, leads remaining / leads total, selected verticals, and status
- **Buy More Leads** — SKU picker grid
- **Custom Top-Up** — amount input ($50 minimum, $10,000 maximum per transaction)

Legacy `/buyer/balance` redirects to `/buyer/billing`.

#### Buying a SKU

1. Click a SKU card in the **Buy More Leads** grid
2. In the vertical picker modal, select up to **Max lead types** verticals (the SKU's configured limit)
3. Click **Continue to Stripe**
4. Stripe Checkout opens (tenant's connected account, card statement shows tenant brand)
5. On success you're redirected back to `/buyer/billing` and the webhook provisions the SKU in the background — refresh after a few seconds if it doesn't appear immediately

#### Custom Top-Up

1. Enter an amount between $50 and $10,000
2. Click **Top Up**
3. Stripe Checkout opens, buyer pays, webhook credits `buyer.balance`
4. Balance updates on return to `/buyer/billing`

#### SKU History

**Location:** [/buyer/skus](/buyer/skus)

Shows every SKU the buyer has ever purchased, filterable by status (`active`, `exhausted`, `cancelled`, `refunded`, `expired`). Each row includes purchased date, leads total, leads remaining, selected verticals, and the Stripe payment intent ID for receipt lookup.

### Landing Page Flow {#skus-landing-page-flow}

This flow lets a tenant sell SKUs from any external marketing site with zero iSCALE branding:

1. **Tenant hosts marketing page** on any domain (WordPress, static HTML, Webflow — doesn't matter)
2. **CTA button links** to `https://portal.theirbrand.com/checkout/bronze` (or whatever SKU slug). The domain must be configured as a `customDomain` for the tenant.
3. **Pre-checkout page** at `/checkout/[skuSlug]` loads tenant branding via host, shows the SKU summary (including the SKU's single bound vertical), and collects email + name
4. **Stripe Checkout** opens on the tenant's connected account. Card statement shows the tenant name — true white-label.
5. **Webhook receives** `checkout.session.completed` and:
   - Finds or creates a `buyer` (de-dupes by email within the tenant)
   - Creates or links a portal user
   - Inserts the `buyerSku` row with `leadsRemaining = sku.leadCount` and the selected verticals
   - Issues a single-use **magic token** (15-min TTL) stored on `checkoutSession`
6. **Redirect** to `/welcome?session=cs_xxx` consumes the token, sets the `lr_session` cookie, and redirects to `/buyer`
7. **Later sign-in** happens through `/login` with magic link, Google, or passkey

No account creation, no password friction on first visit, no iSCALE logo anywhere in the path.

#### Returning active-buyer handling {#skus-returning-active-buyer}

When a returning buyer (already `status='active'` for the tenant) submits the public checkout form, the page does **not** send them to Stripe — instead it renders an inline panel:

> **Looks like you already have an account.**
> Sign in to reorder with your saved card and billing details.
> [ Sign in to reorder ]   [ Use a different email ]

"Sign in to reorder" links to `/buyer/billing?sku=<slug>` and preserves any entered coupon as `coupon=<code>`. The buyer billing purchase modal's share icon copies the same SKU/coupon link. If the visitor has no live session, middleware bounces them through `/login` and returns them to the same billing link. Active buyers must reorder from the portal — they get their saved Stripe customer + card, and the order history stays clean.

This is a **security boundary**, not just a UX choice. Allowing an unauthenticated caller to start a Stripe checkout against an existing active buyer's email would let them mint a portal session for that buyer via the Stripe `success_url` / `/welcome` magic-token flow (the success URL lands in the requester's browser, not the buyer's inbox). The route returns HTTP 422 `BUYER_EXISTS_ACTIVE` for any active-email match regardless of request body shape; the Stripe webhook adds defense-in-depth by refusing to email-bind a null-`buyerId` `checkoutSession` to an active buyer (the session is marked `failed` and the incident is logged to Sentry + audit log as `checkoutSession.refused_active_buyer_bind`).

### Subscription vs One-Time {#skus-subscription-vs-one-time}

| Behavior | One-Time SKU | Subscription SKU |
|---|---|---|
| First charge | `checkout.session.completed` creates first `buyerSku` | Same — first `buyerSku` is created on the initial `checkout.session.completed` |
| First `invoice.paid` | N/A | **Skipped** (`billing_reason = subscription_create` — no-op, SKU already exists) |
| Subsequent `invoice.paid` | N/A | Creates a **new `buyerSku` row** per cycle (audit trail preserved) |
| Cancel | N/A | Prior `buyerSku` drains normally; no new renewals |
| Failed payment | N/A | Stripe Smart Retries → `past_due` → `canceled`. Current SKU marked `expired`, no new row. |
| Refund | Full reverses entirely; partial prorates unused leads | Full targets the latest paid subscription invoice; partial prorates unused leads; `refund_application_fee: true` gives back the platform fee proportionally |
| Fee mechanism | `application_fee_amount = round(priceCents * feeBps / 10000)` | `application_fee_percent = feeBps / 100` (Stripe requires percent for recurring) |

To see every active subscription across all buyers in one place, use **Finance → Subscriptions** (`/admin/subscriptions`). The page lists each recurring `buyerSku` row (anything with a `stripeSubscriptionId`). Filters: buyer, lifecycle status (defaults to `active`), Stripe billing state, and renewal payment status. The default view also hides rows whose billing has been canceled (you can still see them by explicitly filtering Billing → Canceled).

Columns:
- **Status** — lead-pack lifecycle (`active`, `exhausted`, `expired`, `cancelled`, `refunded`, `disputed`, `reissued`). Tells you whether THIS specific pack is still alive.
- **Billing** — Stripe subscription state (`active`, `paused`, `canceled`). Tells you whether the subscription will renew next cycle. Independent from Status — a buyer can have `Status=active` with `Billing=canceled` (still has unused leads but won't be billed again).
- **Lead Price** — the per-lead price captured at purchase (`unitPriceCents`). Immutable on the row.
- **Per Cycle** — calculated: `Lead Price × leadsTotal`. A projection of what a full renewal cycle WILL bill at the agreed rate.
- **Total Billed** — actual gross billed historically. Sum of `grossAmountCents` across every `buyerSku` row sharing the `stripeSubscriptionId` (one row per cycle Stripe creates). Per Cycle ≠ Total Billed when the first cycle had a discount, coupon, or partial-period adjustment.
- **Leads** — `leadsRemaining / leadsTotal` for the current pack.
- **Renews** — `servicePeriodEnd` from the most recent Stripe invoice line. Populated on initial purchase (post-`checkout-completed`) and on every renewal.

Use it to spot stuck renewals (`past_due`, `failed`), confirm pause toggles took effect, or audit the current book of recurring revenue. It is read-only — pause and cancel actions still happen in the buyer detail or buyer self-service portal.

### SKU Gating Behavior {#skus-gating-behavior}

The routing engine checks SKUs in `engines/routing.ts` immediately after contract filter matching and before the balance check. SKU consumption is **implicit on `contract.buyerSkuId`** — there is no tenant-level toggle:

1. If `contract.buyerSkuId` is null → balance path (legacy per-lead pricing applies)
2. If `contract.buyerSkuId` is set → the engine looks up the linked `buyerSku`. It must be `status='active'` and `leadsRemaining > 0`, or the contract is rejected with a reason mentioning "sku exhausted/missing"
3. On delivery success, `decrementSku()` runs an atomic `UPDATE buyerSku SET leadsRemaining = leadsRemaining - 1 WHERE id = ? AND leadsRemaining > 0 RETURNING *`. Loser of a race (rowCount = 0) is rejected and routing moves to the next buyer — no oversell.
4. When a decrement brings `leadsRemaining` to 0, status flips to `exhausted` in the same update

**Known gap:** the decrement currently runs **after** the delivery HTTP call, so a lost race in a high-concurrency burst can cause a free lead to be delivered before the SKU is updated. This is accepted MVP behavior (races are rare) and tracked as tech debt.

### SKU / Product System (Tenant Feature Flag) {#skus-tenant-feature}

Per-tenant `features.sku` flag controls whether the SKU/Product admin UI, public Stripe checkout, buyer self-purchase, and contract auto-creation are visible/usable for a tenant. Lives on `tenant.features` JSONB. Default for new tenants: **off**.

<!-- TODO: add diagram for contract funding modes -->

#### What enabling does

Flipping `features.sku` ON for a tenant:

- Reveals the **SKUs** and **Products** sidebar items under Finance
- Exposes admin pages: `/admin/skus`, `/admin/skus/new`, `/admin/skus/[id]`, `/admin/products/*`
- Exposes admin mutation APIs: `POST /api/admin/skus`, `PUT /api/admin/skus/[id]`, `POST /api/admin/skus/bulk-import`, `POST /api/admin/products`, `PUT /api/admin/products/[id]`
- Exposes admin buyer-SKU non-recovery routes (`/api/admin/buyer-skus/*` except the recovery endpoints listed below)
- Adds the "Funding" + "Leads left" columns on the contract list (default-ON)
- Enables the public Stripe checkout flow at `/checkout/[skuSlug]`, buyer self-purchase at `/buyer/skus`, and contract auto-creation from a paid SKU

#### Call product unit consumption

Call products have a **Call Unit Consumption** section on the product create/edit pages:

- **Unit Consumption Event** — `raw_call`, `connected`, or `duration`
- **Duration Threshold Seconds** — required when the event is `duration`
- **Start Call Length On** — `Connect` (default), `Dial`, or `Incoming`

For duration-based prepaid call SKUs, the target must answer before a unit is consumed. If the target answers, `Start Call Length On` controls whether the threshold is measured from answer, outbound dial, or inbound arrival. Existing duration call products without an explicit anchor use `Connect`.

#### Toggle behavior

The flag lives under **System → Features → SKU / Product System** (also called "Prepaid Lead Packs" in tenant onboarding). FieldHelp on the toggle states:

> Adds the SKU / Product system: SKU/Product pages, public Stripe checkout, buyer self-purchase, contract auto-creation. Disabling hides admin UI only. It does NOT cancel Stripe subscriptions, close the public `/checkout/[slug]` URL, or stop new purchases from `/buyer/skus`. Active buyers continue purchasing and renewing. To fully stop SKU sales: cancel Stripe products + revoke buyer portal access separately. Disable is soft-blocked when active SKU subscriptions or pending checkouts exist.

Disable is **soft-blocked** when any of the following are true:

- A `buyerSku` row has `status IN ('active','disputed','reissued')`
- A `buyerSku` row has `routeabilityStatus IN ('paused','payment_failed')`
- A `buyerSku` row has `subscriptionPauseStatus = 'paused'`
- A `buyerSku` row has `stripeSubscriptionId IS NOT NULL AND subscriptionPauseStatus != 'canceled'`

The toggle UI runs a preflight check that opens a confirmation modal listing concrete blast radius before submission. The server-side enforcement uses `SELECT ... FOR UPDATE` on the tenant row inside the mutation transaction.

#### Field-locking on SKU-funded contracts

Contracts created from a paid SKU have `contract.buyerSkuId IS NOT NULL`. On those contracts, 27 economic/provenance fields are **read-only in the UI** and **rejected by `PATCH /api/v1/contracts/[contractId]`** (400 + `VALIDATION_ERROR` with reason `sku_contract_guard`). Locked categories:

| Category | Fields |
|---|---|
| Provenance | `buyerId`, `verticalId`, `buyerSkuId`, `productId` |
| Pricing | `pricePerLead`, `pricingModel`, `exclusiveMultiplier`, `revenueOnAcceptance`, `minimumBidFloor` |
| CPA | `revenueOnCpa`, `cpaAmount`, `cpaWindowDays`, `cpaLookbackDays`, `cpaLookbackExcludeDays` |
| Call billing | `callEnabled` and call billing rule writes |

Caps, filters, schedule, delivery configuration, dispositions, and lifecycle controls (`ruleType`, `allowMultiSale`) remain editable — admins manage routing and scheduling on SKU-funded contracts as normal.

A `ContractFundingChip` renders in the contract status header whenever `contract.buyerSkuId IS NOT NULL` **regardless of the flag**. Chip text varies:

- `features.sku=true` → "Funded by SKU · {name} · {N} left" with a **Manage SKU** link
- `features.sku=false` → "Funded by SKU (legacy)" (no Manage link)
- Orphaned FK (buyerSkuId set, row missing) → "Funding source unavailable" badge
- `leadsRemaining=0` → "0 leads left" badge

Amber callouts at the top of the **Leads** and **Calls** tabs read: "This contract is funded by a SKU. Pricing, CPA, and call billing are locked — manage on the SKU page →". Locked pricing/CPA/call-billing fields render read-only with a "from SKU" hint, and the Lead Sales / Call Tracking capability toggles are locked to the product's sell type.

#### Audit trail

All `tenant.features` changes write to `auditLog`:

- Successful change → action `features_changed`, inside the same transaction as the `UPDATE tenant` (rolls back together on error)
- Blocked attempt → action `features_change_blocked`, written in a **separate** transaction (default DB handle, before the soft-block throws) so the blocked row survives the validation rollback

#### Documented limitations

1. **Buyer portal not gated.** `/buyer/skus` and `/buyer/billing` continue working when admin disables `features.sku`. Existing buyers can still purchase and renew.
2. **Public checkout not gated.** `/checkout/[slug]` and the public SKU APIs (`GET /api/v1/public/skus`, `GET /api/v1/public/skus/[slug]`) remain reachable. To fully stop sales, cancel the Stripe products and revoke buyer portal access separately.
3. **Stripe state machine coverage limited.** The soft-block predicate keys off DB-mirror columns (`status`, `routeabilityStatus`, `subscriptionPauseStatus`, `renewalPaymentStatus`, `stripeSubscriptionId` presence). Stripe-side states `incomplete`, `trialing`, and `past_due` are not directly queried.
4. **Webhook race not guarded.** If admin flips the flag OFF mid-checkout, the in-flight Stripe webhook still processes, the `buyerSku` is created, and contract auto-creation still runs even though the sidebar nav is now hidden. The funding chip continues rendering on the resulting contract (Decision 9 — flag-OFF chip text becomes "Funded by SKU (legacy)").
5. **15s UI cache stale window.** Cached `isSkuFeatureEnabled()` reads can lag the flip by up to 15s on warm Vercel lambdas. Mutation paths (page gates on `/admin/skus/*`, API gates on `POST/PUT /admin/skus/*`) use `isSkuFeatureEnabledFresh()` which bypasses the cache, so security boundaries do not stale.
6. **Subscription renewals continue regardless of flag.** Stripe `invoice.paid` keeps creating new `buyerSku` rows for active subscriptions after the flag is disabled. The Decision 4(d) soft-block predicate blocks the disable while any `stripeSubscriptionId IS NOT NULL AND subscriptionPauseStatus != 'canceled'` exists, preventing the worst case at the source.

#### Admin recovery routes (exempt from gating)

Even when `features.sku=false`, these admin routes remain reachable so admins can wind down an existing SKU footprint:

- `POST /api/admin/buyer-skus/[id]/cancel`
- `POST /api/admin/buyer-skus/[id]/refund`
- `POST /api/admin/buyer-skus/[id]/reissue`
- `POST /api/admin/buyer-skus/[id]/manual-credit`

The corresponding **buttons** on the buyer detail SKU tab are hidden when the flag is off (UX cleanup), but the routes themselves remain callable by other admin tooling.

### Stripe Connect Setup Checklist {#skus-connect-setup}

**Environment variables** (set in Vercel project settings):

| Variable | Purpose |
|---|---|
| `STRIPE_SECRET_KEY` | Platform secret key (`sk_live_...`). Used for all Stripe API calls. |
| `STRIPE_WEBHOOK_SECRET_PLATFORM` | Signature verification for platform-level events (Connect account updates, `account.updated`) |
| `STRIPE_WEBHOOK_SECRET_CONNECT` | Signature verification for connected-account events (`checkout.session.completed`, `invoice.paid`, `charge.refunded`, etc.) |

The webhook handler at `/api/v1/stripe/webhook` tries both secrets and uses whichever one validates.

**Stripe dashboard webhook endpoints** (register once, both point to the same URL):

1. **Platform webhook** — in Stripe dashboard > Developers > Webhooks > Add endpoint:
   - URL: `https://theleadrouter.com/api/v1/stripe/webhook` (or your portal domain)
   - Events: `account.updated`
   - Copy the signing secret to `STRIPE_WEBHOOK_SECRET_PLATFORM`
2. **Connect webhook** — add another endpoint on the same page with "Connected accounts" checked:
   - URL: same as above
   - Events: `checkout.session.completed`, `checkout.session.expired`, `invoice.paid`, `invoice.payment_failed`, `customer.subscription.deleted`, `charge.refunded`
   - Copy the signing secret to `STRIPE_WEBHOOK_SECRET_CONNECT`

**Stripe API version** is pinned to `2024-12-18.acacia` in `src/lib/stripe/client.ts`. Do not change without testing all webhook handlers.

### Pricing Models & Use Cases {#skus-pricing-models}

Use the SKU fields to create different billing structures for your buyers. Every example below is configurable from **Finance > SKUs** with no code changes.

#### Simple Prepaid Bundle

**Scenario:** "Buy 50 leads for $750."

| Field | Value |
|---|---|
| Price | $750 |
| Lead Count | 50 |
| Subscription | Off |

Buyer pays once, gets 50 leads. When all 50 are delivered, the SKU status flips to `exhausted`. Buyer must purchase again.

#### Monthly Subscription

**Scenario:** "15 auto insurance leads every month for $690/mo, auto-billed."

| Field | Value |
|---|---|
| Price | $690 |
| Lead Count | 15 |
| Subscription | On |
| Billing Interval | month |

Each billing cycle, Stripe auto-charges and the system provisions a fresh 15-lead allocation. Unused leads from the prior cycle do NOT roll over.

#### Annual Subscription (Volume Discount)

**Scenario:** "Commit for a year at a better rate."

| Field | Value |
|---|---|
| Price | $7,200 |
| Lead Count | 200 |
| Subscription | On |
| Billing Interval | year |

$36/lead vs $46/lead on the monthly plan. Annual billing communicates commitment value and reduces churn.

#### Time-Limited Bundle

**Scenario:** "100 leads for $1,200, but use them within 90 days or they expire."

| Field | Value |
|---|---|
| Price | $1,200 |
| Lead Count | 100 |
| Expiry Days | 90 |

Creates urgency. Good for seasonal campaigns or buyers who need accountability to consume leads quickly.

#### Buy 10 Get 5 Free (Bonus Leads)

**Scenario:** "Promotion — pay for 10 leads, receive 15 total."

| Field | Value |
|---|---|
| Price | $500 |
| Lead Count | 10 |
| Bonus Leads | 5 |

Buyer pays $500. System provisions 15 total leads (10 paid + 5 bonus). Checkout displays "15 leads (5 bonus)" and per-lead cost is calculated on paid leads only ($50/lead). Works with subscriptions too — bonus applies on every renewal.

#### Free Trial then Subscription

**Scenario:** "Try 10 free leads for 14 days. If you like it, we auto-bill $690/mo for 15 leads."

| Field | Value |
|---|---|
| Price | $690 |
| Lead Count | 15 |
| Subscription | On |
| Billing Interval | month |
| Trial Leads | 10 |
| Trial Days | 14 |

1. Buyer signs up — card is captured but NOT charged
2. System immediately provisions 10 trial leads
3. After 14 days, Stripe charges $690 and the system provisions 15 paid leads
4. Normal monthly cycle continues
5. If buyer cancels during trial, they keep remaining trial leads and are never charged

#### 30-Day Generous Trial

**Scenario:** "Full month free with 20 leads, then $1,540/mo for 35 leads."

| Field | Value |
|---|---|
| Price | $1,540 |
| Lead Count | 35 |
| Trial Leads | 20 |
| Trial Days | 30 |

Trial has fewer leads than the paid tier (20 vs 35). Buyer experiences the value but wants more — natural upgrade path when the paid cycle starts.

#### Welcome Bonus (No Trial Period)

**Scenario:** "Sign up and get 10 extra leads on your first purchase — charged immediately."

| Field | Value |
|---|---|
| Price | $500 |
| Lead Count | 10 |
| Bonus Leads | 10 |

Unlike a trial, buyer pays upfront. The bonus is a "thank you for signing up" — no risk to the tenant. Buyer gets 20 total leads for the price of 10.

#### Promo Code Discount

**Scenario:** "Give webinar attendees 20% off with code WEBINAR20."

| Field | Value |
|---|---|
| Allow Promo Codes | On |

1. Create a coupon "WEBINAR20" in your Stripe dashboard (20% off, one-time use, or repeating — your choice)
2. Enable "Allow Promo Codes" on the target SKU
3. Buyers enter the code at Stripe checkout and the price is reduced
4. Full lead count is delivered — the discount applies to price, not leads

Stripe coupon types supported: percentage off, fixed amount off, duration (once / repeating / forever).

#### Tiered Pricing (Bronze / Silver / Gold)

**Scenario:** "Three options side by side on a pricing page."

| SKU | Price | Leads | Badge | Sort Order |
|---|---|---|---|---|
| Bronze | $690 | 15 | — | 1 |
| Silver | $1,540 | 35 | Most Popular | 2 |
| Gold | $3,150 | 75 | Best Value | 3 |

Per-lead cost decreases with tier: Bronze $46 > Silver $44 > Gold $42. The **Badge** field shows an eyebrow label on the pricing card. **Sort Order** controls left-to-right display.

#### Vertical-Specific Bundle

**Scenario:** "Only sell solar and auto leads in this package."

| Field | Value |
|---|---|
| Lead Count | 30 |
| Max Lead Types | 2 |
| Allowed Verticals | Solar, Auto |

Buyer must pick exactly 2 verticals from the allowed list at checkout. Routing engine only delivers leads matching those verticals. Protects against buyers cherry-picking high-value verticals after purchase.

#### Single-Vertical Specialist

**Scenario:** "Buyer only does Medicare. Lock them to that vertical, lower price."

| Field | Value |
|---|---|
| Lead Count | 25 |
| Max Lead Types | 1 |
| Allowed Verticals | Medicare |
| Price | $500/mo |

Lower per-lead cost ($20) because the tenant knows the vertical and can price accordingly. Buyer cannot pivot to higher-value verticals mid-contract.

#### Time-Pressured Promo

**Scenario:** "Flash sale: 50 leads for $400 (normally $750). Promo runs through May 31."

| Field | Value |
|---|---|
| Price | $400 |
| Lead Count | 50 |
| Available Until | 2026-05-31 |
| Active | On |

Set **Available Until** to the last day of the promo. The SKU is purchasable through May 31 (tenant timezone) and disappears from the shop on June 1 — no manual flip required. Past-cutoff checkout attempts return 410. Existing purchases continue working until exhausted.

#### Enterprise Annual + Trial + Bonus

**Scenario:** "30-day trial with 25 free leads, then annual billing: 500 leads + 50 bonus, $18,000/year."

| Field | Value |
|---|---|
| Price | $18,000 |
| Lead Count | 500 |
| Bonus Leads | 50 |
| Billing Interval | year |
| Trial Leads | 25 |
| Trial Days | 30 |
| Max Lead Types | 5 |

The full stack: trial + annual subscription + bonus leads + multi-vertical. $36/lead paid. Generous trial closes large buyers.

#### Trial + Promo Code Stack

**Scenario:** "14-day trial with 10 free leads, then 20% off the first paid month."

| Field | Value |
|---|---|
| Trial Leads | 10 |
| Trial Days | 14 |
| Allow Promo Codes | On |

Admin creates a "20% off first month" coupon in Stripe (duration: once). Buyer enters code at checkout, gets the trial, then the first charge is discounted. Subsequent months are full price.

#### Subscription + Recurring Bonus

**Scenario:** "Subscribe monthly and get 5 bonus leads every cycle as a loyalty reward."

| Field | Value |
|---|---|
| Lead Count | 15 |
| Bonus Leads | 5 |
| Subscription | On |
| Billing Interval | month |

Every billing cycle, buyer gets 20 total leads (15 paid + 5 bonus). Bonus applies on every renewal, not just the first purchase. If admin changes the bonus later, the new amount takes effect on the next cycle for existing subscribers.

#### Referral Gift SKU

**Scenario:** "Give a referred buyer 5 free leads as a welcome gift."

| Field | Value |
|---|---|
| Price | $0 |
| Lead Count | 0 |
| Bonus Leads | 5 |
| Active | Off |

A $0 SKU with bonus leads = free gift. Keep it **inactive** so it doesn't show on the public pricing page. Admin sends the direct checkout link (`/checkout/[slug]`) to referred buyers manually.

#### Balance Top-Up (No SKU Needed)

**Scenario:** "Just add $500 to my account — no package needed."

No SKU required. Buyers use the **Custom Top-Up** field on the billing page to add $50–$10,000 to their balance. This credits `buyer.balance` directly — no lead count, no vertical gating.

Only works when **Require SKU for Delivery** is OFF. When ON, the balance exists as a wallet but routing uses SKUs for delivery decisions.

#### Combining Features

All SKU fields can be mixed. Here's the compatibility matrix:

| Feature | One-Time | Subscription | Top-Up |
|---|---|---|---|
| Fixed lead count | Yes | Yes (per cycle) | No (balance) |
| Bonus leads | Yes | Yes (each cycle) | N/A |
| Trial period | N/A | Yes | N/A |
| Trial leads | N/A | Yes | N/A |
| Promo codes | Yes | Yes | No |
| Expiry (days) | Yes | Yes | N/A |
| Vertical gating | Yes | Yes | No |
| Auto-renewal | No | Yes | No |
| Full refund | Yes | Yes (latest invoice) | No |
| Partial refund | Yes (prorated) | Yes (prorated) | No |
| Platform fee | Per txn | Per cycle | Per txn |

### Custom Funnel Integration (API) {#skus-custom-funnel}

For tenants who want to build their own checkout experience (WordPress, Webflow, custom React site, mobile app), the following public API endpoints are available:

#### List Active SKUs

```
GET /api/v1/public/skus
```

Returns all active SKUs for the tenant (resolved via host header). No auth required. Rate-limited by IP. Past-cutoff SKUs (`availableUntil < today` in tenant timezone) are filtered from the list. Response includes: slug, name, tagline, badge, priceCents, leadCount, bonusLeads, verticalId, isSubscription, billingInterval, trialLeads, trialDays, availableUntil (ISO date, nullable), availableUntilDisplay (server-formatted display string, nullable), perLeadCostCents, totalLeads. Single-slug endpoint returns 410 GONE if past cutoff.

#### Get Single SKU

```
GET /api/v1/public/skus/:slug
```

Returns one SKU by slug with full details.

#### Create Checkout Session

```
POST /api/v1/public/checkout/sku
Content-Type: application/json

{
  "skuSlug": "silver",
  "verticalIds": ["uuid-1", "uuid-2"],
  "email": "buyer@example.com"
}
```

Returns `{ data: { url: "https://checkout.stripe.com/..." } }`. Redirect the buyer to this URL. Stripe handles payment. The webhook handles everything else (buyer provisioning, SKU creation, magic link).

#### Custom Funnel Flow

1. Your site calls `GET /api/v1/public/skus` to fetch the SKU catalog
2. You render your own pricing UI (any tech stack)
3. On selection, call `POST /api/v1/public/checkout/sku` with slug, verticals, and email
4. Redirect buyer to the returned Stripe URL
5. After payment, buyer lands on `/welcome?session=...` and is auto-logged into the portal
6. Done — webhook handles provisioning automatically

### Troubleshooting {#skus-troubleshooting}

| Symptom | Likely Cause | Fix |
|---|---|---|
| `CONNECT_NOT_READY` on checkout | Tenant hasn't completed Stripe onboarding, or `chargesEnabled = false` | Admin opens **Settings > Billing** and finishes onboarding |
| Webhook returns 400 "signature verification failed" | Wrong webhook secret or wrong endpoint (platform vs connect) | Verify both `STRIPE_WEBHOOK_SECRET_*` env vars match the dashboard |
| SKU not decrementing after delivery | Delivery failed (buyer endpoint returned non-2xx) | Check lead detail > Delivery tab — decrement only runs on `status = delivered` |
| `buyerSku` row not appearing after Stripe payment | Webhook wasn't delivered or failed idempotency check | Check **Developers > Webhooks** in Stripe dashboard, replay the `checkout.session.completed` event |
| Magic link expired | 15-minute TTL elapsed or token already consumed | Buyer signs in through `/login` with the email they entered at checkout |
| Duplicate `buyerSku` rows | None — the `stripeEvent.id` unique constraint prevents duplicate webhook processing | If observed, file a bug |
| Subscription renewal didn't grant new SKU | First `invoice.paid` is skipped by design (`subscription_create`). Second cycle should create a new row. | Wait for the next cycle, or replay the event from Stripe dashboard |
| Refund didn't reverse platform fee | `refund_application_fee` flag missing | Code path uses `refund_application_fee: true` — if your case doesn't, file a bug |

---

## Referrer Program & Coupons {#referrer-program}

A built-in referral system that pays third parties to send you buyers. Two surfaces work together:

- **Coupons** — Stripe-synced discount codes. Optionally tie one to a referrer for auto-attribution when a buyer redeems it without a `?ref=` link.
- **Referrers** — distinct from `partner` (lead source). A referrer earns a one-time signing bonus on a buyer's first paid checkout, plus a recurring revenue share % on every subsequent subscription invoice.

<!-- react-flow:referrer-flow -->

### Setting up

1. **Stripe Connect required.** The referrer program writes invoices and discount codes through your connected Stripe account. If Connect isn't onboarded, `/admin/coupons` and `/admin/settings/referrer-program` show a CTA to finish onboarding under **Settings > Billing**.
2. **Enable the program.** Go to **Settings > Referrer Program**. Flip the toggle, then set:
   - Default signing bonus (dollars) — paid once per referred buyer's first paid checkout.
   - Default rev-share % — paid against every subscription invoice for that buyer.
   - Rev-share base — `gross_revenue` (full subtotal, excluding tax) or `net_of_platform_fee` (after Stripe Connect's 5% fee).
3. **Per-referrer overrides** — open any referrer's detail page. Override fields blank by default; setting one wins over the tenant defaults.

### Creating a referrer

Two paths:

- **Admin invite.** From **Referrers > Add Referrer**. Fill name + email, then click **Invite** in the row to send a magic-link. The referrer signs in and lands on `/referrer`.
- **Self-signup.** Public page at `/refer-signup` (tenant-scoped via domain). Creates a `pending` referrer and emails the admin for approval. Commissions don't accrue until you flip status to `active`.

### Creating a coupon

**Coupons > Create Coupon**. One form creates BOTH a Stripe coupon AND a 1-1 promotion code in a single step:

- **Code** — what the buyer types in checkout (`SUMMER25`). Uppercase A-Z, 0-9, dash, underscore.
- **Discount** — radio between `$ off` (cents per currency) or `% off` (basis points).
- **Duration** — `once` (single invoice), `repeating` (N months), `forever` (every invoice).
- **Max redemptions / redeem by** — Stripe enforces both.
- **Referrer link (optional)** — if a buyer redeems this coupon without already having a `?ref=` cookie, they're auto-attributed to this referrer.

Editing is intentionally minimal: Stripe only allows updating the **name** field after creation. To change discount/duration, delete and recreate.

Deleting calls Stripe's coupon delete (which cascades the promotion code) AND soft-deletes the local mirror. Existing buyer attributions are preserved.

### Attribution model

Priority order (first match wins):

1. **`?ref=<referrer-id>` query param** on any page request — sets the `lr_ref` cookie (30-day, SameSite=Lax). The token is the referrer's UUID `id` (canonical form). Legacy 8-char codes are still accepted during the transition window.
2. **`lr_ref` cookie** — persists across pages until the buyer hits checkout.
3. **Coupon-driven backfill** — if a buyer reaches `checkout.session.completed` without a referrer attached AND uses a coupon that's linked to a referrer, the referrer is set on `buyer.referrerId` after the fact.

Tenant boundary: a `?ref=<referrer-id>` that resolves to another tenant's referrer is dropped silently (logged to Sentry). No cross-tenant attribution.

### Commission types

| Event | Trigger | When | Idempotency key |
|---|---|---|---|
| Signing bonus | `checkout.session.completed` | Buyer's FIRST paid checkout (`buyer.firstPaidCheckoutAt IS NULL`) | `(tenantId, stripeCheckoutSessionId)` partial unique |
| Revenue share | `invoice.paid` | EVERY subscription invoice (both `subscription_create` and `subscription_cycle`) | `(tenantId, stripeInvoiceId)` partial unique |
| Reversal | `charge.refunded` | Refund of either the original checkout charge OR a subscription invoice | Negating ledger row; partial refunds reverse proportionally |

Self-referral guard: if `referrer.email = buyer.email` (case-insensitive), accrual returns early. No bonus, no error.

Inactive referrer guard: if `referrer.status != 'active'`, accrual returns early. Existing balances stay payable.

### Payouts

For v1, payouts are **manual ACH** — no Stripe Connect transfers. Flow:

1. The `/api/cron/partner-payouts` cron runs daily and creates a `payout` row when `referrer.balance >= referrer.paymentThreshold` AND the next payout date has arrived.
2. Admin opens `/admin/payouts/[id]` and clicks **Mark Paid** when the wire/ACH has been sent. This:
   - Debits `referrer.balance` (SQL-side numeric subtract for race safety)
   - Writes a negating `transaction(type='payment')` row
   - Sends the `referrer-payout-sent` email

### Email cadence

- **Per-event accruals** are batched into a daily digest (`referrer-commission-accrued`) sent by `/api/cron/referrer-commission-digest`. Per-event emails are intentionally avoided to prevent spam.
- **Payout sent** — fires immediately on Mark Paid.
- **New buyer signup** — fires when a buyer attributed to the referrer creates an account.
- **Admin pending approval** — fires when a referrer self-signs up.

### Disabling the program

`Settings > Referrer Program > Enabled` is a hard kill switch. Setting it off makes both `accrueSigningBonus` and `accrueRevShare` return early — no new commissions accrue. Existing balances continue to pay out via the normal payout flow.

---

## Provider Credentials {#provider-credentials}

Unified storage for Twilio, Telnyx, Bandwidth, SignalWire, SES, SendGrid, and Resend credentials. One credential row declares which routing decisions (`capabilities`) it can serve; tenants enable each capability via a separate `providerAssignment` row that picks one default per capability.

<!-- react-flow:provider-assignment-resolution -->

### Overview

Two tables drive the entire system:

- **providerCredential** — the credential itself (encrypted secrets, plaintext config, declared capabilities, optional caps).
- **providerAssignment** — which tenant uses which credential for which capability, with at most one `isDefault=true` per `(tenantId, capability)` and an optional `overrides` JSON for safe non-secret per-tenant tweaks.

Resolution is explicit. A tenant resolves to a credential only through an active default assignment — there is no implicit "fall back to system Twilio" magic. The migration backfill creates assignments mirroring today's behavior, then every send path calls `resolveCredential(tenantId, capability)`.

### My Providers tab

Lists the credentials your tenant owns. Each row shows name, provider, capabilities, masked credentials (first 4 / last 3 chars), and status. Use the **Add Provider** button to register a new credential — pick the provider, enter the per-provider secret + config fields, and narrow the capabilities if you don't want the credential serving every default.

You can edit any owned credential at any time. The credentials are masked by default; click **Replace Credentials** in the edit modal to enter new secret values. Leaving secret fields blank preserves the stored ciphertext.

### System Providers tab

Lists shareable system credentials published by the platform team. For each system credential you can subscribe to one or more capabilities (creates an assignment), pick which capability it should serve as the default, and enter override values within the cap the platform team set.

Overrides are strictly allowlisted per provider — only `fromNumber`, `campaign`, `statusCallbackUrl`, `region`, and `dailySendLimit` are accepted. Anything else is rejected with `VALIDATION_ERROR`. Twilio's `messagingServiceSid` is intentionally NOT overridable on shared system creds — tenants who want their own MG SID must register their own Twilio credential in My Providers.

### Default selection

The **Defaults** tab shows the resolution map: each capability with the credential currently routing it (own or system), plus any alternates. Use this to verify what each routing decision will pick at request time. If a system provider becomes hard-disabled (`shareable=false` mid-soak), a banner surfaces here so you can choose another credential.

**Promoting a credential to default (My Providers):** Each capability row on a credential shows a small chip next to the assignment:
- **★ default** (green) — this credential is the active default for that capability.
- **☆ click-to-make-default** (gray) — this credential serves the capability but isn't currently the default. Click the chip to promote it.

Clicking the gray chip POSTs to `PUT /api/v1/providers/assignments/{assignmentId}` with `{ isDefault: true }`. The server atomically demotes the previous default for the same `(tenantId, capability)` pair in the same transaction, so you never have two defaults at once. Refresh-free — the chip flips to green and the previous default flips to gray on success.

The same pattern works for system credentials you've subscribed to: subscribe first (creates the assignment), then click the chip to make it your default for that capability.

### Overrides + caps

Caps are set superadmin-side via `limits.maxDailySendLimit`. The cap is enforced twice — at write (the API rejects an override above the cap) and at read (the resolver clamps `dailySendLimit` to the cap before returning, so a retroactive cap reduction takes effect immediately).

### Hard-disable behavior

Three things can pull a credential out of resolution:
- `credential.status='inactive'` — the credential is fully off.
- `credential.scope='system' && credential.shareable=false` — the catalog row was retracted.
- `assignment.status='inactive'` — the tenant turned off the assignment.

In any of these cases, `resolveCredential` returns null and the calling site degrades gracefully (delivery error logged, sends paused). A 5-min cron flips assignment.status=inactive whenever its referenced credential becomes hard-disabled, so tenants get same-day visibility in the Defaults tab.

---

## What's New

The **Help → What's New** page (`/admin/whats-new`; superadmins have it in their own sidebar at `/superadmin/whats-new`) is an in-app product changelog. It lists every shipped feature, improvement, and fix grouped by month, newest first, drawn from the same release history as the public changelog on the marketing site. Filter chips (All / New / Improved / Fixed) narrow the timeline to a single category, and each chip shows how many entries it covers. It is read-only reference — no configuration lives here.

## Need Help?

Contact your system administrator for:
- Account access issues
- Configuration changes
- Technical support
- Custom reporting needs

<!-- release: marketing-send-billing 2026-07-07 (messaging prod deploy) -->
