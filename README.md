# ga-executive-orders
A repository housing executive orders issued by the Governor of the State of Georgia. Inspired by https://github.com/gaodp/executive-orders 

# What it is
A machine-readable archive of Georgia Governor's executive orders, maintained by Votega. One JSON file per year. Data is sourced from gov.georgia.gov and includes PDF links which go directly to gov.georgia.gov. 

# Coverage
Currently 2023–2026. Earlier years (2019–2022) used a different URL structure and are not included (yet).

# Data schema
{
  "date":     "2024-09-24",
  "number":   "09.24.24.01",
  "title":    "Declaring a State of Emergency for Tropical Storm Helene",
  "category": "State of Emergency",
  "url":      "https://gov.georgia.gov/document/2024-executive-order/09242401/download"
}

# Categories
Categories are keyword-matched from title: State of Emergency, Writ of Election, Suspension, Appointment, Authorization, Flag at Half-Staff, Other. Note: Some may be misclassified given the logic of keyword matching from Executive Order title. 

# How it's updated
The votega.org repository runs a GitHub Actions workflow that fetches new orders and publishes here. The 2026.json file is updated once per day to catch new orders as they are issued. Prior years are considered complete and are not updated. 

# How to use it
Raw file URL pattern for direct fetch: https://raw.githubusercontent.com/Votega/ga-executive-orders/main/data/2026.json

# Limitations
- PDF links go directly to gov.georgia.gov — no mirror is hosted here
- Category assignment is automated by title keyword and may occasionally misclassify
- 2022 and earlier not yet available

# Contributing
Corrections (wrong category, missing order, etc) can be submitted via issue or PR.

# Source & license
Original documents are public records from the State of Georgia. Data compilation is released under General Public License v3. 
