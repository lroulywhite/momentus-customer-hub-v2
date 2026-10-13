# Momentus Customer Hub — Executive Summary

## TL;DR
- One login replaces five separate ones for Learning, Support, Community, Rewards, and Events.
- It proactively recommends what each customer should try next, driving adoption and retention.
- It's mostly connecting existing systems, so launch takes about 6–9 months.

## 1. Why We Need This

**Problem today**
- Five logins, five mental models across Learning, Support, Community, Rewards, and Events.
- No single place to see what's new (releases, workflow changes, webinars).
- Adoption gaps stay invisible to customers until a renewal conversation.
- Engagement is reactive: customers show up when something breaks.

**What the Hub changes**
- **One login, one home base** for the whole Momentus relationship.
- **Adoption agent:** recommendations tied to a real signal, each explaining why it surfaced.
- **A reason to return:** announcements plus opt-in Instant / Weekly / Monthly email digests.
- **One search** across courses, KB articles, discussions, missions, rewards, and events.

**Why it matters to us**
- Adoption drives retention and expansion.
- Self-service reduces support load.
- Five vendor UIs feel like one Momentus.
- A new channel for product announcements, betas, and campaigns.

## 2. Go-Live Roadmap (~6–9 months)

| Phase | Scope | Duration |
|---|---|---|
| 0 | Decisions: identity provider, vendor SSO/SCIM tiers, search build-vs-partner, permission-mapping owner | 2–3 wks |
| 1 | Identity foundation: hub as SSO client (SAML/OIDC), SCIM where supported | 4–6 wks |
| 2 | Native content integration per page (Learning, Support, Community, Events, Recommendations) | 8–12 wks, parallel |
| 3 | Universal search, starting federated | 8–16+ wks, parallel |
| 4 | Notification preferences and digest emails | 4–6 wks, parallel |
| 5 | Pilot, then staged rollout | 4–6 wks |

| Page | Source system | Change from prototype |
|---|---|---|
| Learning | 360Learning | Real course/progress data via API |
| Support | 4 Ungerboeck/Zendesk centers | Real KB articles and ticket status |
| Community | Zealot | Live groups, discussions, missions, rewards |
| Events | Events/webinar calendar | Real feed; RSVP writes back |
| Featured | New: adoption agent | Needs a real usage-data pipeline |

## 3. Possible Blockers and How We Navigate Them

| Blocker | How we navigate |
|---|---|
| Vendor SSO/SCIM gated behind higher contract tiers | Confirm with each vendor's account team in Phase 0, before engineering commitment |
| Partner platforms (360Learning, Zendesk, Zealot) block iframe embedding | Accept as permanent; link out in context with matching styling |
| Unified search is hard across five permissioned systems | Start federated; revisit a central index or buy/partner (Glean, Coveo) once usage data justifies it |
| Role/permission mapping differs per system | Assign one owner in Phase 0 to define IdP-group-to-role mapping |
| Adoption agent needs usage data not centralized today | Start with a small set of high-confidence signals; expand after launch |
| Deprovisioning gaps on offboarding | Test end-to-end in production during the pilot |

## Ask
Confirm vendor SSO/API tiers and make the search build-vs-partner call early. Those two decisions set the real timeline.
