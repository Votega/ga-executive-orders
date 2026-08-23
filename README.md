# ga-executive-orders

Machine-readable archive of executive orders issued by the Governor of Georgia, maintained by [VoteGA.org](https://votega.org).

---

## Files

| File | Format | Contents |
|---|---|---|
| `data/2026.json` … `data/2023.json` | JSON | Orders for one year each (2026 updated daily; earlier years complete) |
| `data/executive-orders.csv` | CSV | **Every order, all years, in one file** — one row each, for spreadsheets |
| `data/executive-orders.schema.json` | JSON Schema | Validating / typing a per-year file |
| [`SUMMARY.md`](SUMMARY.md) | Markdown | **Reading** — counts by year & category, plus the 10 most recent orders |

**Coverage:** 2023–present. Years prior to 2023 used a different URL structure on gov.georgia.gov and are not yet included.

Raw files can be accessed directly using:

```
https://raw.githubusercontent.com/Votega/ga-executive-orders/main/data/[year].json
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
