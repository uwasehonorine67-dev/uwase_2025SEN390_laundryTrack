# LaundryTrack — Phase II: Data Sourcing and Normalization

Student: Uwase Honorine | ID: 20252SEN390  
Course: INSY 8311 — Database Development with PL/SQL  
Instructor: Eric Maniraguha

## Important status
This is an organized Phase II working package. **A verified real dataset of at least 500 records has not been provided**, so the raw-data folder is intentionally empty except for instructions. No invented records are presented as real. The sample CSV is synthetic and is for demonstration only.

Before submitting Phase II, obtain a real dataset with permission/licence, anonymize personal data, place the unchanged anonymized source export in `data/raw/`, and complete the dataset card and quality counts. Replace illustrative normalization rows with 3–5 rows from that real dataset.

## Contents
- `data/raw/`: immutable anonymized raw data (pending).
- `data/dataset_card.md`: source, licence, size, record unit and quality counts.
- `data/sample/`: clearly labelled synthetic example only.
- `docs/design/normalization.md`: UNF → 1NF → 2NF → 3NF.
- `docs/design/field_mapping.md`: raw field mapping and Phase IV cleaning checklist.
- `docs/design/erd.png` and `erd_source.mmd`: proposed logical ERD.
- `docs/design/data_dictionary.md`: logical columns, keys, meanings and cardinalities.

Proposed entities: CUSTOMERS, ORDERS, SERVICES, ORDER_ITEMS, PAYMENTS, ORDER_STATUS_HISTORY and COLLECTIONS. Validate the design against the real source before finalizing it.
