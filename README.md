# Spencel

> **Modern Personal & Household Finance, Smart Pantry Management, Receipt OCR, and Fair Expense Settlement.**

Spencel is an edge-native mobile and cloud platform engineered to simplify daily living. Designed with a **solo-first** philosophy and extensible to couples, roommates, and multi-member households, Spencel eliminates friction in expense tracking, pantry inventory, cook-from-stock recipe discovery, receipt line-item breakdown, bank statement parsing, and financial reporting.

---

## 🌟 Key Capabilities

### 1. 🧾 Smart Receipt Capture & Ingestion
- **Four Ingestion Channels**: Real-time native iPhone camera capture, digital document upload (PDF/PNG), auto-forwarded email parsing (`alex.spencel@receipts.spencel.app`), and manual entry.
- **Automated Line-Item Breakdown**: Extracts vendors, dates, tax, tip, and items.
- **Pantry Matching**: Scanned grocery receipt items automatically match against active pantry inventory and update stock counts.
- **Itemized Expense Splitting**: Assign individual receipt items to specific household members or split evenly.

### 2. 🏦 Bank Statement Ingestion & Financial Ledger
- **Statement Import**: Upload monthly **PDF or CSV** bank statements for automated offline transaction extraction.
- **Multi-Account Overview**: Supports checking, savings, and credit accounts with auto-sync or manual balancing.
- **Budgeting & Gap Analysis**: Dynamic category caps, visual spending rings, and smart savings insights.
- **Subscription Tracking**: Monitors recurring services, renewal dates, and impending renewal alerts.

### 3. 📊 Reports & Financial Auditing
- **Monthly Financial Reports**: High-level spend analytics, income vs. expense breakdowns, and category distributions.
- **Export Capabilities**: Generates downloadable PDF/CSV audit reports stored securely in Cloudflare R2.

### 4. 🥑 Zero-Waste Smart Pantry & Recipes
- **Storage Zone Management**: Tracks food across `fridge`, `freezer`, and `cupboard`.
- **Perishability Warnings**: Visual shelf-life indicator (`fresh`, `soon`, `expired`) with days-left countdowns.
- **Predictive Replenishment**: Machine-learned consumption patterns suggest staple re-orders before items run out.
- **Cook-from-Stock Recipe Matching**: Real-time recipe engine ranks dishes by pantry stock match percentage (e.g. *100% Stocked*).

### 5. 🤝 Fair Household Settlement
- **Flexible Living Modes**: Default solo-first mode toggles seamlessly into shared household spaces (Couples, Roommates, Family).
- **Settlement Matrix**: Mathematical netting engine minimizes peer-to-peer transfers into simplified single-payment IOUs.

---

## 🏗️ System Architecture

Spencel operates on a **100% Cloudflare Edge-Native Architecture** coupled with a high-performance **React Native (Expo SDK 54)** mobile client:

```text
┌─────────────────────────────────────────────────────────────────────────┐
│                       Cloudflare Global Edge Network                    │
│                                                                         │
│  [ Spencel Mobile Client ] (React Native & Expo SDK 54 on iOS / Android)│
│             │                                                           │
│             ▼                                                           │
│  [ Cloudflare Workers: spencel-api ] (FastAPI Python via Pyodide)       │
│             ├───► [ Cloudflare D1: spencel-db ] (Serverless SQLite)    │
│             ├───► [ Cloudflare R2: spencel-media ] (Public CDN Assets)  │
│             └───► [ Cloudflare R2: spencel-vault ] (Private Financial) │
│                         ▲                                               │
│                         │                                               │
│  [ Cloudflare Workers: spencel-processor ] (Async OCR / Statements / PDF)
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 📁 Repository Structure

```text
Spencel/
├── README.md                            # Main project overview (this file)
├── .gitignore                           # Root Git ignore rules
│
├── frontend/                            # React Native & Expo Mobile Client (ARCHITECTURE.md inside)
│   ├── App.tsx                          # Root navigator & state provider
│   ├── babel.config.js                  # Babel configuration with babel-preset-expo
│   ├── package.json                     # Dependencies (Expo SDK 54.0.37, React Native 0.81.5)
│   ├── SPENCEL_DESIGN_SYSTEM.md         # Design system, color tokens & typography
│   └── src/
│       ├── api/                         # Backend client SDK & offline mock fallbacks
│       ├── components/                  # Domain cards, lists, banners, and modals
│       ├── design-system/               # Buttons, Badges, Headers, BrandLogos, Inputs
│       ├── screens/                     # Application screens grouped by domain:
│       │   ├── auth/                    # Login & authentication
│       │   ├── finance/                 # Statements, reports, budgets, subscriptions, banks
│       │   ├── grocery/                 # Shopping list & staple re-orders
│       │   ├── home/                    # Executive summary & quick actions
│       │   ├── pantry/                  # Food inventory & shelf-life tracking
│       │   ├── receipts/                # Camera capture, scan inbox, reconciliation
│       │   ├── recipes/                 # Cook-from-pantry recipe matching
│       │   ├── settings/                # Household mode, preferences, members
│       │   └── settle/                  # IOU ledger & debt settlement
│       └── theme/                       # Colors, radii, spacing, typography scales
│
└── backend/                             # FastAPI Python Server & Cloudflare Workers
    ├── run.py                           # Local development entrypoint (Uvicorn)
    ├── schema.sql                       # 17-table Cloudflare D1 / SQLite database schema
    ├── wrangler.toml                    # Cloudflare Workers, D1, and R2 bindings
    ├── requirements.txt                 # Python dependencies
    └── app/
        ├── main.py                      # FastAPI app initialization & CORS
        ├── core/                        # Config, database engine, security
        ├── db/                          # SQLAlchemy models & auto-seeding mock datasets
        ├── schemas/                     # Pydantic v2 validation models
        ├── services/                    # Business logic (OCR, settlement, recipe matching)
        └── api/v1/                      # Modular REST endpoints
```

---

## 🗄️ Cloudflare Infrastructure Specification

### 1. Cloudflare D1 Database (`spencel-db`)
- 17 structured SQLite tables defined in [`backend/schema.sql`](file:///c:/Users/Admin/OneDrive/Desktop/Spencel/backend/schema.sql):
  - **Identity**: `users`, `households`, `household_members`
  - **Ledger**: `transactions`, `budget_categories`, `subscriptions`, `bank_accounts`, `ious`
  - **Documents**: `receipts`, `receipt_line_items`, `bank_statements`, `generated_reports`
  - **Pantry & Food**: `pantry_items`, `replenish_suggestions`, `grocery_items`, `recipes`, `recipe_ingredients`

### 2. Cloudflare R2 Storage Buckets
Spencel isolates public user media from sensitive financial records:

- **`spencel-media`** (Public CDN):
  - `avatars/` — User profiles and household member avatars
  - `grocery_images/` — Pantry and grocery inventory item catalog
  - `logos/` — Merchant and bank brand logos (rendered by `BrandLogo.tsx`)
  - `receipts/` — Captured store receipts and scanned photos
  - `recipes/` — Hero cooked dish recipe photos
- **`spencel-vault`** (Strictly Private - Signed URLs only):
  - `statements/` — Uploaded bank statement PDFs and CSVs
  - `reports/` — Monthly financial audit reports and settlement summary PDFs

### 3. Cloudflare Workers
- **`spencel-api`**: Primary FastAPI application worker providing low-latency REST endpoints.
- **`spencel-processor`**: Background queue worker handling PDF/CSV statement parsing, OCR receipt line-item matching, and monthly report compilation.

---

## 🚀 Getting Started

### Prerequisites
- Node.js `v18+` or `v20+`
- Python `3.10+`
- Expo Go app on your physical iPhone (or Android device)
- Cloudflare Wrangler CLI (`npm install -g wrangler`)

---

### Local Frontend Development

1. Navigate to the frontend directory:
   ```bash
   cd frontend
   npm install
   ```
2. Start the Expo development server:
   ```bash
   npx expo start -c
   ```
3. Open **Expo Go** on your iPhone and scan the generated QR code.

---

### Local Backend Development

1. Navigate to the backend directory:
   ```bash
   cd backend
   pip install -r requirements.txt
   ```
2. Run the local development server (with SQLite auto-seeding):
   ```bash
   python run.py
   ```
3. Interactive API documentation is available at:
   - **Swagger UI**: [http://localhost:8000/docs](http://localhost:8000/docs)
   - **ReDoc**: [http://localhost:8000/redoc](http://localhost:8000/redoc)

---

### Cloudflare Deployment

```bash
# 1. Authenticate with Cloudflare
npx wrangler login

# 2. Provision D1 Database & Execute Schema
npx wrangler d1 create spencel-db
npx wrangler d1 execute spencel-db --file=./schema.sql --remote

# 3. Provision R2 Storage Buckets
npx wrangler r2 bucket create spencel-media
npx wrangler r2 bucket create spencel-vault

# 4. Deploy the FastAPI Worker
cd backend
npx wrangler deploy
```

---

## 🧪 Testing & Verification

| Target | Command | Purpose |
|---|---|---|
| **Frontend Types** | `npx tsc --noEmit` (in `frontend/`) | TypeScript strict type checking |
| **Frontend Health** | `npx expo-doctor` (in `frontend/`) | Expo SDK 54 dependency alignment |
| **Backend Suite** | `python tests/run_tests.py` (in `backend/`) | Integration test suite (14 endpoints) |
| **Code Formatting** | `npx eslint .` / `flake8` | Code style enforcement |

---

## 📄 License & Attribution

Designed and engineered for Spencel. All rights reserved.
