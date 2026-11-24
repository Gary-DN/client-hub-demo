# ClientHub 360 — Complete Platform Scope & Build Guide

> **Purpose:** Master reference specification and production build guide for **ClientHub 360 (DNXT Client Hub)** — a 360-degree agency operating system handling the entire client lifecycle from Prospecting and Quoting through Onboarding, Production, Reporting, Retention, and Offboarding, plus internal Agency Tools, Slack Alerts, and AI/MCP integration.

---

## 1. End-to-End Commercial & Delivery Flow

```text
Lead / Upsell  ──▶  Quote Builder  ──▶  Proposal & E-Sign  ──▶  Magic Link Sign-Off
                                                                        │
Retention / Offboarding  ◀──  Reporting & Portal  ◀──  Production  ◀──  Onboarding
```

- **App Delivery:** Installable **Progressive Web App (PWA)** (`manifest.json`, `sw.js`, Digital Next `favicon.png` / `192x192` / `512x512` icons) so staff can pin the Hub to their Windows Taskbar and clients can access a mobile-responsive Reporting Portal.
- **No-Login Client Touchpoints:** Clients interact via expiring, passwordless **Magic Links** (zero per-link cost) for **Proposal E-Sign**, **Onboarding Credential Collection**, **Vault Login Requests**, and **One-Click Content Approvals**.

---

## 2. Core Lifecycle Modules

### 2.1 Prospecting & Pipeline (`prospecting.html`)
- **Tab Bar:** `Pipeline (Kanban)` | `Files` | `Meetings` | `Activity Log` | `Proposals` | `Lost (Archive)`
- **Pipeline Views:** Toggle between **Kanban** and **List View**.
- **Kanban Columns (4 Active Stages):**
  1. `New Lead` (Inbound, Website Form, Outbound LinkedIn, Referral, Google Ads, Manual Entry)
  2. `Discovery Complete`
  3. `Quote Building`
  4. `Awaiting Sign-Off`
- **Add New Prospect Form:** Captures Business Name, Contact Name, Email, Phone, Website URL, Lead Source (Attribution), Proposed Budget, Deal Probability (%), Contract Term, Billing Cycle, Assigned Sales Rep, Services Discussed (multi-select), and Discovery/Handover Notes.
- **Deal Detail Modal:**
  - **Client Data & Commercial Terms** + **Sales Handover Notes (Scope Guardrail)**.
  - **Automated Company & Finance Enrichment (Creditsafe / Companies House):** Credit Limit, Credit Risk Score, Company Reg No., Est. Turnover & Staff Size, VAT Number, CCJs / Adverse Filings.
  - **Linked Quote Builder & Proposal/Agreement:** Direct link (`quote-builder.html?client=...`) pre-filling the prospect in Quote Builder, plus direct link to the E-Sign Agreement.
  - **Mandatory Sign-Off Gate:** Displays **`All Documents Complete`** (Quote approved, Agreement e-signed & locked, Xero invoice sent on 30-day payment terms) with a **Sign Off & Move to Onboarding** button.
  - **Defer Lead (Archive & Re-engage):** Pick a future re-engagement date + note to archive the lead from the active board and automatically return it to `New Lead` on that date.
- **Files Tab (Sales Workspace Sync & Conversion Handoff):**
  - Prospects sync with a dedicated **Google Workspace Sales Folder** (`/Sales-Prospects/<Prospect>/`).
  - On conversion to a live client, **working files automatically move to the Production Client Folder** in Google Workspace, and **signed agreements lock into an immutable Google Cloud Storage (GCS) Bucket**.
- **Meetings Tab:** Schedule Google Meet discovery calls, track attendance, and **Log Manual Contact** (Phone, In-Person, LinkedIn/DM, Teams/Zoom) with optional file upload.
- **Proposals Tab:** Tracks all prospect proposals with status (`Created`, `Sent`, `Sent • Viewed`, `Signed`), view count, time spent, and quick links to **Create from Quote Builder** or **Manual Proposal**.
- **Lost (Archive) Tab:** Holds Closed-Lost deals (Reason: Budget, Competitor, Timing, In-House), notes, and a **Re-Open to Leads** button to restore returning prospects to `New Lead`.

---

### 2.2 Onboarding & Passwordless Magic Link (`onboarding.html`, `magic-link.html`)
- **Live Client Creation Profile:** Business Name, Primary Contact, URL, Services Subscribed & Live Budget, Slack Handle (`@client-handle`), Status (`Active` / `Issue` / `Cancelled`), and Days as Live Client.
- **OAuth Account Connectors (Not just raw IDs):**
  - One-click **"Connect Account"** / **"Reconnect"** OAuth flows for **Google Analytics 4 (GA4)**, **Google Search Console**, **Google Tag Manager (GTM)**, and **Google Ads MCC**.
  - Synced status for **CMS / Staging Logins (1Password / Bitwarden Vault)**, **Brand Guidelines & Asset Folders (Google Drive)**, and **Competitors List (3–5 URLs)**.
- **Client-Facing Magic Link Wizard (`magic-link.html`):**
  - Passwordless, expiring token URL that **dynamically shows/hides credential sections** based on the client's subscribed services (`SEO`, `Content`, `PPC`, `Paid Social`, `Design & Dev`).
- **Campaign Creation Data:** Blogs/mo, Content pages/mo, Outreach links/mo, Campaign Length, Focus Keyword Area → **Plan Tasks & Populate Boards**.

---

### 2.3 Production & Operations (`production.html`, `content-approval.html`)
- **Header Controls:** Multi-client workspace selector, Active/Month badge, **Profitability Bar** (Allocated vs. Logged Hours), and Month 2 recurring task preview.
- **11 Tabs (Default: `Overview`):**
  `Overview` | `SEO` | `Content` | `PPC` | `Design & Dev` | `Paid Social` | `Live Links / PR` | `Files` | `Meetings` | `Passwords` | `Activity Log`

1. **Overview Tab:**
   - **Xero Overdue Invoice Alert Banner:** Flags overdue invoices (e.g., `#INV-2048 — 9 Days Overdue`) with **Send Xero Reminder** and **Flag as Issue** actions.
   - **Full Client Profile & Contacts:** Company Reg/VAT, Registered Address, Primary Contact, Billing/Finance Contact, Commercial Terms, Agency Team, and **`Hosted by Digital Next (£150/mo)`** marker.
   - **Connected Accounts & Live Connector Health:** Real-time status grid for **Website Status** (pinged every **10 minutes, 7am–7pm**), **GA4**, **Search Console**, **GTM**, **Google Ads MCC**, **Ahrefs/SEMrush**, **Meta Business Manager** (with expiring token refresh), **1Password Vault**, **Xero & Slack**, **Google Merchant Center**, **LinkedIn Ads**, **Figma REST API**, **SFTP/FTP & GitHub**, and **Google Calendar & Meet**.
2. **SEO Tab:**
   - Month 1 automated Competitor Analysis, Initial Website Audit, 12-Month Work Plan Generator; Months 2–12 recurring task engine (populating 1st of each month).
   - **Quarterly Audit Auto-Regeneration:** Whenever a **Quarterly Audit** task is marked complete (`Done`), the task engine automatically schedules the next Quarterly Audit for **3 months' time**.
   - Kanban cards include **Due Dates**, estimated/logged hours, and file attachments.
3. **Content Tab & Budget Attribution Rule (CRITICAL BUILD SPEC):**
   - **SEO-Funded Content Tasks:** An SEO campaign carries a single commercial SEO budget that **includes** recurring Content team deliverables (e.g., monthly blogs, on-page copy). Those Content tasks appear on the Content board for delivery & client approval, while drawing their allocated hours/credits from the **SEO budget**.
   - **Standalone Content Upsell Projects:** Separately, the Content team can also receive dedicated **upsell projects** (e.g., ebooks/resource guides, extra blog packages, copy overhauls) which carry their own separate Content budget alongside the SEO retainer.
   - **Client Content Approval Magic Link (`content-approval.html`):** Temporary passwordless link showing SEO Meta Title/Description preview + full formatted article draft. Client clicks **"Yes (Approve)"** or **"No (Reject / Revise)"** with optional/required feedback; timestamp and notes log directly back to the Production board & Activity Log.
4. **PPC Tab:**
   - Live ad spend pacing bar, **Left Box:** Active PPC Connectors (Google Ads MCC, **Google Merchant Center**, GA4 Conversions, GTM), **Right Box:** **Top 3 Campaigns by Conversions**, **Below:** PPC Kanban board with due dates.
   - **PPC Auto-Generated Tasks (TBD):** PPC will also include recurring auto-generated tasks (finalized task list to be confirmed before build).
5. **Paid Social Tab:**
   - Live social spend pacing & blended CPL/ROAS, **Left Box:** Active Paid Social Connectors (Meta Business Manager, Meta Pixel & Conversions API, LinkedIn Campaign Manager, TikTok optional), **Right Box:** **Top 3 Paid Social Campaigns by Conversions** (with Creative Fatigue frequency alerts), **Below:** Paid Social Kanban board.
6. **Design & Dev Tab:**
   - **Left Box:** Managed Hosting (`Hosted by Digital Next £150/mo`), **Figma REST API** (`Project files synced`), **SFTP / FTP Access**, **GitHub Repo**, and Pre-Launch QA Checklist.
   - **Right Box:** **Figma Project Files Created** (via Figma API) + Bug/Ticket Intake & Revision Round counters (`Round 1 of 2`).
   - **Below:** Design & Dev Kanban board with due dates.
7. **Live Links / PR Tab:**
   - **Top Ahrefs Metrics Strip:** Domain Rating (DR), Referring Domains, Total Live Backlinks, and **7-Day Auto-Ping Monitor**.
   - **Placed URLs Table:** Pings every placed backlink every **7 days** to verify **Live (`200 OK`)** vs. **Down (`404`)** with Last Checked date and Outreach alerts.
8. **Files Tab:** Dedicated directory synced with **Google Workspace Shared Drives** (`01_Contracts`, `02_Brand_Assets`, `03_SEO_Audits`, `04_Content_Drafts`, `05_Reports_PDF`).
9. **Meetings Tab:**
   - **Schedule Google Meet** (via Google Calendar API).
   - **Attendance Verification** (via Google Meet REST API `conferenceRecords` & `participants`: logs `Completed • duration + who joined` or flags `Client No-Show`).
   - **Auto-Saved Call Notes:** Pulls Gemini *"Take notes for me"* / Meet transcripts via Meet & Drive APIs and saves summaries + action items to Meetings & Files.
   - **Log Manual Contact:** Logs offline Phone Calls, In-Person Meetings, Slack Huddles, or Teams/Zoom calls with optional **File Upload**.
10. **Passwords Tab:**
    - Encrypted **1Password / Bitwarden Vault** directory (SFTP/FTP, WordPress CMS, Cloudflare DNS, WP Engine) with Reveal/Copy, **Add Credential** manual modal, and **Request Login via Magic Link** (client-facing preview modal).
11. **Activity Log Tab:** Chronological audit trail of onboarding, file uploads, approvals, meetings, and alerts.

---

### 2.4 Reporting & Client Portal (`reporting.html`)
- **Dual Mode:** **Agency AM View** (editable Executive Summary) vs. **Client Portal (PWA)** (read-only client view).
- **Unified Channel-Agnostic Data Model:** Joins **SEMrush/DataForSEO**, **Search Console**, **GA4**, **Google/Meta Ads**, and **Production Tasks** by `Landing Page URL` and `Keyword / Topic` in PostgreSQL so clients see unified **Business Performance by Topic & Page** without raw "tool silos".
- **Mandatory Executive Summary:** Wins & Next Month's Focus added by AM before finalizing.
- **Data + Output View:** Connects completed production tasks (`4 Blogs Published → +400 sessions`) to performance metrics.
- **KPIs & Tables:**
  1. **Unified Business Performance by Topic & Page** (Merged Search + Ads + GA4 + Output)
  2. **GA4 Analytics — Traffic Acquisition by Channel**
  3. **Top Pages (GA4)**
  4. **Top Landing Pages (Conversions)**
  5. **SEO Keyword Rankings & Search Console Signals**
  6. **AI Visibility & GEO Report (30 Focused Prompts):** Google AI Overviews, ChatGPT, Perplexity & Gemini Share of Voice across Focused Keyword Areas vs. Competitors.

---

### 2.5 Retention, Health Score, Upsells & Offboarding (`upsells.html`)
- **Tabs:** `Health & Upsells` | `Files` | `Proposals` | `Offboarding / Cancellation`
- **Automated Client Health Score (RAG):** Red/Amber/Green indicator scored on Invoice Status, KPI Trends, Overdue Tasks, and Client Engagement (including missed meetings). `RED • ISSUE` triggers a **Recovery Playbook** & Slack alert.
- **AI Signal-Backed Upsell Engine:** Cross-references the unified reporting table (SQL + Gemini) to surface data-backed opportunities rather than generic missing-service flags (e.g., *High Organic Traffic + Low GA4 CVR → CRO Redesign*, *High-Converting Keyword at Pos #6–15 → PPC Bridge*, *Paid Search Traffic Leak without Retargeting → Paid Social*, *Competitor Winning AI Visibility → Digital PR / GEO*).
- **Files Tab:** Working upsell pitch decks, audits, and draft quotes before moving to Production.
- **Proposals Tab:** Full Upsell Proposals & E-Sign tracker (`Created`, `Sent`, `Sent • Viewed`, `Signed`) with **Create from Quote Builder** and **Manual Upsell Proposal**.
- **Structured Offboarding / Cancellation:** Tracks notice period end date, mandatory **Churn Reason** for leadership analytics, schedules final Xero reconciliation invoice, halts future recurring Production tasks, and provides an interactive Platform Access Revocation Checklist.

---

### 2.6 Users & Teams (`users.html`)
- Staff roster, roles, and client workspace assignments.

---

## 3. Agency Tools Suite

1. **Quote Builder (`quote-builder.html`):**
   - Interactive `£` GBP pricing calculator across Account Mgmt, SEO, Content & Email, Outreach / Digital PR, PPC (`15%` of spend), Paid Social (`20%` of spend), AI Search & GEO, and Web & Managed Hosting (`£150/mo`).
   - Uses **`Qty`** per campaign (e.g., 4 quarterly audits for a 12-month campaign), editable blended hourly rate, and displays **Selected Campaign Retainer** (`12 Month Campaign` default).
   - Supports `?client=` URL parameter from Prospecting/Upsells and flows directly into **Proposal & E-Sign** (`proposal-view.html`).
2. **Campaign Planner (`campaign-planner.html`):**
   - Client selector, Month filter (`M1–M4`), Auto-Generate from Quote, and **Send Tasks** button to push deliverables into Production Kanban boards.
3. **Rank Tracker & AI Visibility (`rank-tracker.html`):**
   - Client selector supporting **Setup Mode** (Target Domain, Location/Search Engine, Focused Keyword Areas, max **50 keywords**) and **Active Tracking Mode**:
     - **Google SERP Rankings:** 6-month average position chart + keyword table (Position, 30d Change, Search Volume, Ranking URL).
     - **AI Visibility & GEO (30 Focused Prompts):** Tracks brand mentions & citations across Google AI Overviews, ChatGPT, Perplexity, and Gemini strictly for the client's **Focused Keyword Areas** vs. their 3 competitors.
4. **MCP Setup (`mcp-setup.html`):**
   - Exposes ClientHub360 as a **Model Context Protocol (MCP)** data source (SSE endpoint + RBAC Bearer token) for **Antigravity, Claude, ChatGPT, and Cursor** to query pipeline deals, health scores, production profitability, and reporting metrics.
5. **External Link:** **DN Tool Suite** (`https://tools.digitalnext.co.uk`).
6. **Demo Notes (`demo-notes.html`):** Built-in walkthrough & spec reference.

---

## 4. Automated Slack Alert System
- Every active client is connected to a dedicated Slack channel (e.g., `#client-global-dynamics`).
- **Real-Time Alerts Fired by the Hub:**
  - Tasks marked complete / milestones delivered.
  - Connected accounts disconnecting or OAuth tokens expiring (e.g., Meta BM 7-day warning).
  - Website uptime ping failures (checked every 10 mins, 7am–7pm).
  - Placed backlinks returning `404` (checked every 7 days).
  - Content approvals (`Yes`/`No`), Proposal views/e-signatures, Xero overdue invoices, and Health Score drops to `AMBER` / `RED`.

---

## 5. Google Cloud Production Tech Stack & Cost Estimate

### 5.1 Recommended Google Cloud Architecture (`europe-west2` London)
| Layer | Google Cloud / Workspace Service | Purpose |
| :--- | :--- | :--- |
| **App, API & MCP Server** | **Cloud Run** (Serverless) + Custom Domain SSL | Hosts web app, REST APIs, Magic Link routes, PDF generator, and MCP SSE server. Scales to zero when idle. |
| **Database** | **Cloud SQL (PostgreSQL)** (`db-f1-micro` or `db-g1-small`) | Stores CRM deals, clients, tasks, quotes, proposals/e-sign audit trails, health scores, rankings, and meeting logs. |
| **Working Files** | **Google Workspace Shared Drives** (Drive API) | Stores working client folders (`/Sales-Prospects/` → `/Clients/<Name>/`), brand assets, audits, content drafts, and Gemini call notes. |
| **Locked / System Files** | **Cloud Storage (GCS Bucket)** | Immutable storage for signed SOW/MSA PDFs + SHA-256 audit certificates, report snapshots, and DB backups. |
| **Auth & Secrets** | **Firebase Auth / Identity Platform** + **Secret Manager** | Staff login via Google Workspace SSO + Client Portal auth; encrypted storage for OAuth refresh tokens and API keys. |
| **Background Jobs** | **Cloud Scheduler + Cloud Tasks** | 10-min website uptime pings (7am–7pm), 7-day backlink pings, weekly SERP & AI Visibility checks, 1st-of-month recurring tasks, and Slack alerts. |

### 5.2 Google & Third-Party APIs to Connect
- **Google APIs (Free within Workspace/GCP quotas):** GA4 Data API, Search Console API, Google Ads API, Merchant Center Content API, Google Calendar API, Google Meet REST API (`conferenceRecords`, `participants`, `transcripts`), Google Drive API, OAuth Consent Screen.
- **Rank Tracker & AI Visibility:** **DataForSEO** / **Serper.dev** (SERP + Google AI Overviews), **OpenAI / Perplexity / Gemini APIs** (30 focused prompts/client weekly).
- **Agency Stack:** **Ahrefs / SEMrush API**, **Meta Marketing API**, **LinkedIn Marketing API**, **Figma REST API**, **Xero API**, **Slack API**, **1Password / Bitwarden Vault API**, **Creditsafe / Companies House API**.

### 5.3 Estimated Monthly Running Cost (15 Staff, 30 Clients, 1 Report/Mo)
- **Google Cloud Hosting:**
  - *Lean (scales to zero):* **~\$12 – \$20 / month (~£10 – £16 / mo)**
  - *Recommended (1 warm Cloud Run instance + `db-g1-small` Postgres):* **~\$35 – \$45 / month (~£28 – £36 / mo)**
- **Magic Links:** **\$0** (self-hosted expiring tokens in PostgreSQL; email/Slack delivery free on standard tiers).
- **SERP & AI Visibility APIs (30 clients):**
  - DataForSEO weekly SERP + AIO checks: **~\$3 – \$5 / mo** (or ~\$25–\$40/mo daily).
  - LLM API calls (30 focused prompts/client weekly): **~\$5 – \$15 / mo** total.

### 5.4 Live On-Demand API Pull (No Raw Analytics Storage)
- **Analytics & Ads Data (GA4, Search Console, Google Ads, Meta Ads):** Do **not** replicate or store raw daily analytics in Cloud SQL. Pull live via API according to the date range selected at the top of Reporting (with a short 15–60 min cache) and join in memory by URL/Topic.
- **What Is Stored in PostgreSQL (< 1 GB total):**
  - Rank Tracker & AI Visibility weekly snapshots (historical timelines not kept by SERP/LLM APIs).
  - Finalized Monthly Report snapshots & AM Executive Summaries.
  - Weekly Health Score & AI Upsell Signal summaries.

---

## 6. Security & Compliance Architecture
1. **Zero Plain-Text Passwords in App DB:** Client CMS/FTP/hosting credentials collected via Onboarding or Vault Magic Links are transmitted over HTTPS directly to **1Password / Bitwarden Vault API** (or **Cloud Secret Manager**), with per-user permissions and audit logging on every *Reveal / Copy*.
2. **Hardened Magic Links:** Use **256-bit cryptographically random tokens** with strict expiration (e.g., 7 days), single-resource scope (e.g., a Content Approval token can only view/approve that single asset), and rate-limiting.
3. **Read-Only RBAC on MCP Server:** Every MCP Bearer token enforces per-user RBAC (only assigned clients), read-only queries (no destructive writes), and **never** exposes the `Passwords` vault.
4. **Staff SSO, Encrypted Tokens & Private Buckets:** Enforce **Google Workspace SSO + 2FA** for staff; encrypt client OAuth refresh tokens with **Cloud KMS / Secret Manager**; keep the GCS Bucket **100% private** (serving signed contract PDFs only via short-lived signed URLs) and validate all file uploads.
