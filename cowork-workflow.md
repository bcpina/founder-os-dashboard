# Authority Studio — Cowork Workflow Playbook
Last updated: 20 Jul 2026
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
**Update 20 Jul 2026:** Weekly health check re-checked Broadcasts in Resend and found Group 1 (Portrait completers) now shows 1 bounce (8/9 delivered, 11.1%) — a transient bounce that registered after the original 0-bounce verification. Redemptions since send: PORTRAIT20, PRESENCE20, WELCOME20, RETRY20 all still at 0 — no conversions from the campaign yet. Open/click rates remain unmeasurable because tracking is not enabled on the authoritystudio.app sending domain (see T2-C-adjacent note below).
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
**Update 20 Jul 2026:** 7-day pull (Jul 13-20) confirms the tracking gap is still present and now looks worse in isolation: Portrait went page_view (206) → upload_started (50, 24.3%) → preview_generated (20, 40.0%) → checkout_started (0) — zero checkout events despite 20 previews generated. Presence is effectively flat: page_view (27) → analysis_started (0) → results_viewed (1) → checkout_started (0). Both checkout_started and purchase_completed events read zero across both funnels this week, which matches this week's real $0 revenue/0 purchases in Supabase, but the complete absence of checkout events even for users who reached preview strongly suggests the GA4 checkout event simply isn't firing, not that nobody attempted to buy. See new task T1-F below.
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
### T1-F: Investigate GA4 Checkout Event Tracking Gap (NEW — flagged 20 Jul 2026)
Status: NEW — needs scoping with Minh
What: Both Portrait and Presence funnels show zero checkout_started and purchase_completed events in GA4 across two consecutive weekly pulls (14-day and 7-day), even in weeks where users reached preview_generated (Portrait, 20 this week) or where real purchases exist elsewhere in the stack. This points to the checkout event simply not firing client-side, rather than zero checkout attempts.
Ask Minh to: confirm the GA4 event trigger for checkout_started/portrait_checkout_started and presence_checkout_started is actually wired into the checkout button/flow, and test end-to-end with GA4 DebugView
Expected outcome: Restores accurate funnel visibility into the paywall step — currently the dashboard cannot tell the difference between "nobody is trying to buy" and "the tracking is broken"
---
## TIER 2 — HIGH VALUE (run after Tier 1)
### T2-A: Supabase Revenue Data Quality Audit
Status: IN PROGRESS — root cause identified 20 Jul 2026
What: Investigate $508 Supabase vs $40 LemonSqueezy revenue discrepancy
Query purchases table for all rows, identify which are sandbox/manual/fake
Give Minh precise list to clean permanently
Expected outcome: Clean revenue data, accurate dashboard figures
**Update 20 Jul 2026:** This week's health check re-ran the all-time revenue query against the Supabase project queried for weekly health checks (xqfrfbfukebhutrdxglv, "AI Portrait Generator App") and got $508.10 in "real" purchases, while LemonSqueezy's own dashboard shows only $1.10 all-time (3 lifetime orders — see T3-C). Checked LemonSqueezy's webhook configuration (Settings → Webhooks) and found it points to a **different** Supabase project (pfylwkwldztmrzzqkcvg) than the one being queried every week for this health check. This means the "purchases" table read for the weekly numbers may not be the one actually receiving live transaction data from LemonSqueezy — the $508.10 figure is very likely the same T3-C phantom/bypass rows (or similar), not real revenue, and the health check has potentially been reading from the wrong database this whole time. **Priority: confirm with Minh which Supabase project the production app.authoritystudio.app backend actually writes purchases to, and repoint either the webhook or the weekly query so they match.**
### T2-B: Fix VectorFI Google Ads Campaign
Status: READY TO RUN
What: Campaign named "Search - USA" instead of "VectorFI - Search - Coast FIRE"
Issues: blocking negative keyword [fire number calculator] needs removal
Actions: rename campaign, remove blocking negative, confirm serving
Expected outcome: VectorFI ads start delivering correctly
**Update 20 Jul 2026:** Re-checked under bcpina@gmail.com. Campaign name is actually **"VectorFI- Coast Fire"** — a third distinct name, matching neither "Search - USA" nor the target "VectorFI - Search - Coast FIRE". The **[fire number calculator]** exact-match negative keyword is still active (confirmed on page 2 of a 28-item negative keyword list) — it has not been removed despite the earlier instruction to do so. Campaign status this week: Eligible (Limited by budget), 32 clicks, 615 impressions, 5.20% CTR, €1.47 avg CPC, €46.93 spend (note: this campaign bills in EUR, unlike the two Authority Studio campaigns which bill in USD — flagged so ad-spend totals aren't silently mixed across currencies). Still needs: campaign rename and negative keyword removal.
### T2-C: Resend Failure Email Audit
Status: READY TO RUN
What: Verify Minh's Part A automatic failure email is firing correctly
Check: any new failed jobs in Supabase → cross-reference Resend logs → confirm email sent
Also verify Joshua received reply to personal apology email
Expected outcome: Confirms automation working or flags gap to Minh
**Update 20 Jul 2026:** Partial evidence found opportunistically this week — 3 "Something went wrong with your portraits" emails were delivered this week, all to internal monitoring addresses (anhminh10a2hoa+postdeploy, +sweepguard, +stucktest), suggesting Minh has synthetic health-check probes wired into the failure-email pipeline and that it is firing correctly for those tests. No real user-facing failure emails were triggered this week (consistent with 0 stuck jobs and 0 bypass rows found in Supabase). Whether Joshua replied to the personal apology email is still not verifiable — Resend has no inbound receiving domain configured on authoritystudio.app, so reply tracking is unavailable via Resend for any of the above. Full manual verification with Minh still outstanding.
### T2-D: Portrait Upload Drop-off Root Cause
Status: Completed 18 Jul 2026
What: 32 of 52 uploaders never reached preview in last 14 days
Minh investigating: jobs between upload and completion in Supabase
Expected outcome: Determines if drop-off is abandonment or generation failure
Summary: Ran 7 Supabase queries on generation_jobs for the 14-day window. Only 9 job rows were created by 8 distinct uploaders (7 completed, 2 failed) — versus 52 portrait_upload_started events in GA4 for the same period, meaning roughly 43 people who started uploading never triggered a backend job at all. This gap, not backend reliability, explains most of the "32 never reached preview" figure. The 2 backend failures that did occur both passed quality check cleanly (8/8 photos) with no error_message logged, and neither user retried; one job also sat unresolved for 7.1 days before being marked failed, pointing to no active stuck-job monitoring. Verdict: primarily a client-side/mobile upload UX issue (consistent with 83.5% mobile traffic), with a secondary backend reliability gap worth a quick flag to Minh (silent failures, no retry/cleanup logic).
### T2-E: Remove Conflicting Negative Keywords — Presence Campaign (NEW — flagged 20 Jul 2026)
Status: NEW — READY TO RUN
What: Google Ads Recommendations page (Authority Studio - Search - Presence, under vectorfiapp@gmail.com) flags "Remove conflicting negative keywords" — the negatives `linkedin profile for job search` / `[linkedin profile for job search]` are blocking some Presence ads from serving against relevant search terms
Action: review and remove/adjust the conflicting negative keywords in the Presence campaign
Expected outcome: Removes a self-inflicted limiter on an already budget-limited campaign (currently "Eligible (Limited)" status this week, partly due to "missing enough relevant keywords")
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
**Update 20 Jul 2026:** One new LemonSqueezy order this week — VectorFI Core, $0.00 (100% discount), 15 Jul 2026. Still 0 paying orders in the store all-time; all-time real revenue remains $1.10. See T2-A for the deeper webhook/project-mismatch finding this uncovered.
### T3-D: Dashboard Health Check
Status: Completed 20 Jul 2026
What: Confirm v2 dashboard loading correctly at bcpina.github.io/founder-os-dashboard
Check: all 6 tabs loading, computed alerts rendering, sparklines showing, DATA values current
Expected outcome: Confirms dashboard working correctly for Monday report
Summary: DATA.week and DATA.systems updated with this week's figures and committed to main (commit 2091ae9, "Weekly health check — 20 Jul 2026"). Code-level verification only this session (structure and values confirmed correct in the committed file); did not separately re-render and click through all 6 live tabs on the published GitHub Pages URL — recommend a quick visual spot-check next session if time allows.
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
### Unexplained Automated Weekly Report Email (NEW — flagged 20 Jul 2026)
Status: NEW — needs confirmation with Minh
What: An email titled "Authority Studio — Weekly Report (Jul 13 – Jul 20)" was found in Resend, delivered to anhminh10a2hoa@gmail.com + 2 others, sent automatically ~7 hours before this health check ran. Origin and purpose unclear — may be a Minh-built automation (possibly related to the "Minh API Endpoint Brief" one-off above) or a monitoring artifact. Flagging so it isn't mistaken for noise; confirm with Minh what it is and whether it should stay.
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
### Current Campaign Status (as of 20 Jul 2026)
- Portrait campaign (vectorfiapp@gmail.com): Eligible (Learning), $2.00 max CPC cap confirmed still active, 300 clicks, 4,768 impressions, 6.29% CTR, $222.43 spent this week — conversions down sharply week over week per Google's own recommendation panel
- Presence campaign (vectorfiapp@gmail.com): Eligible (Limited — missing relevant keywords), 73 clicks, 1,403 impressions, 5.20% CTR, $65.23 spent this week — conflicting negative keywords blocking some ads, see new T2-E
- VectorFI campaign (bcpina@gmail.com): actual name "VectorFI- Coast Fire" (still mismatched, see T2-B), Eligible (Limited by budget), 32 clicks, 615 impressions, 5.20% CTR, €46.93 spent this week (EUR, not USD) — [fire number calculator] negative keyword still blocking relevant traffic, not yet removed
---
