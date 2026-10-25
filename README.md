# GST Reconciliation Pro — Books vs GSTR-2B Input Tax Credit (ITC) Automation Engine

An enterprise-grade statutory automation tool designed for Indian Chartered Accountants, Tax Consultants, and Corporate Finance Teams to reconcile **Input Tax Credit (ITC) claimed in Books of Accounts** against **auto-drafted GSTR-2B statements** for a full financial year or specific periods under **Section 16(2)(aa)** and **Rule 37 of the CGST Act, 2017**.

---

## 🚀 Key Highlights & Capabilities

### 1. Multi-Register Books Ingestion
Handles the exact accounting records mandated by corporate audits:
- **Purchase Register**: All supplier invoices with GSTIN, Invoice No, Invoice Date, Taxable Value, IGST, CGST, SGST, Cess, Total Value, Place of Supply, and RCM.
- **Journal Register**: Statutory ITC adjustments, Rule 42/43 turnover reversals (exempt supplies), Section 17(5) blocked credit reversals, and RCM liability availment.
- **Debit Note Register**: Post-purchase rate differences, material returns, and commercial adjustments.
- **Payment Register (Rule 37 180-Day Rule)**: Correlates invoice dates and clearance dates to flag **unpaid invoices older than 180 days** requiring mandatory reversal in GSTR-3B with Section 50 interest calculations.

*Supports single multi-sheet workbooks (sheets named "Purchase", "Journal", "DebitNotes", "Payments") or individual file uploads in Excel (`.xlsx`, `.xls`) or CSV format with intelligent column header auto-detection.*

### 2. Full-Year GSTR-2B Ingestion
- Upload the official consolidated full-year GSTR-2B Excel downloaded from the GST Portal (with sheets: `B2B`, `B2BA`, `CDNR`, `CDNRA`, `ISD`, `IMPG`).
- Or multi-select all 12 monthly Excel / JSON files (April to March) — the parser merges and normalizes them automatically.

### 3. Multi-Tier Reconciliation & Difference Categorization
- **Exact Match (Clean ITC)**: 100% match on normalized GSTIN, normalized invoice number (stripping symbols `/`, `-`, `_`, leading zeroes), and tax values within tolerance (default ± ₹1.00).
- **Probable / Fuzzy Match**: Matches where invoice numbers contain minor typos or formatting variations (e.g. `TSL/23-24/00841` in Books vs `TSL2324841` in 2B) using Levenshtein distance algorithms.
- **Value Mismatches**: Flags exact tax differences (`Books Tax > 2B Tax` excess claimed risk vs `Books Tax < 2B Tax` under-claimed credit).
- **Tax Head Mismatches**: Flags Place of Supply (POS) errors where IGST was claimed in Books but CGST+SGST was uploaded in GSTR-2B (or vice versa).
- **Missing in 2B (High Audit Risk)**: Invoices present in Books but missing from GSTR-2B. Calculates financial tax exposure under Section 16(2)(aa).
- **Missing in Books (Unclaimed Opportunity)**: Invoices present in GSTR-2B but missed in Books — direct cash flow saving opportunity before Section 16(4) time-limit.
- **Rule 37 (180-Day Non-Payment Check)**: Unpaid invoices > 180 days flagged with 18% p.a. statutory interest liability.
- **Ineligible ITC in 2B**: Flags supplies where supplier marked POS out-of-state for intra-state supplies or supplier cancelled.

### 4. Interactive Dashboard & Visual Analytics
- Obsidian dark glassmorphic interface with radiant emerald, amber, rose, and cyan indicator pills.
- Chart.js interactive Donut chart showing ITC category distribution.
- Chart.js monthly trend bar chart comparing Books claimed vs GSTR-2B available month-by-month.
- Real-time search across GSTIN, Supplier Name, and Invoice Number.
- Vendor dropdown filter for quick supplier drill-down.

### 5. Automated Supplier Notice Generator
- 1-click generation of legally compliant formal notices under Section 16(2)(aa) citing specific missing invoices, dates, and tax at risk.
- Instant "Copy Email Body" and "Send via WhatsApp" shortcuts.

### 6. Excel & Template Export
- Download comprehensive multi-sheet Excel (`.xlsx`) workbook:
  - `Executive_Summary`: Complete reconciliation statement and compliance certificate.
  - `Matched_Invoices`: Clean matched items.
  - `Missing_in_2B_Risk`: High-risk items for supplier follow-up.
  - `Missing_in_Books_Unclaimed`: Cash saving opportunities.
  - `Value_Mismatches`: Side-by-side tax difference analysis.
  - `Rule37_180Days_Risk`: Non-payment exposure list.
  - `Vendor_Dispute_Summary`: Supplier-level aggregated discrepancy tracker.
- Built-in "Download Template Format" for accounting teams.

---

## 📂 Project Structure

```
gst-recon-tool/
├── index.html              # Main Single-Page Web Application
├── run-app.bat             # 1-Click Windows Batch Launcher
├── server.ps1              # Lightweight zero-dependency PowerShell HTTP server
├── styles/
│   └── main.css            # Dark glassmorphic design system & typography
├── scripts/
│   ├── demo-data.js        # Full-year FY 2023-24 realistic dataset (60+ transactions)
│   ├── parser.js           # Multi-sheet Excel, CSV, & GSTN JSON ingestion engine
│   ├── reconciler.js       # Multi-pass reconciliation & fuzzy matching engine
│   ├── exporter.js         # Multi-sheet XLSX export & template generator
│   └── app.js              # Application state, UI controller, charts & modals
└── vendor/
    ├── xlsx.full.min.js    # SheetJS offline library (Excel parsing & export)
    ├── chart.umd.min.js    # Chart.js offline library (Interactive data visualization)
    └── lucide.min.js       # Lucide vector icons
```

---

## ⚡ How to Run

1. **Option 1 (Direct Double-Click)**:
   Double-click `run-app.bat`. It will instantly launch the tool in your default web browser (Edge/Chrome).

2. **Option 2 (Direct File Open)**:
   Simply open `index.html` in Microsoft Edge or Google Chrome. All libraries and styles are 100% self-contained and run locally offline with zero dependencies!

3. **Option 3 (PowerShell Server)**:
   Run PowerShell in the directory:
   ```powershell
   .\server.ps1 -Port 8085
   ```
   Navigate to `http://localhost:8085/`.

---

## 🧪 Testing with Sample Data
Click the **"Load Sample FY 23-24"** button on the top navigation bar. It instantly loads a comprehensive, real-world full-year test dataset including exact matches, round-off differences, invoice number variations, missing GSTR-1 filings, unclaimed ITC, Rule 37 180-day overdue payments, and journal reversals.
