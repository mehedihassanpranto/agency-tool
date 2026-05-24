# AdLedger — Media Buying Agency Finance Tracking Tool

## Product Goal
AdLedger is an internal web app for media buying agencies to replace spreadsheet-based finance tracking with a ledger-first system.

It tracks:
- Client payments
- USD allocation
- Service charges
- Ad account balances
- Ad spend
- Spending limits
- Remaining client balance
- Profit/loss
- Invoices and reports

**Core principle:** all balances are derived from immutable transaction records. No manually stored “remaining balance” fields.

---

## MVP Scope

### Dashboard
Display:
- Total clients
- Total active ad accounts
- Total client deposits
- Total ad spend
- Total service charge earned
- Total USD sold
- Total remaining client balance
- Accounts near spending limit

### Client Management
Track:
- Client name
- Business name
- Phone/email
- Payment type
- Notes
- Status

### Ad Account Management
Track:
- Meta ad account ID
- Assigned client
- Spending limit
- Current spend (derived)
- Remaining limit (derived)
- Status

### Transactions
Support transaction types:
- `DEPOSIT`
- `SERVICE_FEE`
- `AD_BUDGET_ALLOCATED`
- `AD_SPEND`
- `REFUND`
- `ADJUSTMENT`

---

## MVP Architecture

### Tech Stack
- **Frontend:** Next.js (App Router), TypeScript, Tailwind CSS, shadcn/ui
- **Backend:** Node.js, Prisma ORM, Zod
- **Database:** PostgreSQL
- **Charts/Reports:** Recharts
- **Auth:** Clerk or NextAuth

### Recommended Modules
1. Clients
2. Ad Accounts
3. Transactions
4. Spend Entries
5. Dashboard
6. Invoices

### Main Routes
- `/login`
- `/dashboard`
- `/clients`
- `/clients/[id]`
- `/ad-accounts`
- `/transactions`
- `/spend`
- `/invoices`
- `/reports`
- `/settings`

---

## Data Model (Initial)

### Core Tables
- `users`
- `clients`
- `ad_accounts`
- `transactions`
- `invoices`
- `invoice_items`

### Suggested Transaction Shape
Each transaction should include:
- `id`
- `client_id`
- `ad_account_id` (nullable for client-level entries)
- `type` (enum)
- `amount`
- `currency`
- `fx_rate` (nullable)
- `notes`
- `created_by`
- `created_at`

---

## Financial Logic

### Non-Negotiable Rule
Never manually store client/ad account remaining balance as source-of-truth. Always derive from ledger entries.

### Formulas
- **Client Remaining Balance** = Total Ad Budget Allocated − Total Ad Spend
- **Agency Revenue** = Service Fee + USD Markup Profit
- **Ad Account Remaining Limit** = Spending Limit − Current Spend

---

## MVP Milestones (5 Weeks)

### Week 1
- Project setup
- Authentication setup
- Database schema

### Week 2
- Clients CRUD
- Ad Accounts CRUD
- Transactions CRUD

### Week 3
- Dashboard
- Calculation services
- Client financial summaries

### Week 4
- Invoice generation
- Reports
- CSV export

### Week 5
- Bug fixing
- Roles/permissions
- UI improvements

---

## Build Acceptance Questions
The MVP is successful if it clearly answers:
1. How much did the client pay?
2. How much is service charge?
3. How much ad budget did they receive?
4. How much has been spent?
5. How much balance is left?

---

## Implementation Starter Prompt

> Build a Next.js App Router web app for a media buying agency finance tracker.
> Tech stack: Next.js, TypeScript, PostgreSQL, Prisma, Tailwind CSS, shadcn/ui, Zod.
> Core modules: Clients, Ad Accounts, Transactions, Spend Entries, Dashboard, Invoices.
> Rules: use ledger-based accounting; do not store remaining balance manually; calculate balances from transaction records.

