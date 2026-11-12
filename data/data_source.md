# Data Source: LaundryTrack

## Summary

| Item | Detail |
|---|---|
| Source type | Own data collection from a real laundry |
| Provider | Name of the laundry (fill in), with the owner's permission |
| Location | Town or district only, no exact address needed |
| Period covered | Fill in, for example 3 months of orders |
| Target size | 300 orders or more |
| Format | CSV, using `data_collection_template.csv` |
| Licence / permission | Shared by the owner for educational use (see below) |


## Why this source

I searched for a public laundry-orders dataset (Kaggle, Hugging Face and general web search) and found none that fits. The results were general e-commerce or food-order data. The course allows collecting your own data, so I will use real order records from a local laundry.

## Permission and licence

| Item | Detail |
|---|---|
| Owner name | Fill in |
| Permission given on | Date (fill in) |
| How | Verbal or written (keep a short message or note as proof) |
| Allowed use | Educational use in this course only |

## Privacy

| Rule | How it is applied |
|---|---|
| No personal information | Customer names, phones and addresses are not copied |
| Customer codes | Each customer gets a code (C001, C002, ...) |
| Staff | No staff names recorded |
| Sharing | Only the anonymised file goes to GitHub |

## Collection method

| Step | Action |
|---|---|
| 1 | Ask the owner for permission and agree on the period to copy |
| 2 | Copy order lines from the paper tickets or notebook into `data_collection_template.csv` |
| 3 | Replace each customer with a code |
| 4 | Check totals: line_total_rwf = quantity x unit_price_rwf |
| 5 | Count the orders and record the number below |

## Backup plan

If the owner cannot share records, use one of these, with the instructor's approval:

| Option | Description |
|---|---|
| Survey | Short survey of laundry customers and owners about typical orders and prices |
| Documented sample | A sample built from a real price list and real observed orders, clearly described as such |

## Final dataset record (complete in Phase II)

| Item | Value |
|---|---|
| File name | |
| Number of orders | |
| Number of rows | |
| Date range | |
| Date collected | |
