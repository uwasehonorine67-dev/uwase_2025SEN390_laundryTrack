# data/ folder

This folder holds the data for LaundryTrack. In Phase 0 it describes where the data comes from and how it will be collected. Later phases add the raw data and the cleaned files.

## Files

| File | Purpose |
|---|---|
| `README.md` | This guide to the folder |
| `data_source.md` | Where the real data comes from, permission, privacy and licence |
| `data_collection_template.csv` | Empty template: the columns to fill in when copying orders from the laundry's records |
| `sample_data.csv` | 10 **made-up** rows that show the format only. **Not real data.** |

## Column guide

| Column | Meaning | Example |
|---|---|---|
| order_id | Unique code for the order | O0001 |
| order_date | Date the clothes were dropped off (YYYY-MM-DD) | 2026-10-05 |
| customer_code | Coded customer, never a real name or phone | C001 |
| service_type | Wash, Iron or Wash+Iron | Wash+Iron |
| item_type | Kind of clothing | Shirt |
| quantity | Number of items of that type | 4 |
| unit_price_rwf | Price per item in RWF | 500 |
| line_total_rwf | quantity x unit_price_rwf | 2000 |
| payment_method | Cash or Mobile money | Cash |
| payment_status | Paid or Unpaid | Paid |
| order_status | Received, In Progress, Ready or Collected | Collected |
| pickup_date | Date the order was collected (empty if not yet) | 2026-10-07 |

One row is one **item line** of an order, so an order with 3 item types has 3 rows with the same order_id. This makes it easy to split into LAUNDRY_ORDER and ORDER_ITEM in Phase II.

## Rules

1. Never write real customer names, phone numbers or addresses. Use codes (C001, C002, ...).
2. Use the date format YYYY-MM-DD.
3. Keep at least 300 orders in the final real dataset.
4. Do not mix the made-up sample rows with the real data.


