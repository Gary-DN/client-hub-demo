# Google Cloud Setup Guide — ClientHub 360

This guide walks through everything you need to set up in **Google Cloud (GCP)** so we can deploy and run the production **ClientHub 360** app (`europe-west2` London).

---

## 1. Will We Use SSH Keys?

**No — you do not need SSH keys.**  
SSH keys are only used for logging into raw virtual machines (Compute Engine VMs). Because ClientHub 360 runs on **serverless Google Cloud Run** (plus Cloud SQL, Cloud Storage, and Secret Manager), deployment uses **Google Cloud IAM** instead of SSH:

- **Option A (Easiest & Safest for Local Deployments):** You install the **Google Cloud CLI (`gcloud`)** on your Windows machine, run `gcloud auth login` with your Google account, and I run the `gcloud` deploy commands right from this workspace. Nothing sensitive is stored in files.
- **Option B (Scoped Service Account Key):** You create a dedicated **Service Account** (`client-hub-deployer`) with only the permissions needed to deploy the app, download its **JSON key file** to a folder **outside** this Git repo (so it is never pushed to GitHub), and point `gcloud` / me to that key. You can revoke or delete the key in 1 click at any time.
- **Option C (Automated CI/CD later):** Connect GitHub Actions to Cloud Run via **Workload Identity Federation** (zero static keys at all — pushing to `main` auto-deploys).

---

## 2. Step-by-Step Google Cloud Setup (~15 Minutes)

### Step 1: Create the Project & Set a Budget Alert
1. Go to [console.cloud.google.com](https://console.cloud.google.com).
2. Click the **Project dropdown** (top bar) → **New Project**:
   - **Project Name:** `DNXT Client Hub`
   - **Project ID:** e.g. `dnxt-client-hub-prod` *(note this down)*
3. Link your **Billing Account** when prompted.
4. Go to **Billing → Budgets & alerts** and create a monthly budget alert (e.g. **£40 / month** with alerts at 50%, 90%, 100%) so there are never surprise bills.

---

### Step 2: Enable Required Google Cloud APIs
In the Cloud Console (**APIs & Services → Library**) — or in Cloud Shell / `gcloud` — enable these APIs:

```bash
gcloud config set project dnxt-client-hub-prod

gcloud services enable \
  run.googleapis.com \
  cloudbuild.googleapis.com \
  artifactregistry.googleapis.com \
  sqladmin.googleapis.com \
  storage.googleapis.com \
  secretmanager.googleapis.com \
  identitytoolkit.googleapis.com \
  cloudscheduler.googleapis.com \
  cloudtasks.googleapis.com
```

---

### Step 3: Give Me / Your Machine Deployment Access

Choose **Option A** (interactive `gcloud` login) or **Option B** (Service Account JSON):

#### Option A: `gcloud` CLI Login (Recommended)
1. Install the Google Cloud CLI on Windows: [cloud.google.com/sdk/docs/install](https://cloud.google.com/sdk/docs/install).
2. Open PowerShell and run:
   ```powershell
   gcloud auth login
   gcloud config set project dnxt-client-hub-prod
   gcloud config set run/region europe-west2
   ```
3. That's it — once you are signed in, tell me your **Project ID** (`dnxt-client-hub-prod`) and I can build, push, and deploy directly.

#### Option B: Scoped Service Account (No Personal Login Shared)
1. Go to **IAM & Admin → Service Accounts → Create Service Account**.
2. Name it `client-hub-deployer`.
3. Grant these scoped roles (no Billing Admin or Org Admin needed):
   - **Cloud Run Admin** (`roles/run.admin`)
   - **Cloud Build Editor** (`roles/cloudbuild.builds.editor`)
   - **Artifact Registry Writer** (`roles/artifactregistry.writer`)
   - **Cloud SQL Admin** (`roles/cloudsql.admin`)
   - **Storage Admin** (`roles/storage.admin`)
   - **Secret Manager Admin** (`roles/secretmanager.admin`)
   - **Service Account User** (`roles/iam.serviceAccountUser`)
4. Click the newly created service account → **Keys tab → Add Key → Create new key → JSON**.
5. Save the `.json` file somewhere **outside** your Git repo (e.g. `C:\Users\GaryDouglas\.gcp\dnxt-client-hub-deployer.json`).
   > **CRITICAL:** Never put this `.json` file inside the `Client-Hub` repo or commit it to GitHub.

---

### Step 4: Set Up Digital Next Staff SSO (Google Workspace)
Because all Digital Next staff will log in via **SSO**:
1. Go to **Identity Platform** (or **Firebase Authentication**) in Google Cloud Console and click **Enable**.
2. Under **Providers → Add a provider**, select **Google**.
3. Configure your **OAuth Consent Screen** (**APIs & Services → OAuth consent screen**):
   - Set **User Type** to **Internal** (if all staff are on your Google Workspace domain, e.g. `@digitalnext.co.uk`) so only Digital Next staff can sign in.
   - *(Note: We will later add External/client-facing OAuth scopes separately for GA4, Search Console, Ads, and Drive connectors, or use a dedicated OAuth app for client connectors.)*

---

### Step 5: Core Resources We Will Provision (I Can Run These Once Connected)
Once you give me the **Project ID** and access (Option A or B), I can create these for you via `gcloud`:
1. **Cloud SQL (PostgreSQL 16)** in `europe-west2` (`db-f1-micro` or `db-g1-small`, automated daily backups enabled).
2. **Private GCS Bucket** (`dnxt-client-hub-contracts-prod` in `europe-west2`) with **Public Access Prevention = Enforced** for locked signed SOW/MSA PDFs.
3. **Cloud Run Service** (`dnxt-client-hub`) in `europe-west2` + custom domain mapping (e.g., `hub.digitalnext.co.uk`).
4. **Cloud Scheduler Jobs** for 10-minute website uptime pings (7am–7pm), 7-day backlink checks, weekly SERP/AI Visibility snapshots, and 1st-of-month recurring tasks.

---

## 3. Third-Party API Keys (Can Be Added Incrementally After Deploy)
Once the core app is live on Cloud Run, we'll store these in **Google Secret Manager** (never in code):
- **Google APIs (same GCP project):** GA4 Data API, Search Console API, Google Ads API, Merchant Center Content API, Calendar & Meet REST API, Drive API.
- **Agency & Finance:** Xero OAuth Client ID/Secret, Slack Bot Token & Webhook, 1Password / Bitwarden Vault API token, Creditsafe / Companies House API key.
- **SEO / AI / Design:** DataForSEO (or Serper.dev), Ahrefs / SEMrush API, Gemini / OpenAI / Perplexity API keys, Figma Personal/OAuth Token, Meta & LinkedIn Marketing APIs.

---

## 4. Quick Checklist to Get Started
When you're ready for me to upload and deploy:
1. Create the GCP project (`dnxt-client-hub-prod`) and enable billing + budget alert.
2. Enable the 9 APIs listed in Step 2.
3. Either run `gcloud auth login` on your machine (**Option A**) or save the `client-hub-deployer` JSON key outside the repo (**Option B**).
4. Reply with your **Project ID** (and key path if using Option B), and we can start deploying!
