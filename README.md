# focus-group-form

Static pages for FocusFirst.

## Pages

- **`index.html`** — Focus Group registration form.
- **`checks.html`** — FocusFirst accounting: **check printing** for Office Depot
  B 7200 business voucher checks (item 637‑515), South State Bank account.

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
