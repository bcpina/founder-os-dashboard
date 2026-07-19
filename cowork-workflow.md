# Authority Studio — Cowork Workflow Playbook
Last updated: 18 Jul 2026
This document maps all recurring and one-off Cowork tasks across Authority Studio. Run these in order of priority. Each task has a prompt saved in the Monday health check notes or can be reconstructed from this document.
---
## MONDAY MORNING — WEEKLY HEALTH CHECK
Run every Monday. Takes 20-25 minutes.
Prompt: saved in Notes app as "AS Monday Health Check Prompt"
Covers:
- Supabase: signups, portrait jobs, presence reports, purchases, bypass rows, stuck jobs
- LemonSqueezy: orders, revenue, product status
- Resend: email delivery, duplicates, portraits ready, presence ready
- Google Ads: all 3 campaigns — clicks, CTR, spend, status
- GA4: full funnel data for Portrait and Presence
- Update Founder OS dashboard DATA object
- Commit index.html to github.com/bcpina/founder-os-dashboard
- Generate plain language weekly report → paste into Claude chat
---
## TIER 1 — HIGH IMPACT (run when credits available)
### T1-A: Send Re-engagement Email Campaign
Status: Completed 19 Jul 2026
What: Send 4 email groups via Resend Broadcasts
Groups:
- Group 1: 10 Portrait completers → PORTRAIT20 discount
- Group 2: 8 Presence viewers → PRESENCE20 discount
- Group 3: 36 cold signups → WELCOME20 discount
- Group 4: 3 failed generation users → RETRY20 discount
Send order: Group 4 first → Group 1 → Group 2 → Group 3
Wait 30 minutes between groups
Discount codes: all created and Active in LemonSqueezy CareerVector store
Prerequisite: Minh to confirm branded templates ready + send files
Expected outcome: First revenue possible within 24-48 hours of sending
Summary: Re-engagement campaign sent 19 Jul 2026 — Group 4 (1 of 3 sent, only Joshua Redman found in Resend; other 2 failed-generation users from Minh's list could not be located), Group 1 (9 of 9 Portrait completers), Group 2 (7 of 8 Presence viewers, 8th recipient not found), Group 3 (18 of 18 cold signups). Total 35 of 38 targeted delivered, 0 bounces, 0 failures. Discount codes: RETRY20, PORTRAIT20, PRESENCE20, WELCOME20 — all expire 31 Jul 2026.
### T1-B: Pull GA4 Funnel Data
Status: Completed 18 Jul 2026
Summary: 14-day pull (Jul 4-17) shows healthy top-of-funnel (220 Portrait page views, 55 Presence page views, 83.5% mobile) but a severe mid-to-bottom funnel tracking gap — Portrait converts page_view to purchase at just 0.9% with only 1 checkout_started event logged against 2 purchases, and Presence shows 0 presence_purchase_completed events in the period, pointing to broken/missing checkout and purchase event tracking as the real paywall blocker, not traffic quality.
- Portrait: 52 uploads → only 20 previews (61% drop before paywall)
- Portrait: 2 purchase events with 1 checkout — tracking bug flagged to Minh
- Presence: analysis rate only 1.8% — very low top of funnel engagement
- 83.5% mobile traffic — mobile upload experience flagged for investigation
Pull:
- Portrait funnel: page_view → upload_started → preview_generated → checkout_started → purchase_completed
- Presence funnel: page_view → analysis_started → results_viewed → checkout_started → purchase_completed
- Conversion rate at each step
- Mobile vs desktop split
- Top 3 countries
- Average session duration on each product page
Expected outcome: Identifies exact paywall drop-off point to inform conversion fix
### T1-C: Fix Portrait Keyword — Add Exact Match Negatives
Status: READY TO RUN
What: ai professional photo generator (QS 5, 35% of Portrait budget) is triggered by generic terms
Add as exact match negatives to Portrait campaign:
[ai image generator]
[ai photo generator]
[free ai image editor]
[free ai image generator]
Expected outcome: ~$15-20/month saved, QS improvement over time
### T1-D: Mobile Upload Experience Audit
Status: Completed 18 Jul 2026
What: 83.5% of traffic is mobile. Test the full Portrait upload flow on mobile viewport — file picker, photo upload, quality check, generation progress screen
Specifically check: does drag-and-drop work on mobile? Is the minimum 8 photos requirement clear? Does the progress screen display correctly on small screens?
Expected outcome: Identifies mobile friction causing 61% upload-to-preview drop-off
Summary: Audited the mobile upload flow for Authority Studio. Browser viewport resize did not propagate to actual page rendering in this environment (confirmed via window.innerWidth staying at desktop width), so structural findings below are verified but visual layout specifically at 390px is inferred, not confirmed - tested at desktop width per your direction. Reaching the upload step takes 1 tap from the CTA, and the 8-photo minimum is stated clearly 3 times, but a mandatory account-creation gate (Google OAuth or email/password) blocks users right at the Generate Portrait Previews click with no guest path - this directly explains the Supabase-confirmed gap where 43 of 52 uploaders never triggered a backend job. Error handling for under-8 photos is passive (Generate button just stays disabled, no live photo count or explicit message), and the landing page hero has a fade-in animation that sticks at roughly 26% opacity until the user scrolls, creating a blank-feeling first impression. Ratings (desktop-width, unconfirmed at true 390px): Landing page 3/5, Navigation to upload 4/5, Upload interface 3/5, Error handling 2/5, Progress screen 4/5. Top friction points: mandatory account creation at the generation step, no live feedback on photos still needed, and the stuck hero animation. Top fixes: allow guest generation with account creation deferred to results/checkout, add a live X of 8 photos selected counter with camera-roll guidance, and fix the hero fade-in to complete on load without requiring a scroll.
### T1-E: Brief Minh on Mobile Upload Fixes
Status: Ready to run
What: Mobile audit found 3 concrete conversion blockers - mandatory account-creation gate at the Generate click with no guest path, passive error handling for under-8-photo uploads (no live count or clear message), and a landing page hero animation that sticks at ~26% opacity until scroll
Expected outcome: Minh implements guest-mode generation (account creation deferred to results/checkout), live photo-count guidance, and fixes the stuck hero animation - directly targeting the 43/52 upload abandonment gap confirmed in the Supabase investigation
---
## TIER 2 — HIGH VALUE (run after Tier 1)
### T2-A: Supabase Revenue Data Quality Audit
Status: READY TO RUN
What: Investigate $508 Supabase vs $40 LemonSqueezy revenue discrepancy
Query purchases table for all rows, identify which are sandbox/manual/fake
Give Minh precise list to clean permanently
Expected outcome: Clean revenue data, accurate dashboard figures
### T2-B: Fix VectorFI Google Ads Campaign
Status: READY TO RUN
What: Campaign named "Search - USA" instead of "VectorFI - Search - Coast FIRE"
Issues: blocking negative keyword [fire number calculator] needs removal
Actions: rename campaign, remove blocking negative, confirm serving
Expected outcome: VectorFI ads start delivering correctly
### T2-C: Resend Failure Email Audit
Status: READY TO RUN
What: Verify Minh's Part A automatic failure email is firing correctly
Check: any new failed jobs in Supabase → cross-reference Resend logs → confirm email sent
Also verify Joshua received reply to personal apology email
Expected outcome: Confirms automation working or flags gap to Minh
### T2-D: Portrait Upload Drop-off Root Cause
Status: Completed 18 Jul 2026
What: 32 of 52 uploaders never reached preview in last 14 days
Minh investigating: jobs between upload and completion in Supabase
Expected outcome: Determines if drop-off is abandonment or generation failure
Summary: Ran 7 Supabase queries on generation_jobs for the 14-day window. Only 9 job rows were created by 8 distinct uploaders (7 completed, 2 failed) — versus 52 portrait_upload_started events in GA4 for the same period, meaning roughly 43 people who started uploading never triggered a backend job at all. This gap, not backend reliability, explains most of the "32 never reached preview" figure. The 2 backend failures that did occur both passed quality check cleanly (8/8 photos) with no error_message logged, and neither user retried; one job also sat unresolved for 7.1 days before being marked failed, pointing to no active stuck-job monitoring. Verdict: primarily a client-side/mobile upload UX issue (consistent with 83.5% mobile traffic), with a secondary backend reliability gap worth a quick flag to Minh (silent failures, no retry/cleanup logic).
---
## TIER 3 — USEFUL (run when time allows)
### T3-A: Competitor Paywall Benchmark
Status: Completed 18 Jul 2026
What: Visit Aragon AI, Secta Labs, HeadshotPro
Evaluate: paywall design, pricing, social proof, urgency mechanics, CTA copy
Compare against Authority Portrait paywall
Expected outcome: Specific paywall improvement ideas informed by what competitors do
Summary: Visited Aragon AI, Secta Labs, and HeadshotPro as a cold visitor. All three gate account creation (or payment, in Secta's case) immediately - before any photo upload or preview - which is earlier than Authority Portrait, which lets visitors upload photos and pick a style before hitting its account gate at the Generate click. Secta is the most aggressive, routing straight to a $49 Stripe checkout before any account exists. HeadshotPro states a striking "1-3 selfies" minimum as a headline differentiator versus Authority's 8-photo minimum, and offsets having no free trial with a prominent "100% money-back guarantee, no questions asked" badge. Aragon and HeadshotPro both place trust signals (ratings, guarantees, customer counts) directly on their account-creation screen, which Authority Portrait's gate currently lacks - the single most transferable fix. Pricing: Aragon $35-75, Secta $49, HeadshotPro EUR29-59, all one-time.
### T3-B: Portrait Watermark Mobile Check
Status: READY TO RUN
What: 83% of traffic is mobile — check if watermarked preview is usable/screenshottable on mobile
Resize browser to mobile width, attempt to screenshot/save watermarked preview
Expected outcome: Confirms watermark is painful enough on mobile to motivate payment
### T3-C: LemonSqueezy $39 Order Investigation
Status: Completed 18 Jul 2026
What: Weekly report flagged ~$40 in LemonSqueezy ($1.10 + $39)
The $39 matches Presence price exactly — could be first real customer
Check LemonSqueezy orders → confirm product, date, customer, payment status
Cross-reference Supabase purchases table
Expected outcome: Confirms or rules out first real Presence sale
Order finding: Confirmed via LemonSqueezy Orders (all-time, 23 Feb-19 Jul 2026) that only 3 orders have ever been placed in the CareerVector store, all Paid: $0.00 VectorFI Pro (bcpina, 100% discount), $1.10 Authority Portrait Starter (anhminh10a2hoa@gmail.com, 13 May), and $0.00 VectorFI Core (bcpina, 100% discount). No $39 order exists in LemonSqueezy, ever - real revenue to date is $1.10 total. Cross-referencing Supabase purchases (amount_cents >= 3000) found 17 rows marked status=paid that do NOT correspond to any real LemonSqueezy order (1x Portrait $39, 16x Presence $39 = $663 total), with fabricated ls_order_id values outside the real order-number range, traced to just 3 user accounts across 5 different IPs plus 4 rows with no IP logged, all dated 2-5 Jun 2026. Verdict: NOT a real customer - this is a purchase-confirmation bypass creating phantom paid rows in Supabase, not a missed first sale. Flagged as a priority follow-up for Minh (see T2-A).
### T3-D: Dashboard Health Check
Status: READY TO RUN
What: Confirm v2 dashboard loading correctly at bcpina.github.io/founder-os-dashboard
Check: all 6 tabs loading, computed alerts rendering, sparklines showing, DATA values current
Expected outcome: Confirms dashboard working correctly for Monday report
---
## ONE-OFF TASKS — PENDING
### Minh API Endpoint Brief
Status: NOT STARTED — needs scoping session with Claude first
What: Brief Minh to build /api/health-stats endpoint
Covers: Supabase + LemonSqueezy data in one JSON call
Dashboard fetches live data on load — no weekly manual update needed
Estimated Minh hours: 5-6
### Email Campaign Tab — Dashboard
Status: NOT STARTED
What: Add 5th product tab to Founder OS dashboard for email campaigns
Shows: campaigns sent, delivery rates, discount code redemptions, revenue attributed
Prerequisite: first email campaign sent and data available
---
## SYSTEM REFERENCE
### Product URLs
- Authority Portrait: https://authoritystudio.app
- Authority Presence: [https://authoritystudio.app/presence](https://authoritystudio.app/presence￼-)
[-](https://authoritystudio.app/presence￼-) VectorFI: https://myvectorfi.com
- Founder OS Dashboard: [https://bcpina.github.io/founder-os-dashboard](https://bcpina.github.io/founder-os-dashboard￼-)
[-](https://bcpina.github.io/founder-os-dashboard￼-) GitHub Dashboard Repo: https://github.com/bcpina/founder-os-dashboard
### Discount Codes (LemonSqueezy — CareerVector store)
- PORTRAIT20: 20% off Portrait tiers, expires 31 Jul 2026
- PRESENCE20: 20% off Presence, expires 31 Jul 2026
- WELCOME20: 20% off all products, expires 31 Jul 2026
- RETRY20: 20% off Portrait + Presence, expires 31 Jul 2026
### Key Contacts
- Developer: Minh (WhatsApp) — $25/hr
- Google Ads account: bcpina@gmail.com
- Resend sending address: bruno@authoritystudio.app
- LemonSqueezy store: CareerVector
### Current Campaign Status (as of 18 Jul 2026)
- Portrait campaign: Active, $16.67/day, Maximize Clicks $2.00 cap, 57 negative keywords
- Presence campaign: Active, $10/day, Maximize Clicks, 59 negative keywords
- VectorFI campaign: Active but serving incorrectly — see T2-B
---
