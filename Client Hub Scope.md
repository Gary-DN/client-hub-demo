### Client Hub 360 - Complete Platform Scope
A comprehensive 360-degree agency management platform handling the entire client journey from Prospecting to Cancellation, integrating CRM, Project Management, and Client Reporting.

--------------------------------------------------------------------------------

#### 1. Prospecting / Pipeline & Commercial Handoff
**Views Available:**  Kanban View, List View
**Inbound to the System:**
*  Manual Entry
*  Via Website Forms / Lead Capture
*   **Deal Hygiene:**  Lead Source (Attribution), Assigned Sales Rep, Deal Probability (%), and Closed-Lost Reason (Budget, Timing, Competitor).
**Prospect Client Profile:**
*  Name & Business Name
*  Phone, Email, URL
*  Status: Spoken To / Discovery Complete / Meeting Booked / Proposal Sent / Active
*  Services Discussed
*  Proposed Budgets
*  Days in Prospecting
*   **Commercial Terms:**  Contract Term (e.g., 6-month, rolling), Billing Cycle, and Sales Handover Notes to prevent scope creep.
**Integrations & Handoff:**
*   **Client Enrichment:**  Automated data gathering for finance.
*   **PandaDoc / Xero Integration:**  Syncing proposals and invoices.
*   **Mandatory Sign-Off Check:**   **Required file upload field for the final written, signed agreement**  (Satisfies management compliance before a client can be moved to active).
*   **Automated Stage Change:**  When the signed agreement is uploaded, PandaDoc proposal is signed, and the first Xero invoice/deposit is paid, the system automatically flips the client status to  *Onboarding* .

--------------------------------------------------------------------------------

#### 2. Onboarding & Kickoff
**Live Client Creation Profile:**
*  Name, Business Name, Phone, Email, URL
*  Status: Active / Issue / Cancelled
*  Services Subscribed & Live Budgets
*  Days as a Live Client
**Asset & Credential Collection:**
*   **Client-Facing "Magic Link" Wizard:**  A branded, passwordless form sent to the client that dynamically asks  *only*  for the credentials required for their specific services.
*   **Website Logins & Vault:**  Integrated with an encrypted vault API (like 1Password/Bitwarden) for secure storage.
*  Google Access: GA4, GTM, Search Console, Google Ads ID.
*  Social & Ads: Social Links, Meta Business Manager ID, LinkedIn/TikTok Ad IDs.
*  Design & Content: Hosting/DNS/Staging Access, Brand Guidelines, Asset Folders.
*  Competitors list as urls.
*  Slack Handle (for channel automation and alerts).
**Progression to Production:**
*  Conditional required fields based on the specific services sold.
*  Manual "Verify Access & Launch" button for the Account Manager to test logins before work begins.

--------------------------------------------------------------------------------

#### 3. Production & Operations
**Overview Page (Per Client):**
*   **File Storage:**  Dedicated directory with links to shared drives.
*   **Service Tabs:**  A dedicated tab for each active service.
*   **Kanban To-Do Board:**  Auto-populated by the budget, services, and month of the campaign. Each task includes an  **Estimated Time/Credit value**  to track Profitability (Allocated Hours vs. Logged Hours).
**Department-Specific Workflows:**
*   **SEO:**
    *  Attach audit reports, links built, and critical docs.
    *  Month 1: Competitor Analysis (Automated), Website Audit, 12-Month Work Plan (Semi-automated based on budget/services), One-Click Generate Work Plan.
    *  Months 2-12: Recurring task engine populating the first of every month.
*   **Content (Client Approval Loop):**
    *   **Direct Approval Links:**  One-click links sent to the client via email/Slack for blogs, copy, or ad creatives. The client views the asset and clicks  **"Yes (Approve)"**  or  **"No (Reject/Revise)"** . Timestamps and feedback are logged directly back into the Production board.
*   **PPC & Paid Social:**  Campaign build trackers, pixel QA checklists, ad creative loops, and live budget pacing alerts.
*   **Design & Dev:**  Bug/ticket intake, revision round counters, and pre-launch QA checklists.

--------------------------------------------------------------------------------

#### 4. Reporting & Client Portal
**Connectors & Data:**
*  Analytics Connector (GA4)
*  Search Console Connector
*  SEO Tools: Keyword ranking tool, Ahrefs Connector, SEMrush Connector.
*   **Paid Media Connectors:**  Google Ads, Meta Ads, LinkedIn Ads (Live Ad Spend, CPA, ROAS).
**Client Experience:**
*  Client login portal exclusively for reports and approvals.
*   **Insights Dashboard:**  Pages combining links, analytics, keywords, impressions, and the signals that connect them.
*   **Data + Output View:**  Displays completed production tasks (e.g., "4 blogs published") alongside the performance metrics to prove ROI.
*   **Executive Summary Block:**  Mandatory text box for the Account Manager to add human context (Wins, Next Month's Focus) before reports are finalized.
*  Automated scheduled reports and alerts.
*   **Mobile Strategy:**  A mobile-responsive Progressive Web App (PWA) rather than a native iOS/Android app. Real-time alerts (spend anomalies, ranking shifts) pushed via Email, Slack, or WhatsApp/SMS.

--------------------------------------------------------------------------------

#### 5. Retention, Upsells & Offboarding (Lifecycle Management)
*   **Automated Client Health Score:**  A Red/Amber/Green indicator based on invoice status, KPI trends, overdue tasks, and client engagement. Triggers a recovery playbook if marked "Issue".
*   **Cross-Sell / Upsell Triggers:**  Automated flagging at 90 days for single-service clients to pitch complementary services (e.g., pitching PPC to an SEO client).
*   **Structured Offboarding / Cancellation:**
    *  Tracks notice period end date.
    *  Schedules final Xero reconciliation invoice.
    *  Halts future recurring Production tasks.
    *  Triggers checklist for revoking agency access to client platforms.
    *  Logs a mandatory "Churn Reason" for leadership analytics.
