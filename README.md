# Finance Control

A client-side personal finance management application built as a single HTML file with optional cloud sync via Supabase. Designed to track recurring bills, credit accounts, and payment history — with no backend required for local use.

**Live app:** [lloammes81.github.io/fab-control](https://lloammes81.github.io/fab-control/)

---

## Features

### Dashboard
- KPI summary cards showing total monthly bills, paid amount, pending balance, and days to next due date
- Upcoming bill due dates with status indicators (overdue / due soon / current)
- Credit account overview with total debt, credit utilization percentage, average APR, and monthly payments

### Bills Management
- Create and edit recurring bills with name, amount, due day, payment method, currency, category, and vendor
- Mark bills as paid with a custom date, amount, and optional note
- Filter bills by status (overdue, due soon, current) and search by name
- Per-bill payment history modal with total paid and CSV export

### Credit Accounts
- Three account types: credit card, loan, and service/repair
- Track credit limit, current balance, APR, and minimum payment
- Log payments with running balance tracking after each payment
- Credit utilization indicator with visual progress bar (safe / warning / danger thresholds)
- Per-account payment history

### Reports
- Filterable by period: current month, last 3 months, last 6 months, full year, or all time
- Summary KPIs: total paid on bills, total paid on credit, grand total, and remaining debt
- Detailed payment tables for bills and credit accounts
- One-click CSV export of the full report

### Data Layer
- **Local mode:** all data persisted in the browser via IndexedDB — no account or internet connection required
- **Cloud mode:** optional Supabase integration for real-time sync across devices; credentials stored in `localStorage`
- Supports both modes simultaneously with graceful fallback

---

## Tech Stack

| Layer | Technology |
|---|---|
| UI | Vanilla HTML / CSS / JavaScript (ES Modules) |
| Storage — local | IndexedDB (`fc_v3`) |
| Storage — cloud | [Supabase](https://supabase.com) (PostgreSQL + REST API) |
| Supabase client | `@supabase/supabase-js v2` via `esm.sh` CDN |
| Fonts | DM Mono · Cabinet Grotesk (Google Fonts) |
| PWA | Web App Manifest + `apple-mobile-web-app-capable` |
| Deployment | GitHub Pages |

---

## Database Setup (Supabase — optional)

To enable cloud sync, create the following four tables in your Supabase project's SQL Editor:

```sql
-- Bills
CREATE TABLE bills (
  id TEXT PRIMARY KEY,
  name TEXT, amount NUMERIC, due_day INT,
  method TEXT, currency TEXT, category TEXT, vendor TEXT,
  created_at TIMESTAMPTZ DEFAULT now()
);

-- Bill payments
CREATE TABLE payments (
  id TEXT PRIMARY KEY,
  bill_id TEXT, pay_date DATE, amount NUMERIC, note TEXT,
  created_at TIMESTAMPTZ DEFAULT now()
);

-- Credit accounts
CREATE TABLE credit_accounts (
  id TEXT PRIMARY KEY,
  name TEXT, type TEXT, balance NUMERIC, credit_limit NUMERIC,
  apr NUMERIC, min_payment NUMERIC,
  created_at TIMESTAMPTZ DEFAULT now()
);

-- Credit payments
CREATE TABLE credit_payments (
  id TEXT PRIMARY KEY,
  account_id TEXT, pay_date DATE, amount NUMERIC,
  balance_after NUMERIC, note TEXT,
  created_at TIMESTAMPTZ DEFAULT now()
);

-- Disable RLS on all tables
ALTER TABLE bills DISABLE ROW LEVEL SECURITY;
ALTER TABLE payments DISABLE ROW LEVEL SECURITY;
ALTER TABLE credit_accounts DISABLE ROW LEVEL SECURITY;
ALTER TABLE credit_payments DISABLE ROW LEVEL SECURITY;
```

Then open the app, click the **Supabase** button in the top bar, enter your Project URL and Anon Key, and click **Test Connection**.

---

## Local Development

No build step required. Open `index.html` directly in a browser or serve it with any static file server:

```bash
npx serve .
# or
python3 -m http.server 8080
```

---

## Repository Structure

```
fab-control/
├── index.html        # Full application (self-contained)
├── manifest.json     # PWA manifest
├── icon.svg          # Scalable app icon
├── icon-192.png      # PWA icon (192×192)
└── icon-512.png      # PWA icon (512×512)
```

---

## License

MIT
