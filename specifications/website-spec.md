# AWS User Group BuildHers+ Philippines: Website Specification

**Status:** Draft for review · **Scope:** navbar pages, shared chrome, non-functional requirements
**Branding:** see `specifications/branding.md` (TBD). This spec defines no colors, fonts, logos or imagery.

---

## 1. Overview

### 1.1 Mission (drives every page)

> We empower women and LGBTQIA+ people in tech and create safe, inclusive spaces for them to learn AWS and the cloud, build skills and grow their careers.

The site leads with people and belonging, then AWS. Safety (Code of Conduct and a way to report concerns) is visible on every page, not buried.

### 1.2 Facts the site is built on

| Fact | Value | Notes |
|---|---|---|
| Founded | 2018 | Originally "AWS Women's Group PH"; renamed "AWS User Group BuildHers+" in 2022 to explicitly welcome LGBTQIA+ people |
| Recognition | Officially recognized by AWS Global | One of the longest-running AWS User Groups in the Philippines |
| Reach | Philippines-wide | Planning local chapters in Visayas and Mindanao |
| Members | 1.3k (Facebook group) + 700+ (Meetup) | Shown as **"1.5k+ registered members"** (Facebook group and Meetup combined) |
| Events hosted | 35+ | |
| Ambassadors | 30+ | Across local student builder groups and learning clubs |
| Event cadence | 2 per quarter | Plus flagship celebrations: **Women's Month (March)** and **Pride Month (June)** |
| Event platforms | Meetup.com and Luma | RSVP happens there; the site links out |
| Language | English only | |
| Contact | awsugbuildhersph@gmail.com | Also the safety-reporting address (see 5.4) |

### 1.3 Audience

1. Women and LGBTQIA+ people in or entering tech (students, career shifters, early- to mid-career).
2. Student builder groups and learning clubs.
3. Prospective speakers, volunteers, sponsors and partners.
4. Allies who want to support the community.

### 1.4 Reference patterns (adapted, not copied)

Structural inspiration from awsugdavao.ph: announcement ticker, hero with two CTAs, stats strip, numbered "What we do" cards, blog and announcement previews, multi-column footer. The newsletter signup pattern is **deferred** (see 6.5), because there is no newsletter yet.

> **Note:** the reference site could not be fetched from the authoring environment (network policy). The patterns above come from the brief. Re-verify against the live site if exact layout matters.

### 1.5 Site map

```
HOME (/)
EVENTS (/events, /events/[slug])
BLOG (/blog, /blog/[slug], /blog/tag/[tag])
ANNOUNCEMENTS (/announcements, /announcements/[slug])
ABOUT (/about, with #code-of-conduct and #report anchors)
[ JOIN → ]  CTA only, links out to the Facebook group (not a page)
```

Footer-only utility pages (outside navbar scope, listed so links don't dead-end): Privacy Notice, 404.

### 1.6 Tech context (for fit only)

Astro + TypeScript, static-first. Events, blog posts and announcements are Markdown/MDX in Astro content collections, so volunteers contribute by pull request. Hosted on AWS Amplify Hosting. No server-side form is needed at launch (see 6.5).

---

## 2. HOME (`/`)

**Purpose:** Tell a visitor in seconds who this community is for, that it is safe, and how to join.

### 2.1 Sections, in order

| # | Section | Content |
|---|---|---|
| 1 | **Announcement ticker** | Thin bar above the navbar. Rotates the latest announcements flagged `ticker: true` (max 5), each linking to its announcement. Pause on hover/focus, pause/play button, no auto-motion when `prefers-reduced-motion`. Hidden if there are no active items. |
| 2 | **Navbar** | See section 7.1. |
| 3 | **Hero** | Headline expressing the mission (empowerment + safe spaces) in plain language; one-sentence supporting line naming women and LGBTQIA+ people in tech and AWS/cloud learning. Two CTAs: **primary "Join the community"** (Facebook group) and **secondary "See upcoming events"** (`/events`). A one-line safety reassurance under the CTAs, linking to the Code of Conduct. Optional image slot (see branding). |
| 4 | **Stats strip** | Three stats, from the site config: **1.5k+** registered members (Facebook group and Meetup) · **35+** events hosted · **Since 2018**. Optional 4th: **30+** student ambassadors, and/or "Free to join" (see Open questions). Each stat has a short caption; numbers are plain text (not images) and not animated for reduced-motion users. |
| 5 | **Who this community is for** | Short inclusion statement: women and LGBTQIA+ people, all levels and backgrounds, no prior AWS experience required (confirm wording with the chapter). Links to About. |
| 6 | **What we do** | Four numbered cards (01-04): **Learn** (AWS and cloud workshops, talks, study groups) · **Build** (hands-on sessions and projects) · **Grow** (career growth: mentorship, speaking, networking) · **Belong** (safe, inclusive spaces; Women's Month and Pride Month celebrations). Each card: number, title, 1-2 sentence description, optional link. Final copy is the chapter's to confirm. |
| 7 | **Next event** | The next upcoming event as a large card (cover, title, date/time in PH time, format, venue, RSVP button). If none: empty state (see 2.3). Below it: "All events →". |
| 8 | **Safe space and Code of Conduct band** | Always-visible block: one line on what a safe space means here; link **"Read our Code of Conduct"** (`/about#code-of-conduct`); **"Report a concern"** with the dedicated email and the Facebook page (see 5.4). Appears on Home regardless of event/blog state. |
| 9 | **Latest from the blog** | Up to 3 most recent posts as cards, "All posts →". |
| 10 | **Latest announcements** | Up to 3 most recent as compact list items with type badge and date, "All announcements →". |
| 11 | **Our partners and supporters** | Logo row of past sponsors, event partners and community partners (from the `partners` collection); hidden if none. Link to About. |
| 12 | **Join CTA band** | Closing call to action: "Join the community on Facebook" (group, primary) and "Follow our page" (secondary). Short line that membership is a community group on Facebook. No form (see 6). |
| 13 | **Footer** | See section 7.2. |

### 2.2 Key components

- `AnnouncementTicker`, `Hero`, `StatsStrip`, `NumberedCard`, `EventCard`, `PostCard`, `AnnouncementItem`, `SafeSpaceBand`, `PartnerLogoRow`, `JoinBand`.

### 2.3 States

| Component | Empty | Loading | Notes |
|---|---|---|---|
| Ticker | Hidden entirely | n/a (static) | Expired items excluded at build |
| Next event | "No events scheduled yet. Follow us on Facebook, Meetup and Luma to hear first." with links in the Home social order (see 7.2) | n/a | If the next event is within 7 days, show a "Happening soon" label |
| Blog / Announcements | "Nothing here yet" with a link to the other content or contribution guide | n/a | Section hides if the collection is empty at launch |
| Partners | Section hidden | n/a | |

Content on Home is generated at build time, so there is no client loading state. Rebuilds are triggered on merge to the main branch, and a scheduled daily rebuild moves events from "upcoming" to "past" and expires ticker items.

---

## 3. EVENTS (`/events`)

**Purpose:** Show what's coming up and give people a way to RSVP, and keep a browsable archive of past events.

### 3.1 Sections, in order

| # | Section | Content |
|---|---|---|
| 1 | **Page header** | Title, one-line intro (cadence: "Two events every quarter, plus our Women's Month and Pride Month celebrations"). |
| 2 | **Upcoming events** | Next event featured, then the rest as a list or grid sorted by date ascending. Each `EventCard`: cover image, title, date and time (Asia/Manila), format badge (In-person / Online / Hybrid), venue or "Online", series label (Women's Month, Pride Month, Regular), RSVP button (labeled by platform: "RSVP on Meetup" / "RSVP on Luma"). |
| 3 | **Flagship celebrations** | Short callout for the recurring March (Women's Month) and June (Pride Month) events, linking to the matching upcoming or latest past event. |
| 4 | **Past events archive** | Reverse-chronological, grouped by year, with a year filter. Cards link to the event detail page with recap. Paginated or "Load more" (static, no JS needed for baseline). Target to support 35+ entries at launch. |
| 5 | **Follow for new events** | Links to follow on Meetup, Luma and the Facebook page/group. (Replaces a newsletter signup.) |
| 6 | **Safety footer note** | One line linking to the Code of Conduct and report contact. |

### 3.2 Event detail (`/events/[slug]`)

Cover image · title · series label · date/time with an "Add to calendar" (.ics) link · format · venue with address, map link and accessibility notes (in-person) or join instructions (online, shown after RSVP via the platform) · description (MDX) · speakers/agenda · partners/sponsors · RSVP button (upcoming) or **recap** block (past): summary, slides link, photo link, recording link, attendee count · "Code of Conduct applies to this event" notice with report contact.

### 3.3 Components and states

| Component | States |
|---|---|
| `EventCard` | Upcoming · Happening soon · **Past** (RSVP replaced by "View recap") · **Cancelled/Postponed** (status badge, RSVP disabled with reason) · **RSVP closed/Full** (button disabled, text explains) |
| Upcoming list | Empty: "No upcoming events right now." plus follow links (as 2.3) |
| Past archive | Empty (new site): "Our event history is being added." plus link to the Meetup past events page |
| Year filter | Default "All"; filters by link (works without JS), enhanced with JS if present |
| Event without cover image | Uses a neutral placeholder defined in branding |

### 3.4 Content model: `events` collection (Markdown/MDX)

| Field | Type | Req. | Notes |
|---|---|---|---|
| `title` | string | ✓ | |
| `description` | string (≤160 chars) | ✓ | Card and meta description |
| `startDate` | datetime (ISO, Asia/Manila) | ✓ | Status (upcoming/past) is derived from this |
| `endDate` | datetime | | |
| `format` | enum: `in-person` · `online` · `hybrid` | ✓ | |
| `venueName` | string | if in-person/hybrid | |
| `address` | string | | |
| `city` | string | | |
| `mapUrl` | url | | |
| `onlineUrl` | url | | Only if safe to publish; otherwise leave on the RSVP platform |
| `rsvpUrl` | url | ✓ for upcoming | |
| `rsvpPlatform` | enum: `meetup` · `luma` · `other` | ✓ with `rsvpUrl` | Sets button label |
| `rsvpStatus` | enum: `open` · `closed` · `full` | | Default `open` |
| `status` | enum: `scheduled` · `cancelled` · `postponed` | | Default `scheduled` |
| `series` | enum: `womens-month` · `pride-month` · `regular` | | |
| `coverImage` | image | | |
| `coverImageAlt` | string | ✓ if `coverImage` | |
| `speakers` | list of `{name, role?, link?}` | | |
| `partners` | list of references to `partners` | | |
| `accessibilityNotes` | string | | Step-free access, captions, interpreters, etc. |
| `recap` | object: `summary?`, `slidesUrl?`, `photosUrl?`, `videoUrl?`, `attendeeCount?` | | For past events |
| `tags` | string[] | | |
| `featured` | boolean | | |
| `draft` | boolean | | Excluded from build when true |

---

## 4. BLOG (`/blog`)

**Purpose:** Share learning, stories and recaps from community members so that others can learn from them.

### 4.1 Sections, in order

| # | Section | Content |
|---|---|---|
| 1 | **Page header** | Title and one-line intro (stories, tutorials and recaps from the community). |
| 2 | **Featured post** | Latest post (or one with `featured: true`) as a large card. |
| 3 | **Filter by tag** | Tag chips; each links to `/blog/tag/[tag]`; "All" selected by default. |
| 4 | **Post grid** | `PostCard`s newest first: cover, title, description, author, date, reading time, tags. Paginated (9-12 per page). |
| 5 | **Write for us** | Short CTA inviting members to contribute via pull request; links to the contribution guide in the repository (`CONTRIBUTING.md`, to be written) and states that all contributors, including beginners, are welcome. |
| 6 | **Follow us** | Social links (replaces newsletter signup). |

### 4.2 Post detail (`/blog/[slug]`)

Title · author(s) with optional link · date (and "Updated" date if present) · reading time · tags · cover image · MDX body (headings form an on-page table of contents when 3+ headings; code blocks have a copy button and are keyboard accessible) · share links (copy link, plus the page's social metadata) · previous/next post · related posts by tag.

### 4.3 Components and states

`PostCard`, `TagChip`, `Pagination`, `AuthorByline`, `TableOfContents`.
Empty blog: "Our first stories are coming soon" with the "Write for us" CTA. Empty tag: "No posts with this tag yet" and link to all posts. Post with no cover: placeholder from branding.

### 4.4 Content model: `blog` collection

| Field | Type | Req. | Notes |
|---|---|---|---|
| `title` | string | ✓ | |
| `description` | string (≤160) | ✓ | Cards and meta description |
| `pubDate` | date | ✓ | |
| `updatedDate` | date | | |
| `authors` | list of `{name, link?, avatar?}` | ✓ | Contributors, not only organizers |
| `tags` | string[] | ✓ | Lowercase, kebab-case |
| `coverImage` | image | | |
| `coverImageAlt` | string | ✓ if `coverImage` | |
| `featured` | boolean | | |
| `relatedEvent` | reference to `events` | | For recaps |
| `canonicalUrl` | url | | If cross-posted |
| `draft` | boolean | | |

Reading time is computed at build, not authored.

---

## 5. ANNOUNCEMENTS (`/announcements`) and ABOUT (`/about`)

### 5.1 ANNOUNCEMENTS

**Purpose:** Give members one place for time-sensitive community news: event notices, calls for volunteers and new blog posts.

#### Sections, in order

| # | Section | Content |
|---|---|---|
| 1 | **Page header** | Title, one-line intro, RSS link. |
| 2 | **Pinned announcements** | Items with `pinned: true` (max 3) shown first with a "Pinned" label. |
| 3 | **Type filter** | Chips: All · Event notice · Call for volunteers · New blog · General. Link-based, works without JS. |
| 4 | **Announcement list** | Reverse-chronological, grouped by month. Each `AnnouncementItem`: type badge, title, summary, date, link/CTA. Paginated. |
| 5 | **Where else to follow us** | Social links in the standard order (7.2). |

**Announcement detail (`/announcements/[slug]`):** title, type, date, body (MDX), CTA button if `link` is set, related event or post, back to list. Expired items remain readable with an "Expired" label.

#### States

Empty list: "No announcements yet. Follow our Facebook page to stay updated." Empty filter result: "No {type} announcements" with a reset link. Expired call for volunteers: badge "Closed", CTA removed.

#### Content model: `announcements` collection

| Field | Type | Req. | Notes |
|---|---|---|---|
| `title` | string | ✓ | |
| `summary` | string (≤200) | ✓ | |
| `publishDate` | date | ✓ | |
| `type` | enum: `event` · `volunteer-call` · `blog` · `general` | ✓ | |
| `link` | url or internal path | | CTA target |
| `linkLabel` | string | | Default by type, e.g. "RSVP", "Apply", "Read post" |
| `relatedEvent` / `relatedPost` | reference | | |
| `ticker` | boolean | | Show in the Home ticker |
| `pinned` | boolean | | |
| `expiresOn` | date | | After this: leaves the ticker, shows "Expired" |
| `draft` | boolean | | |

### 5.2 ABOUT

**Purpose:** Explain who the community is, what it stands for, how it keeps people safe, and who supports it, centered on the community itself, not individual organizers.

#### Sections, in order

| # | Section | Content |
|---|---|---|
| 1 | **Intro** | One-paragraph "who we are", mission statement, safety reassurance. |
| 2 | **Our mission and vision** | Mission (verbatim from 1.1). Vision statement: **to be supplied by the chapter** (see Open questions). |
| 3 | **Our story** | Timeline: 2018 founded as AWS Women's Group PH → 2022 renamed to AWS User Group BuildHers+, welcoming LGBTQIA+ people → today (1.5k+ members, 35+ events, AWS Global recognition). Milestones beyond these come from the chapter. |
| 4 | **Who we welcome and what we value** | Inclusion statement and 3-5 community values (copy supplied by the chapter). |
| 5 | **Our community across the Philippines** | Philippines-wide reach; 30+ ambassadors across student builder groups and learning clubs; **"Growing to Visayas and Mindanao"** with a short note on local chapters and a contact link for people who want to help start one. |
| 6 | **Code of Conduct** (`#code-of-conduct`) | Full text of the Code of Conduct, rendered from a Markdown file in the repository. See 5.3. |
| 7 | **Report a concern** (`#report`) | See 5.4. |
| 8 | **Sponsors and partners** | Three groups: past sponsors · event partners · community partners. Logo + name + link, from the `partners` collection. Optional "Partner with us" link to the contact email. |
| 9 | **Connect with us** | Social links in the standard order, contact email. |

#### Components and states

`Timeline`, `ValueCard`, `CodeOfConduct` (long-form Markdown with anchor links per heading), `ReportConcernCard`, `PartnerGroup`.
Empty partners group: group hidden; if all are empty, show "Interested in supporting the community? Get in touch." with the contact email.

#### Content model: `partners` collection (data)

| Field | Type | Req. | Notes |
|---|---|---|---|
| `name` | string | ✓ | |
| `kind` | enum: `sponsor` · `event-partner` · `community-partner` | ✓ | |
| `logo` | image | | If absent, name is shown as text |
| `logoAlt` | string | ✓ if `logo` | Usually the partner name |
| `url` | url | | |
| `since` / `years` | number or number[] | | |
| `draft` | boolean | | |

Static About copy lives in MDX (`about.mdx`) and the Code of Conduct in `code-of-conduct.md`, so both are editable by pull request.

### 5.3 Code of Conduct

**There is no Code of Conduct yet. It must exist before launch.** Requirements for it:

- Written or adopted by the chapter. Recommend starting from a published open template and adapting it to the community's mission (explicitly covering gender identity, gender expression and sexual orientation).
- Covers: expected behavior, unacceptable behavior, scope (events, Facebook group/page, Meetup, Luma, online and in-person), how to report, who receives reports, what happens next, and confidentiality.
- States a response commitment (e.g. acknowledgement within a set time, to be decided by the chapter).
- Linked from: Home safe-space band, hero line, every event page, the footer on all pages, and About.

### 5.4 Reporting a concern

Visible on Home (band), About (`#report`), every event detail page and the footer.

- **Email:** awsugbuildhersph@gmail.com
- **Facebook page:** https://www.facebook.com/awsugbuildhersph (message the page)
- The `ReportConcernCard` states: what can be reported, that reports are handled confidentially, who reads them (to be named by the chapter), and the expected response time.
- Email link is a `mailto:` with a subject prefilled ("Concern report"). No on-site form at launch (nothing is stored on our servers).

---

## 6. JOIN (CTA, not a page)

**Purpose:** Get a visitor into the community with one click.

### 6.1 Destination

JOIN opens the community's **Facebook group** (https://www.facebook.com/share/g/1HwnnJnnZg/) in a new tab. A secondary link, where space allows, goes to the **Facebook page** (https://www.facebook.com/awsugbuildhersph). There is no membership form and no newsletter-only option for now.

### 6.2 Placement

Navbar button (all pages) · Home hero (primary CTA) · Home closing Join band · Footer "Community" column · About "Connect with us".

### 6.3 Behavior and accessibility

- Opens in a new tab with `rel="noopener noreferrer"`; the accessible name says so ("Join the community on Facebook, opens in a new tab").
- The destination URL lives in a single site-config value, so changing where JOIN leads is a one-line change.
- If the group is private or requires approval, a one-line note under the CTA says so ("You'll answer a few questions to join").

### 6.4 Form fields

**None at launch.** No data is collected by the site.

### 6.5 Deferred: newsletter / membership form (not in launch scope)

Kept here so the design isn't blocked later. If the chapter adds a newsletter:

- Placement: a "Stay in the loop" section on Home (above the footer) and in the footer.
- Candidate fields: name (required), email (required), city/region (optional), career stage (optional), AWS experience (optional), interests (optional), consent checkbox for the Data Privacy Act of 2012 (required). Any identity-related field (pronouns, self-identification) is optional, voluntary and explained.
- Endpoint: small AWS Lambda behind Amplify/API Gateway; validation, spam protection (honeypot and rate limit or CAPTCHA), double opt-in.
- Storage and email options are listed under Open questions.
- States: idle · submitting · success ("Check your email to confirm") · validation error · server error with retry · already subscribed.

---

## 7. Shared: Navbar, Footer, Community links

### 7.1 Navbar

`HOME · EVENTS · BLOG · ANNOUNCEMENTS · ABOUT · [ JOIN → ]`

- Sticky on scroll; logo/wordmark (from branding) links to Home.
- Current page indicated visually and with `aria-current="page"`.
- Mobile: collapses to a menu button with a full-screen or drawer menu; JOIN stays visible outside the collapsed menu or as the first item in it. Menu is keyboard-operable and traps focus while open.
- Skip-to-content link is the first focusable element.
- The ticker (Home) sits above the navbar.

### 7.2 Footer (multi-column)

| Column | Content |
|---|---|
| **About the community** | Name, one-sentence mission, "Founded 2018". |
| **Explore** | Home, Events, Blog, Announcements, About. |
| **Community** (links in this order) | 1. Facebook page · 2. Facebook group · 3. Meetup · 4. Luma · 5. LinkedIn. JOIN button here. |
| **Safety** | Code of Conduct · Report a concern · awsugbuildhersph@gmail.com |
| **Legal / base row** | Privacy Notice · © year, chapter name · Built with Astro on AWS Amplify (optional) · "Contribute on GitHub" link. |

All social links open in a new tab and have accessible names ("AWS UG BuildHers+ on Facebook, opens in a new tab"). The same ordered list feeds every "follow us" spot on the site, from one config value.

### 7.3 Site configuration (single source)

Stats numbers, social URLs and order, JOIN target, contact email, founded year. Content authors update one file; no page hard-codes them.

---

## 8. Non-functional requirements

### 8.1 Accessibility (WCAG 2.2 AA)

- Semantic landmarks (`header`, `nav`, `main`, `footer`), one `h1` per page, logical heading order.
- Fully keyboard operable; visible focus indicators; focus not obscured by the sticky navbar (2.4.11); target size at least 24×24 CSS px (2.5.8).
- Color contrast meets AA, with checks against the values in `specifications/branding.md` (TBD).
- Text alternatives for all meaningful images (`coverImageAlt` and `logoAlt` are required by the content schema); decorative images use empty alt.
- The ticker and any carousels can be paused and respect `prefers-reduced-motion`.
- Link text is descriptive; external links say they open a new tab.
- Forms (when added) have visible labels, error identification and no redundant entry (3.3.7).
- Content authors: contribution guide includes an accessibility checklist (alt text, heading order, link text).
- Audit: automated (axe/Lighthouse in CI) plus a manual keyboard and screen-reader pass before launch.

### 8.2 Mobile-first responsive layout

- Designed for ~360px first; breakpoints scale up. No horizontal scroll at any width, and content reflows at 320px and 400% zoom.
- Touch-friendly targets; the navbar collapses to a menu; stats strip wraps; grids go 1 → 2 → 3 columns.
- Text can scale to 200% without loss of content.

### 8.3 SEO and social sharing

- Unique `<title>` and meta description per page, built from content frontmatter.
- Canonical URLs, `sitemap.xml`, `robots.txt`, RSS feeds for blog and announcements.
- Open Graph and Twitter card tags per page (title, description, image); per-item cover image used when set, with a default share image from branding.
- Structured data: `Organization` (site-wide), `Event` (event pages: date, location/online, status, organizer), `BlogPosting` (posts), `BreadcrumbList`.
- Human-readable, stable URLs; redirect rules in Amplify for any future slug changes.
- `lang="en"`; 404 page that links back to the main sections.

### 8.4 Performance

- Static HTML first; JavaScript only where it adds value (menu, ticker, optional filters), loaded as small islands.
- Targets on mobile (4G): LCP ≤ 2.5s, INP ≤ 200ms, CLS ≤ 0.1; Lighthouse performance ≥ 90.
- Responsive, modern-format images (AVIF/WebP) with width/height set, lazy-loaded below the fold; hero image prioritized.
- Fonts subset and `font-display: swap` (families defined in branding).
- Third-party embeds (Meetup/Luma widgets, social) are avoided; the site links out instead.
- Budgets enforced in CI (page weight and Lighthouse checks).

### 8.5 Privacy, security and content operations

- Analytics, if any, are privacy-friendly and cookie-less where possible; if cookies are used, a consent notice is shown (see Open questions).
- No personal data is collected at launch. Meeting the Philippine Data Privacy Act of 2012 becomes necessary once forms are added.
- Security headers via Amplify (CSP, HSTS, X-Content-Type-Options, Referrer-Policy).
- Contribution flow: content collections validate frontmatter schemas at build; a failing schema fails the PR; preview deployments per PR for reviewers; all content PRs reviewed by a maintainer before merge.
- Images supplied by contributors need consent from featured people; photos of attendees follow the chapter's photo policy (to be defined).

---

## 9. Open questions

**Safety**
1. The Code of Conduct needs to be written or adopted. Who is the owner, and by when? Which open template should it start from?
2. Who reads the reports at awsugbuildhersph@gmail.com, what's the response-time commitment, and should a shared (non-personal) mailbox or an anonymous reporting option be added?
3. Is a photo/recording consent policy needed for events?

**Content to be supplied**
4. Vision statement, community values and inclusion statement wording.
5. Final copy and number of "What we do" cards (the four above are placeholders).
6. Which stats to show beyond members, events and founded year: 30+ ambassadors? "Free to join" (is membership always free)?
7. Timeline milestones beyond 2018 and 2022; named partners and sponsors with logos and permission to display.
8. LinkedIn URL and Meetup and Luma profile URLs for the footer.
9. Is the Facebook group private or approval-based, and what does the join-questions flow look like?

**Events data**
10. How to populate the 35+ past events: manual Markdown entries, or a one-off import from Meetup? (The Meetup past-events page couldn't be reached from the authoring environment, so it must be checked by someone with access.)
11. Who adds each new event and how soon after Meetup/Luma, and do we want a rule to avoid duplication between the two?
12. Do Women's Month and Pride Month events need special landing pages or only the `series` label?

**Platform and process**
13. Announcement automation: should publishing a blog post auto-create an announcement, or is it always manual?
14. Analytics: yes/no, and which tool (cookie-less or consent banner)?
15. Privacy Notice owner and text, even with no forms at launch (Facebook/Meetup links, analytics).
16. Should the footer carry an AWS non-affiliation / trademark notice? (The brief says not to mention the wider AWS programmes, but a trademark disclaimer may still be required by AWS.)
17. Custom domain name and who owns DNS/Amplify access.

**Deferred newsletter / membership (only if added later)**
18. Storage for sign-ups: DynamoDB, S3 (JSON/CSV), a Google Sheet, or direct into an email service.
19. Email tool: Amazon SES, Mailchimp, Buttondown, or another.
20. Spam protection: honeypot + rate limit, or CAPTCHA (hCaptcha/Turnstile)?
21. Is self-identification data (pronouns, LGBTQIA+) collected at all? If yes, retention and access policy.
22. Local chapters (Visayas, Mindanao): do they get sections or pages of their own later?
