# Dataset Card: Mr Klean Laundry Business Analytics Dataset

## 1. Dataset Metadata
* **Source Link:** https://www.kaggle.com/datasets/uzedkitchen/mr-klean-laundry-business-analytics-dataset
* **Licence:** CC0: Public Domain / Kaggle Open Dataset Licence
* **Format:** CSV (Comma-Separated Values, UTF-8)
* **Size:** 42 KB / 550 rows × 14 columns
* **Date Collected / Downloaded:** 2026-11-17
* **Unit of Observation (What one record represents):** One record represents a single customer laundry order transaction, including anonymised customer details, service category, item/weight quantities, order/pickup timestamps, payment method, and financial figures (revenue and total expense in RWF).
* **Privacy & Anonymisation Check:** All customer names have been fully anonymised to placeholder IDs (e.g., `Customer_101`) and phone/ID numbers are omitted to comply with privacy guidelines.

## 2. Raw Data Quality Audit (Unmodified Inspection)
*Per Phase II instructions, no values have been altered or cleaned in `data/raw/`. Cleaning will occur in Phase IV.*

* **Total Records:** 550 rows (Meets $\ge 500$ minimum requirement)
* **Duplicate Rows:** 0 exact duplicate rows
* **Missing / Null Values:**
  * `PickupDate`: 54 missing/blank values (orders currently in progress or uncollected)
  * `CustomerType`: 1 missing value
  * `PaymentMethod`: 1 missing value
* **Formatting & Data Type Inconsistencies:**
  * **Currency / Numeric Fields Stored as Text:** `Revenue` contains comma separators as strings, and 5 rows contain text noise (`"ERROR"`).
  * **Inconsistent Category Spellings:** `ServiceType` contains spelling variants (`"Wash & Fold"` vs `"Wash and Fold"`, `"Dry Cleaning"` vs `"Dry Clean"`).
  * **Missing / Blank Fields:** Blank strings present in optional metadata fields.
