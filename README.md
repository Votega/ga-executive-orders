# ga-executive-orders

Machine-readable archive of executive orders issued by the Governor of Georgia, maintained by [VoteGA.org](https://votega.org).

---

## Files

| File | Format | Contents |
|---|---|---|
| `data/<year>.json` | JSON | Orders for one year each — one file per year (e.g. `data/2026.json`; current year updated daily, earlier years complete) |
| `text/<year>.jsonl` | JSONL | Extracted full text of each order's PDF — one line per order, `{"number": "...", "text": "..."}`, joined to the JSON on `number` |
| `data/executive-orders.csv` | CSV | **Every order, all years, in one file** — one row each, for spreadsheets |
| `data/executive-orders.schema.json` | JSON Schema | Validating / typing a per-year file |
| [`SUMMARY.md`](SUMMARY.md) | Markdown | **Reading** — counts by year & category, plus the 10 most recent orders |

Full text is extracted from each PDF (via `pdftotext`, falling back to OCR for scanned documents); orders whose PDF yields no extractable text are omitted from the `text/` bundles.

**Coverage:** 2020–present. Orders before 2020 are hosted only on archived sites and are not yet included.

Raw files can be accessed directly using:

```
https://raw.githubusercontent.com/Votega/ga-executive-orders/main/data/[year].json
https://raw.githubusercontent.com/Votega/ga-executive-orders/main/text/[year].jsonl
```

---

## Schema

Each file contains a metadata wrapper and an `orders` array:

```json
{
  "metadata": {
    "updatedAt": "2025-01-15",
    "source": "gov.georgia.gov"
  },
  "orders": [
    {
      "date":     "2024-09-24",
      "number":   "09.24.24.01",
      "title":    "Declaring a State of Emergency for Tropical Storm Helene",
      "category": "State of Emergency",
      "url":      "https://gov.georgia.gov/document/2024-executive-order/09242401/download"
    }
  ]
}
```

### Fields

| Field | Type | Description |
|---|---|---|
| `date` | string | ISO date the order was issued (`YYYY-MM-DD`) |
| `number` | string | Official order number (format: `MM.DD.YY.SEQ`) |
| `title` | string | Full title of the order |
| `category` | string | Classification — see categories below |
| `url` | string | Direct link to the official PDF on gov.georgia.gov |
| `sha256` | string | SHA-256 hash of the downloaded PDF, for integrity verification |
| `bytes` | integer | Size of the downloaded PDF in bytes |
| `fetchedAt` | string | UTC timestamp (ISO-8601) when the PDF was downloaded and hashed |
| `archiveUrl` | string \| null | Wayback Machine snapshot of the PDF; `null` if archiving hasn't succeeded |

The sha256, bytes, fetchedAt, and archiveUrl fields are added by an enrichment pass and are present once an order has been processed; archiveUrl may be null.

## Verifying integrity

Each order records a `sha256`, `bytes`, and `fetchedAt` so you can confirm a PDF you hold is byte-for-byte the document this archive recorded — and detect if the official file ever changes.

The `sha256` is the SHA-256 of the PDF bytes exactly as downloaded from the order's `url` at the moment given in `fetchedAt`. To verify one order, download the PDF and compare hashes:

```bash
curl -sL "<the order's url>" -o order.pdf
sha256sum order.pdf   # compare the output to the order's "sha256"
```

Windows equivalents: `Get-FileHash order.pdf -Algorithm SHA256` (PowerShell) or `certutil -hashfile order.pdf SHA256`. The `bytes` field is a quick pre-check — if the file size differs, the hash will too.

**What a match proves — and what it doesn't:**

- A match confirms your copy is **identical to the copy this archive fetched**. That is tamper/corruption detection: a re-issued or altered PDF served at the same URL will not match.
- The hash is **self-attested** — it lives in this repo alongside the data, so it certifies "matches VoteGA's recorded copy," not "cryptographically signed by the State of Georgia." It is an integrity anchor, not a proof of provenance.
- For a stronger check, hash the **`archiveUrl`** copy too — an independent Wayback Machine capture taken near `fetchedAt`. If the live PDF, this archive's `sha256`, and the Wayback snapshot all agree, the document is corroborated across three independent points.

**The hash history is a tamper log.** Because `sha256` is committed to git, a silent change to an already-published order is visible even when its `number` and `title` are unchanged: the next daily run rewrites the hash, and `git log` / `git blame` on that value shows exactly when the underlying PDF changed.

### Categories

Orders are automatically classified by title keyword into one of seven categories:

| Category | Description |
|---|---|
| `State of Emergency` | Declarations and renewals of states of emergency |
| `Writ of Election` | Orders calling special elections to fill vacant seats |
| `Suspension` | Suspensions of public officials |
| `Appointment` | Appointments to boards, commissions, or vacancies |
| `Authorization` | Authorizations for agencies or expenditures |
| `Flag at Half-Staff` | Orders directing flags to be lowered |
| `Other` | Orders that do not match a specific category |

Automated classification is based on title keywords and may occasionally miscategorize an order.

---

## Data Source and Freshness

Orders are sourced from the [Georgia Governor's website](https://gov.georgia.gov/executive-action/executive-orders). PDF links point directly to gov.georgia.gov and are not mirrored locally.

**Update schedule:** The current year file updates daily via a GitHub Actions workflow. Prior year files are considered complete and are not re-fetched.

---

## Contributing

To report a missing or miscategorized order, open an issue on this repository. Include the order number, date, and a link to the official PDF if available.

Direct edits to data files are not accepted via pull request — the files are overwritten on each daily run.

---

## License

[GPL-3.0](LICENSE)
