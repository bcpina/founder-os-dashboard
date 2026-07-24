# Authority Studio — Cowork Workflow Playbook
Last updated: 24 Jul 2026
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
**Update 22 Jul 2026:** Removed nstrndstneshan@gmail.com from the "Portrait completers" Resend segment after Bruno sent that recipient a manual, out-of-band email directly (not via the automated Broadcast). All discount codes still show 0 redemptions.
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
**Update 20 Jul 2026:** 7-day pull (Jul 13-20) confirms the tracking gap is still present and now looks worse in isolation: Portrait went page_view (206) → upload_started (50, 24.3%) → preview_generated (20, 40.0%) → checkout_started (0) — zero checkout events despite 20 previews generated. Presence is effectively flat: page_view (27) → analysis_started (0) → results_viewed (1) → checkout_started (0). Both checkout_started and purchase_completed events read zero across both funnels this week, which matches this week's real $0 revenue/0 purchases in Supabase, but the complete absence of checkout events even for users who reached preview strongly suggests the GA4 checkout event simply isn't firing, not that nobody attempted to buy. See task T1-F below.
**Update 22 Jul 2026:** Third consecutive weekly pull (Jul 15-21) with the identical pattern: Portrait page_view (184) → upload_started (35) → preview_generated (10) → checkout_started (0) → purchase_completed (0). Presence: page_view (24) → analysis_started (0) → results_viewed (1) → checkout_started (0) → purchase_completed (0). Zero checkout/purchase events three weeks running despite real users reaching preview each week — this is now a well-established tracking gap, not noise. Still waiting on Minh (T1-F/T2-G).
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
### T1-F: Investigate GA4 Checkout Event Tracking Gap
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
**Update 20 Jul 2026:** This week's health check re-ran the all-time revenue query against the Supabase project queried for weekly health checks (xqfrfbfukebhutrdxglv, "AI Portrait Generator App") and got $508.10 in "real" purchases, while LemonSqueezy's own dashboard shows only $1.10 all-time (3 lifetime orders — see T3-C). Checked LemonSqueezy's webhook configuration (Settings → Webhooks) and found it points to a **different** Supabase project (pfylwkwldztmrzzqkcvg) than the one being queried every week for this health check. This means the "purchases" table read for the weekly numbers may not be the one actually receiving live transaction data from LemonSqueezy — the $508.10 figure is very likely the same T3-C phantom/bypass rows (or similar), not real revenue, and the health check has potentially been reading from the wrong database this whole time. **Priority: confirm with Minh which Supabase project the production app.authoritystudio.app backend actually writes purchases to, and repoint either the webhook or the weekly query so they match.** See T2-F.
### T2-B: Fix VectorFI Google Ads Campaign
Status: ✅ Completed 23 Jul 2026 — removal verified by reloading the list after the change, not just clicking save
What: Campaign named "Search - USA" instead of "VectorFI - Search - Coast FIRE"
Issues: blocking negative keyword [fire number calculator] needs removal
Actions: rename campaign, remove blocking negative, confirm serving
Expected outcome: VectorFI ads start delivering correctly
Update 20 Jul 2026 (health check): Campaign name found as "VectorFI- Coast Fire" — a third distinct name, matching neither "Search - USA" nor the target "VectorFI - Search - Coast FIRE". The [fire number calculator] exact-match negative keyword was still active (confirmed on page 2 of a 28-item negative keyword list), not yet removed. Campaign status that week: Eligible (Limited by budget), 32 clicks, 615 impressions, 5.20% CTR, €1.47 avg CPC, €46.93 spend (bills in EUR, unlike the two Authority Studio campaigns which bill in USD — flagged so ad-spend totals aren't silently mixed across currencies).
Resolution 20 Jul 2026: [fire number calculator] negative keyword removed by Bruno directly in Google Ads. Campaign now serving correctly. Renamed to VectorFI - Coast Fire.
**Update 22 Jul 2026:** This week's health check re-checked the campaign and found it still not matching the intended "VectorFI - Search - Coast FIRE" name, and the negative-keyword list still contains entries blocking relevant traffic ([fire calculator] / [free fire calculator]). Campaign status this week: Eligible (Limited by budget), 48 clicks, 4.61% CTR, €70.31 spend. Either the 20 Jul fix didn't persist, was reverted, or a fresh set of conflicting negatives was added since — needs a direct check with Bruno/Minh on what changed. Reopening until confirmed stable across two consecutive weeks.
**Resolution 23 Jul 2026:** Re-fixed with reload-based verification after every step, to close out why the 20 Jul fix didn't hold. (1) Renamed campaign to "VectorFI - Search - Coast FIRE" — first save attempt appeared to revert on same-tab reload (replicating the original mystery), traced to an incomplete keystroke sequence; redone carefully with a triple-click select + verified typed text, confirmed via Google Ads' own network requests (CampaignService.Mutate → 200, followed by CampaignService.List refetch → 200) AND via an independent freshly-opened tab loading the same URL, which showed the new name persisting. (2) On the negative keyword list (27 entries), found [fire calculator] and [free fire calculator] present and removed both; [fire number calculator] — the third term originally flagged — was not present in the list at all (confirmed by reviewing every row alphabetically), so nothing to remove there. (3) Verified the removal in a brand-new tab (not the same tab used to make the change) navigated fresh to the negative keywords page: list now shows 25 entries (down from 27), with the list running cleanly from "free" straight to "house fire" — no trace of the removed terms — screenshot captured as verification evidence. (4) Campaign status confirmed "Enabled" / "Limited by budget" in the same fresh tab — a budget-pacing status, not a policy or negative-keyword blocking issue, so no separate serving problem. This closes out the discrepancy: the 20 Jul "fix" likely didn't fully save due to an incomplete UI interaction, not a mysterious reversion — the fix this week was independently confirmed to have actually persisted server-side before being marked done.
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
### T2-E: Presence conflicting negative keyword removed
Status: ✅ Completed 20 Jul 2026 — re-confirmed still holding 23 Jul 2026
What: Google flagged 'linkedin profile for job search' as conflicting negative blocking Presence ads
Fix: Removed by Bruno directly in Google Ads — Presence campaign exiting Limited status
**Update 23 Jul 2026:** Re-checked as part of the VectorFI verification pass. Read all 58 negative keywords currently on the Presence campaign (2 pages) — 'linkedin profile for job search' is genuinely absent, confirming the 20 Jul removal held. Scanned the rest of the list for anything else that might block legitimate Presence traffic: the remaining entries are the intentional navigational-negatives batch from an earlier fix (country/city names, "linkedin login/sign in", "resume", "cv", "template", etc., aimed at filtering job-seekers away from this LinkedIn-optimization product) — nothing looks newly or erroneously added. No action needed.
### T2-F: LemonSqueezy webhook wrong Supabase project
Status: ⏳ Waiting for Minh — flagged 20 Jul 2026
What: Webhook points to pfylwkwldztmrzzqkcvg but health check queries xqfrfbfukebhutrdxglv — root cause of $508 vs $1.10 revenue discrepancy
Expected outcome: Webhook repointed to correct project, revenue data trustworthy
### T2-G: GA4 checkout tracking fix
Status: ⏳ Waiting for Minh — flagged 20 Jul 2026, now 3 weeks running
What: Zero checkout_started and purchase_completed events for three consecutive weeks despite real funnel traffic. Events not firing on checkout click.
Expected outcome: Checkout events wired correctly, Google Ads optimising toward real conversion signals
**Update 22 Jul 2026:** Corroborating signal found this week — all 4 conversion actions in the Authority Studio Google Ads account (Purchase, Add to cart, Begin checkout, Sign-up) show "Needs attention" status in Goals → Summary. Consistent with the checkout/purchase events not firing client-side. Still unresolved, still needs Minh.
### T2-H: Google Ads conversion actions importing stale GA4 event names
Status: ⏳ Waiting for Minh — flagged 20 Jul 2026
What: Checked ads.google.com → Tools → Conversions (Authority Studio account, vectorfiapp@gmail.com) to confirm exactly which GA4 events the primary conversion goals import. Two distinct, separate problems found:
1. The primary "Begin checkout" conversion action (Account-default goal, Group 2) imports the event **checkout_started** — an old generic alias, not the current per-product events. This is a different, separate conversion action from portrait_checkout_started / presence_checkout_started, which also exist in the account (under the "Add to cart" bucket) and are also sitting at 0 — consistent with T2-G, the app-side event isn't firing.
2. The primary "Sign-ups" conversion action similarly imports the old generic event **preview_generated** instead of the current portrait_preview_generated.
Both stale conversion actions need repointing to the current per-product event names, on top of the T2-G fix to make the per-product events fire in the first place — repointing alone will not fix this if the underlying events still aren't firing, and vice versa.
Expected outcome: "Begin checkout" and "Sign-ups" goals repointed to portrait_checkout_started/presence_checkout_started and portrait_preview_generated respectively, so Google Ads bidding optimises against real, current conversion signals
### T2-I: All-time signup count keeps declining week over week
Status: NEW — flagged 22 Jul 2026
What: The fresh Supabase all-time signup count keeps coming in lower each week it's queried: 62 (13 Jul), 59 (16 Jul), 53 (20 Jul), 47 (22 Jul). All-time counts should never decrease. Most likely cause is an inconsistent query/filter (e.g. the test-email exclusion regex or a date-window bug) rather than real user deletion, but this hasn't been confirmed.
Expected outcome: Pin down why the query result keeps shrinking and lock in one authoritative, reproducible query for "all-time signups" so the figure can be trusted week to week
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
### T3-E: Watermark strength audit
Status: ✅ Completed 23 Jul 2026
What: Full watermark strength audit on Authority Portrait preview images — fresh test session (8 test photos, Studio style, Male), visual assessment, screenshot test, right-click/save test, DevTools direct-URL test, mobile simulation, competitive comparison against Aragon.ai and Secta.ai, and a Network-tab technical read of the image-serving endpoint.
Summary: The preview watermark is a dense diagonal tiled pattern of "AUTHORITY STUDIO" / "authoritystudio.app" text repeated edge-to-edge across the frame (crossing directly over the face and eyes), plus a solid bottom info bar ("PREVIEW · date · time · authoritystudio.app" with a lock icon). Images are served at a low 400×533px resolution through an authenticated API endpoint (`/api/image?id=...&token=...`) gated by a user-scoped Supabase JWT — accessing the URL directly in a new tab returns the identical watermarked file, confirming there is no separate clean/HD endpoint exposed and that right-click-save or screenshotting both capture the same watermarked raster (the watermark is baked into the image pixels, not a removable CSS overlay, so it scales proportionally and stays visible at any display size, including mobile). Strength rating: 7/10 — dense and unmissable enough that a screenshot clearly cannot pass as a real LinkedIn profile photo, but the text is semi-transparent enough that the underlying face remains fully recognizable, which is the one real weakness: because the tiling is a perfectly uniform, static repeating pattern, it is exactly the kind of watermark that off-the-shelf AI inpainting/watermark-removal tools are good at learning and stripping out. Competitively, this is already stronger than what's visible from Aragon.ai (light corner badge only in marketing examples, gates account creation before any preview) or Secta.ai (no free preview at all — payment required upfront). Verdict: strong enough to motivate payment over screenshotting today. Recommendation: keep the low preview resolution as a secondary deterrent, and harden the watermark itself by randomizing the tiling (angle/offset/spacing/slight per-instance distortion) per generated image instead of one fixed repeating grid, since a randomized pattern is significantly harder for AI inpainting tools to remove than a uniform one; a moderate opacity increase would add further margin without hurting the "does this look professional" first impression.
### T3-F: Anonymous auth implementation
Status: ⏳ Waiting for Minh scoping — flagged 20 Jul 2026
What: Remove account gate before portrait preview using Supabase anonymous auth. Users see watermarked previews as guest, account creation only required at Unlock HD click.
Expected outcome: 43+ additional users reaching preview per 2 weeks — highest conversion impact change available
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
### Unexplained Automated Weekly Report Email
Status: NEW — needs confirmation with Minh
What: An email titled "Authority Studio — Weekly Report (Jul 13 – Jul 20)" was found in Resend, delivered to anhminh10a2hoa@gmail.com + 2 others, sent automatically ~7 hours before this health check ran. Origin and purpose unclear — may be a Minh-built automation (possibly related to the "Minh API Endpoint Brief" one-off above) or a monitoring artifact. Flagging so it isn't mistaken for noise; confirm with Minh what it is and whether it should stay.
---

## COST TRACKING
### C-1: All-Time Cost Audit — Authority Studio + VectorFI
Status: ✅ Completed 24 Jul 2026
What: Full all-time spend pull across Google Ads (both accounts), Supabase, Vercel, Gemini/Google Cloud, LemonSqueezy, Resend, and domains, to establish a true net-position baseline against the confirmed $1.10 all-time revenue (see T3-C).
Period covered: 03 Mar 2026 (earliest platform start, Supabase) to 24 Jul 2026

**ALL-TIME COSTS SUMMARY — AUTHORITY STUDIO + VECTORFI**

FIXED/RECURRING COSTS:
- Supabase (Pro plan, org-level "Vector Lab" — covers both Authority Studio + VectorFI projects combined, not separable): $94.05 (03 Mar-24 Jul 2026, 6 invoices)
- Vercel: $0.00 (Hobby plan, no payment method on file. Only "bruno-pinas-projects" team is accessible under bcpina@gmail.com this session; a previously-known second project "authority-studio-launch" under team "brunos-projects-d97a4883" returned 404 this session and could not be checked — flag as a possible gap if that project still exists under a different login)
- Resend: $0.00 (confirmed live — Transactional 3,000 emails/mo free + Marketing 1,000 contacts/mo free, no paid plan)
- Domains: not verified this session. The Cloudflare session was logged out and no credentials were entered, per policy. Typical combined renewal cost for one .app + one .com domain is commonly $25-35/yr, but this is a general estimate, not a figure pulled from your account.

VARIABLE/USAGE COSTS:
- Google Ads, Authority Studio (USD, Portrait + Presence combined, vectorfiapp@gmail.com): $1,341.19 (May 2026-24 Jul 2026)
- Google Ads, VectorFI (EUR, bcpina@gmail.com): €229.80 (Apr 2026-24 Jul 2026, net of two small refund/credit months)
- Gemini API (Google Cloud billing account 016E33-441279-ACA3C3): €112.33 gross usage, -€104.40 in savings/free-tier credits, €7.93 net billed (Jan-24 Jul 2026 YTD; all charges flagged "New" vs €0 in the prior comparison period, so this is effectively the all-time figure). Google's own breakdown attributes the full €7.93 to "Project VectorFI" in Google Cloud, with the largest single SKU being Gemini image output — consistent with portrait generation. Could not further separate Authority Studio vs. VectorFI usage within this GCP project without per-app labeling, which doesn't currently exist in the account — flagged for Minh.
- LemonSqueezy fees: $0 (not independently re-verified this session — the LemonSqueezy session was logged out and, per policy, no login credentials were entered. LemonSqueezy's pricing is transaction-based only with no monthly platform fee, and per T3-C only $1.10 in real revenue has ever been processed across 3 lifetime orders, 2 of which were $0.00 fully-discounted — so transaction fees to date are negligible, treated as $0 here.)

TOTAL SPEND TO DATE:
- USD-denominated: $1,435.24 ($1,341.19 Google Ads Authority Studio + $94.05 Supabase + $0 Vercel + $0 Resend + $0 LemonSqueezy)
- EUR-denominated: €237.73 (€229.80 Google Ads VectorFI + €7.93 Gemini API net)
- Blended total (EUR to USD at 1 EUR = 1.1391 USD, 24 Jul 2026 rate): $1,706.04

TOTAL REVENUE TO DATE: $1.10 (confirmed real, per T3-C — 3 lifetime LemonSqueezy orders, only 1 non-zero)

NET POSITION: -$1,704.94

RUN-RATE PROJECTION (based on each platform's observed average monthly pace since its own start date — not a forecast of future budget decisions):
- Google Ads Authority Studio: ~$479/mo (dominant driver, ~84% of monthly burn)
- Google Ads VectorFI: ~€60/mo (~$68/mo)
- Supabase: ~$20/mo (list price is $25/mo Pro; observed average is lower due to a partial first month)
- Gemini API: ~€3/mo (rough estimate — usage is ramping with portrait volume, so this will likely rise, not stay flat)
- Vercel / Resend / LemonSqueezy: $0/mo at current usage
- Domains: unverified, likely small (~$2-3/mo equivalent)

Blended monthly run-rate: ~$570/month. If nothing changes:
- +30 days: ~$2,276 total spend
- +60 days: ~$2,846 total spend
- +90 days: ~$3,416 total spend

Ad spend is ~96% of the monthly run-rate and is the one lever directly under Bruno's control — cutting or pausing Google Ads has an outsized, near-immediate effect on this trajectory.

**Data-quality notes / open items:**
1. Supabase billing is org-level only ("Vector Lab" org) and cannot be split between Authority Studio and VectorFI projects from the billing UI.
2. Vercel's $0 figure only covers the one team/account reachable under bcpina@gmail.com this session; a previously-referenced separate project could not be checked (404, different login required).
3. LemonSqueezy and Cloudflare/domain figures could not be independently re-verified this session because both sessions were logged out and re-authenticating was out of scope (no credentials entered, per policy) — both are estimated from prior audits/public pricing, not freshly pulled this session.
4. Gemini API spend is attributed by Google Cloud to a project literally named "VectorFI," which may or may not cleanly map to "VectorFI the product" vs. "Authority Studio backend hosted in that GCP project" — flagged for Minh to confirm which app(s) that GCP project actually serves.
---

## SYSTEM REFERENCE
### Product URLs
- Authority Portrait: https://authoritystudio.app
- Authority Presence: [https://authoritystudio.app/presence](https://authoritystudio.app/presence﻿-)
[-](https://authoritystudio.app/presence﻿-) VectorFI: https://myvectorfi.com
- Founder OS Dashboard: [https://bcpina.github.io/founder-os-dashboard](https://bcpina.github.io/founder-os-dashboard﻿-)
[-](https://bcpina.github.io/founder-os-dashboard﻿-) GitHub Dashboard Repo: https://github.com/bcpina/founder-os-dashboard
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
### Current Campaign Status (as of 23 Jul 2026)
- Portrait: Active, 5.90% CTR, $184.09 last week, 276 clicks, $2.00 CPC cap confirmed
- Presence: Active, 4.61% CTR, $54.09 last week, 63 clicks — negative keyword list re-verified clean (see T2-E)
- VectorFI: Enabled / Limited by budget, 4.61% CTR, €70.31 last week, 48 clicks — renamed to "VectorFI - Search - Coast FIRE" and blocking negatives removed, both verified via independent fresh-tab reload (see T2-B, completed)
---
