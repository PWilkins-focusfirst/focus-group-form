# focus-group-form

FocusFirst operations &amp; accounting workspace — a set of static, dependency-free
HTML pages that run entirely in the browser (data is stored on-device in
localStorage; nothing is sent to a server except the registration form).

## Pages

- **`index.html`** — FocusFirst **hub / landing page**. Links to everything below
  and shows quick stats (ledger balance, checks written).
- **`register.html`** — public **Focus Group registration** form (previously the
  site's front page). Share this link with research participants.
- **`checks.html`** — **check printing** for Office Depot B 7200 business voucher
  checks (item 637‑515), South State Bank account.
- **`ledger.html`** — **bookkeeping ledger**: income/expense/transfer tracking
  with a running net, inline category editing, and check import.
- **`import.html`** — **multi-source CSV importer** (Square, Meta, Tremendous,
  South State, Amex) that feeds the ledger.
- **`reports.html`** — **Profit &amp; Loss** statement for any date range.

> Note: `index_Version6.html` is an older copy of the registration form kept from
> before the restructure; it is not linked anywhere and can be deleted.

## Check printing (`checks.html`)

A self-contained, offline single-page app (no server, no dependencies — the MICR
E‑13B font is embedded). Prints the **full check face** on blank Office Depot
voucher stock: company/bank info, payee, amount (numeric + written), memo,
signature line, and the magnetic **MICR line** (routing / account / check number)
in the bottom clear band, plus two voucher stubs.

### Using it

1. Open `checks.html` in a browser.
2. **Account & Bank Setup** — enter FocusFirst's name/address, South State Bank
   info, 9‑digit routing number (ABA checksum is validated), account number,
   fractional routing, and the next check number. Click **Save setup**
   (stored in your browser's localStorage — nothing is sent anywhere).
3. **Write a Check** — enter payee, amount, memo, and optional stub line items,
   then **Record & Print** (logs to the register and advances the check number)
   or **Print (don't record)**.
4. In the browser print dialog: paper **Letter (8.5×11)**, scale **100% / Actual
   size**, margins **None**, and **disable** "fit to page" and headers/footers so
   dimensions stay exact.

### Calibrating alignment

Because these are *blank* sheets, layout is driven entirely by the app. Use
**Print alignment guide** (draws the check/stub boundaries and the MICR clear
band) on plain paper, hold it against a real check sheet up to the light, and
adjust the **Offset X/Y** and **MICR** position values under *Advanced* until it
lines up on your printer. Defaults assume a 3.5″ check + two 3.5″ stubs.

### Important notes

- **MICR toner:** banks technically require magnetic (MICR) toner for reliable
  machine reading. Standard laser toner usually clears via image processing, but
  confirm acceptance with South State before relying on it.
- **MICR symbol map** (embedded E‑13B font): `A`=Transit ⑆, `B`=Amount ⑇,
  `C`=On‑Us ⑈, `D`=Dash ⑉. The amount field (far right) is intentionally left
  blank for the bank to encode.
- The check register can be exported to CSV for bookkeeping.

## Ledger (`ledger.html`)

Simple single-account bookkeeping for the South State FocusFirst account.

- Set an **opening balance** and as-of date, then add **money in / money out**
  transactions with category, payee, method, memo, and a cleared flag.
- **Import checks from register** pulls every check written in `checks.html` into
  the ledger as expenses (idempotent — re-importing only adds new checks).
- Shows a **running balance** per row plus summary totals (in, out, current
  balance, uncleared count), with search/category/direction filters.
- **Export CSV** for your accountant, and **Backup / Restore** (JSON) to move the
  data between browsers/devices or keep a safe copy — important, since the data
  lives only in this browser.

## Importing transactions (`import.html`)

Upload a CSV export from each source; the app maps the columns, classifies each
row, flags duplicates, and shows a **preview before anything is saved**.

- **Presets** for South State (bank), Amex, Meta, Tremendous, Square, and a
  Generic option. Columns auto-map but can be overridden.
- **Category rules** (`keyword => Category`, saved) auto-categorize rows; a
  category of `Transfer` excludes the row from the P&amp;L.
- **De-duplication**: re-importing the same file adds nothing (matched on
  source + date + amount + description).

### Avoiding double-counting (how the defaults are set)

The books count each dollar once:

- **Revenue = South State deposits.** Square is imported as **transfers** (you
  already count that money when it lands in the bank), so it never double-counts.
- **Expenses = individual charges:** Amex line items + Meta + Tremendous + bank
  checks/ACH. The **bank→Amex payoff** and **Amex payments/credits** are
  auto-marked **transfers** and excluded from the P&amp;L.
- **Transfers** (type `transfer`) never affect the P&amp;L or the ledger's net.

> **Pick one source per expense.** A charge often appears on more than one export
> (e.g. a Meta ad shows on the Meta export *and* on your Amex statement; a check
> shows in the check register *and* on the bank export). Import it from **one**
> place, or mark the duplicate as a transfer. When in doubt, importing **Amex +
> bank** covers most spend; use Meta/Tremendous/Square exports for detail only.

## Profit &amp; Loss (`reports.html`)

Income statement for a chosen period (this month / quarter / year, or custom):
revenue by category, expenses by category, and **net profit**, with transfers
excluded and a note showing how many were left out. Export to CSV or print.

## Roadmap (planned)

- **Cloud storage via Google Sheets** — route the ledger to a Google Sheet
  (reusing the Apps Script pattern the registration form already uses) so books
  are saved off-device and openable in Sheets. *This is the agreed next step.*
- Reconciliation view against a bank statement (per-account balances).
- Cross-source duplicate detection (auto-match a bank debit to its Amex charge).
- Income-vs-expense charts and monthly comparisons on the P&amp;L.
- Deposit-slip printing (Office Depot D 9000 stock in the same pack).
