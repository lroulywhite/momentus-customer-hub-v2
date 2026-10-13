# Handoff: Momentus Customer Hub Prototype

## Overview
A single-login "Customer Hub" that unifies six Momentus customer-facing destinations — Featured/Recommendations, Learning, Support, Community (+ Missions/Rewards), Events, and Account — behind one SSO gate, with a Smart Search assistant, a Notifications Center, a profile panel, and an email-digest subscription system. This is an **adoption-agent concept prototype**: the Featured page demonstrates surfacing personalized recommendations ("why this was suggested") across product areas.

## About the Design Files
The files in this bundle are **design references built as static HTML/React prototypes** (no backend, no real auth, no persistence — all data is mocked in-file). They are not production code. The task is to **recreate this experience in Momentus's real frontend stack** (whatever framework the target app already uses — React/Vue/etc.), wiring it to real SSO, real content APIs (360Learning, Zendesk/Ungerboeck support centers, Zealot community, LMS, rewards/points system, events calendar), and a real notification/digest email service. If no existing app shell exists yet, React is a reasonable default given how these prototypes are structured (component-per-page, single top-level state).

See the companion **Backend Q&A doc** (`Customer Hub - Backend Q&A.dc.html` / its printable version) included in this bundle — it discusses realistic backend/provisioning challenges (locked-down source systems, whether unified search is achievable without deeper platform partnership, SSO federation, etc.). Read that before scoping engineering work.

## Fidelity
**High-fidelity.** Visual design (colors, type, spacing, component states) is intended to be final-direction; several pages (Learning, Support, Community, Rewards, Events) were built to visually match real reference screenshots of existing Momentus product surfaces (360Learning, Zealot community/rewards, support center, events digest email) provided during design. Recreate pixel-close using the target codebase's design system/tokens where one exists.

## Pages / Views

### 1. Login Gate (pre-auth)
Full-screen modal over a dimmed gradient backdrop. Momentus logo (white, on gradient plate), headline, single button: **"Sign in with Momentus SSO"**. Clicking sets `authenticated = true` — no further login prompts anywhere afterward. This single gate is the entire point of the prototype: prove one login unlocks all five destinations.

### 2. Persistent Top Nav (post-auth)
Sticky header, gradient `linear-gradient(90deg, #4146E0 0%, #9B59B6 100%)`. Left→right:
- Momentus wordmark logo (white)
- Tabs: **Featured**, **Learning**, **Support**, **Community**, **Events** (calendar icon), **Account**. Below ~1100px wide the labels hide and tabs become icon-only. A yellow dot on Account appears only when an account-level action is needed (adoption review due within 30 days, renewal within 90 days).
- **Smart Search** (sparkle icon) — opens a right-side drawer
- **Notifications Center** (megaphone icon, yellow unread dot) — opens a right-side drawer
- **Profile** avatar button ("SK") — opens a right-side profile panel

There is no search input in the nav. Tab click sets `activeTab`.

### 3. Smart Search (replaces the nav search bar)
Sparkle button opens an "Smart Search" drawer: input + Ask button (Enter also submits), suggested questions, a short answer, and up to five clickable matching Hub items that jump to the right page. **Scope: Customer Hub content only** (courses, community, missions, rewards, events, recommendations). It does not search product or knowledge base content. In the design environment it calls `window.claude.complete` with the Hub content index and falls back to keyword matching if that call fails (the fallback is what runs in the standalone/GitHub Pages copy). Production needs a real backend call to an AI/search service and an index of Hub content the user is allowed to see.

### 4. Featured (single page)
Title **"Welcome back, {name} 👋"**, then:
- **Action items** panel (navy gradient, yellow count badge) listing **user-level** to-dos: finish a course, finish a certification, RSVP to a webinar (sample data).
- **Subscribe** button (blue) above the carousel arrows opens the Subscribe modal (section 10).
- **Recommended for you** — arrow-scrolled carousel of cards from across the site (Course, Feature, Mission, Community, Support, Reward), each with a "why this was recommended" reason (e.g. "Low adoption · Functions").
- **Announcements** — one-time push communications (Product Release, New Support Workflow, Upcoming Webinar, Event) as banner cards, filterable by type chips.

### 5. Learning Center
Native in-hub page styled to mirror the real 360Learning experience: dark hero banner (`assets/momentus-learning-center.png` real banner image), category chip row, horizontal rows of course cards with progress bars (each card: title, category accent color, duration, % progress bar, thumbnail using the Momentus star watermark on a dark/gradient background so the white logo reads). "Open in 360Learning ↗" link opens `https://learn.gomomentus.com/home/content/all` in a new tab (real product is locked behind auth/CSP — cannot be iframed).

### 6. Support Center
Navy hero: **"We have many ways to support you"** + search bar, with a faint "support center" watermark text + Momentus star behind the logo. Below: 3-column grid of bordered cards, each with a **navy filled circular icon** (white glyph) — Knowledge Base / Submit a Ticket / etc. Below that: 4 portal launcher cards, one per real product line, each linking out:
- Enterprise Support → `https://supportcenter.ungerboeck.com/hc/en-us/`
- Elite Support → `https://elitesupportcenter.ungerboeck.com/hc/en-us/`
- WeTrack Support → `https://wetracksupportcenter.ungerboeck.com/hc/en-us/`
- Momentus Connect Support → `https://connectsupportcenter.ungerboeck.com/hc/en-us/`

### 7. Community (unified hub — Community + Missions + Rewards)
Single top-nav item **"Community"**, with **six internal sub-tabs**:
1. **Quickview** — welcome banner + stat tiles (points, level, rank, missions) + "Jump back in" shortcuts.
2. **My Groups** — left-side list of joined community spaces; discussion post feed + comments on the right, styled after the real Zealot "Start Here" space (Discussion Posts / About This Space tabs, post cards with image attachments).
3. **Directory** — grid of member cards.
4. **Missions** — mission cards with real CDN background images, dark gradient overlay, "MISSION" label, Pinned/time badges, points value, type chip; search + filter row.
5. **Rewards** — reward catalog: product cards with images, "New" tag, title, description, points cost, black **Redeem** pill; search + Points/Sort filters; points balance display.
6. **My Profile** — profile header (avatar, name, role), stat tiles, activity/points history.

Popular Posts + Group Leaderboard sidebar appears on My Groups. "Open full site ↗" links out to `https://momentus.joinzealot.com/`.

### 8. Events
Top-nav tab with a **calendar icon**. Layout:
- Compact single-row toolbar: small search input, **Upcoming/Past** segmented toggle, and type filter chips (Webinar / Training / User Group / Conference / Community, etc.) — deliberately minimal vertical footprint.
- **"Up next" featured hero** banner for the very next upcoming event (branded gradient/thumbnail, kicker + name lockup, short **promo snippet** of the content, and tag pills) — shown only when no filter/search is active.
- Below: events grouped by month, each row = branded thumbnail (date chip top-left, Momentus star watermark, kicker/name lockup) + time + title + **promo snippet** (2–3 sentence description of the session) + tag pills (e.g. "Enterprise", "Release", "What's New"). **No View/RSVP buttons, no host line, no external links on rows** — content-first, per latest direction.
- Empty state message when a filter/search yields nothing.

### 9. Notifications Center Drawer (nav-level)
Megaphone button slides a panel in from the right. Header "Notifications Center", a "Notify me about" row of toggle chips per source (Featured, Learning, Community, Mission, Reward, Product) that filters the feed, and a live feed computed by aggregating the newest items from every page's data, newest first.

### 11. Account
Sixth nav tab. Top: an **Action items** panel (same navy gradient style as Featured) showing **account-level** items only: adoption review due, renewal approaching, ticket awaiting reply. Cards below in a two-column grid:
- **Account contacts** — CSM and Account Manager, Email links, "Book with {name}" buttons and an adoption-review reminder (currently open a prefilled email; swap for real calendar links).
- **Renewal** — date, "Your renewal is coming up in X days.", View renewal details, disabled Sign renewal (needs e-signature integration), and a "Considering for your renewal" strip (MAX for Sales, Momentus Analytics; sample).
- **Support tickets** — Submit a ticket, View your tickets, Open/Closed links. Zendesk blocks iframe embedding, so these open the Zendesk support center in a new tab.
- **Product usage** — organization-level bars (sample data; Pendo/MIP in production).
- **Product feedback** — submit, your submissions, upvote sample items (not persisted).
- **Relevant for you** — three low-key recommendations.
Contacts, renewal, usage, feedback and action items are sample data and need CRM/Pendo/MIP/Zendesk integrations.

### 12. Profile Panel
The "SK" avatar opens a right-side drawer (same pattern as the Notifications Center): user info and subscriptions, points and badges, courses and certifications with Learning Center links, individual product adoption with a next step (kept separate from org-level usage on Account), personalized resource links, and notification preference toggles. All sample data.

### 10. Subscribe Modal (Featured page)
Triggered by the blue **Subscribe** button above the Recommended/Announcements carousel arrows on the Featured page. Modal content:
- Title **"Subscribe"**, subtitle: "Receive new announcements and recommendations directly to your inbox."
- **Delivery frequency** section as a **checklist (multi-select checkboxes)**, no sliders/toggles:
  - **Instant** — "Receive an email each time a new announcement or recommendation is curated for you."
  - **Weekly Digest** — "Receive a weekly roundup of every announcement and recommendation curated in the last 7 days."
  - **Monthly Digest** — "Receive a monthly roundup of every announcement and recommendation curated in the last 30 days."
- Each frequency row has a **"Preview"** link that opens the matching standalone digest email template (see Digest Templates below) in a new tab.
- No per-type "Email me about" checklist — that was explicitly removed; frequency is the only selectable set.

## Digest Email Templates
Three standalone HTML files, modeled on a real Momentus support-center digest email (June 2026 Digest reference) — `Digest-Instant.html`, `Digest-Weekly.html`, `Digest-Monthly.html` (also present as `.dc.html` sources and a combined `Momentus Digests.dc.html` with a switcher for iterating on all three side-by-side).
- **Instant**: single curated item — one announcement/recommendation with title, description, and a CTA button.
- **Weekly Digest**: roundup layout — section header ("Your Weekly Digest"), multiple item cards grouped/listed for the past 7 days.
- **Monthly Digest**: same roundup pattern scaled to 30 days, more items, monthly framing in the header.
All three share the Momentus header/logo lockup and footer, matching brand type/color.

## Design Tokens

**Typography**: Plus Jakarta Sans (primary), Verdana/Geneva/Tahoma/sans-serif (fallback stack). Page titles ~26–30px bold; section headers ~18–22px bold; body 14–16px regular; captions 11–12px.

**Colors**:
- Background (light): `#FFFFFF` / Momentus Neutral `#F2EEEC`
- Text: dark `#1A1A2E` / `#1c1e61` (headings), body `#555770` / `#68737d`
- Primary accent blue: `#4146E0`
- Gradient (hero/nav): `#4146E0 → #9B59B6`
- Momentus Yellow (sparing highlight): `#FFF35F`
- Category accents used across cards: cyan `#7DD3FC`, teal `#2DD4BF`, green `#34D399`, purple `#A78BFA`, pink `#FF6B9D`
- Support/Learning dark surfaces: navy `#141634` / `#1c2150` / `#0e1030`
- Borders/dividers: `#e6e0dc` / `#f0eeec`

**Radii**: cards/buttons 8–14px; pills/chips `99px` (fully rounded); avatar circles `50%`.

**Shadows**: soft, low-elevation (`0 3px 10px rgba(0,0,0,0.15–0.2)`), used sparingly on floating chips/date badges — no heavy card shadows.

## Assets
- `assets/momentus-logo-white.png` — official Momentus wordmark (white/reverse), used on all gradient/dark surfaces (nav, login plate, Learning hero).
- `assets/momentus-star-white.png` — official Momentus star logomark, used as a low-opacity watermark motif on dark hero surfaces and course/event/reward thumbnails.
- `assets/momentus-learning-center.png` — real Learning Center banner image supplied by Momentus, used as-is on the Learning page hero.
- All course/mission/reward/community photos are **live CDN URLs** referenced directly in the mock data (not bundled as local files) — pulled from Momentus's real Zealot/community CDN. A production build should source these from real CMS/CDN records, not hardcode URLs.

## State Management (as prototyped)
Single top-level component state: `authenticated`, `activeTab`, `searchQuery`, plus page-local sub-state: `cmsTab` (Featured sub-tab), `announceFilter`, `commTab` (Community's 6 sub-tabs), `eventsTab` (Upcoming/Past), `eventFilter` (type), `eventQuery`, `rsvped` (map — now unused after RSVP removal, can be dropped), `subscribed` (What's New source toggles), `announceFreq` (Subscribe modal frequency checkboxes), drawer/modal open flags. All derived data (search index, filtered lists, grouped feeds) is computed in `renderVals()`/logic methods each render — no external state library.

In a real implementation, most of this becomes: auth session (real SSO/OIDC), server-side content aggregation per source (or federated search), user notification-preference records (frequency + source subscriptions), and a scheduled digest-email job.

## Files in this bundle
- `index.html` — **upload this as the GitHub Pages entry point** (same file as `Momentus Customer Hub.html`).
- `Customer Hub.dc.html` — main prototype source (all nav destinations, drawers, modals).
- `Customer Hub Standalone Source.dc.html` — source used to build the standalone bundle (LcCard inlined).
- `LcCard.dc.html` — Learning Center course card sub-component.
- `Momentus Digests.dc.html` — combined digest template source with switcher.
- `Digest-Instant.html`, `Digest-Weekly.html`, `Digest-Monthly.html` — standalone bundled digest emails.
- `Momentus Customer Hub.html` — standalone bundled export of the full hub (open directly in any browser, no build step; live CDN images require internet).
- `Customer Hub - Backend Q&A.dc.html` (+ print version) — companion document on backend/provisioning realism and unified-search feasibility; read before scoping engineering.

- `Customer Hub - Go-Live Roadmap.dc.html` and `Customer Hub - Executive Summary.md` — director-facing case, roadmap and blockers.

## GitHub Pages notes
Put `index.html`, `Digest-Instant.html`, `Digest-Weekly.html` and `Digest-Monthly.html` in the repo root; the Subscribe modal's Preview links resolve to the digest files by relative path. Console 404s for `{{ r.img }}`-style URLs come from image placeholders in the bundled template and are harmless. The `previews/` screenshots predate the Account page, Smart Search and profile panel.
