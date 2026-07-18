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
Status: READY TO RUN — waiting for Minh template files
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
### T1-B: Pull GA4 Funnel Data
Status: READY TO RUN
What: Pull complete funnel breakdown missed during Monday outage
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
---
## TIER 3 — USEFUL (run when time allows)
### T3-A: Competitor Paywall Benchmark
Status: READY TO RUN
What: Visit Aragon AI, Secta Labs, HeadshotPro
Evaluate: paywall design, pricing, social proof, urgency mechanics, CTA copy
Compare against Authority Portrait paywall
Expected outcome: Specific paywall improvement ideas informed by what competitors do
### T3-B: Portrait Watermark Mobile Check
Status: READY TO RUN
What: 83% of traffic is mobile — check if watermarked preview is usable/screenshottable on mobile
Resize browser to mobile width, attempt to screenshot/save watermarked preview
Expected outcome: Confirms watermark is painful enough on mobile to motivate payment
### T3-C: LemonSqueezy $39 Order Investigation
Status: READY TO RUN
What: Weekly report flagged ~$40 in LemonSqueezy ($1.10 + $39)
The $39 matches Presence price exactly — could be first real customer
Check LemonSqueezy orders → confirm product, date, customer, payment status
Cross-reference Supabase purchases table
Expected outcome: Confirms or rules out first real Presence sale
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
