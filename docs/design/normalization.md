# Step-by-Step Normalization & Field Mapping (UNF -> 3NF)

## 1. Identified Entities
1. **CUSTOMER**: Stores unique customer profile and tier/type attributes.
2. **BRANCH**: Stores store branch/counter location.
3. **SERVICE_CATALOG**: Stores standardized laundry service types and base unit rates.
4. **LAUNDRY_ORDER**: Stores order-level header details, timestamps, status, and branch/customer links.
5. **ORDER_ITEM**: Junction/child table resolving the many-to-many relationship between an Order and the Services requested.
6. **PAYMENT_EXPENSE**: Stores financial settlement, payment method, revenue, and operational expense tracking per order.

## 2. Normalization Walkthrough

### Unnormalized Form (UNF)
The raw CSV arrives as a single flat table where order headers, customer details, repeated/multi-valued service items (e.g., "Wash & Fold, Ironing"), and payment/cost metrics are combined in one row.

### First Normal Form (1NF)
* **Dependency Removed:** Removed repeating groups and multi-valued cells so every cell holds a single atomic value with composite primary key `(OrderID, ServiceName)`.

### Second Normal Form (2NF)
* **Dependency Removed:** Removed partial dependencies where order-header attributes depended only on `OrderID`, and `UnitPrice` depended only on `ServiceName`.

### Third Normal Form (3NF)
* **Dependency Removed:** Removed transitive dependencies where customer attributes depended on `CustomerID`, branch district depended on `BranchID`, and financial totals moved to `PAYMENT_EXPENSE`.

## 3. Raw Field Mapping & Phase IV Cleaning Checklist

| Raw CSV Column | Target 3NF Table | Target Column | Action / Status | Phase IV Type Conversion & Cleaning Rule |
| :--- | :--- | :--- | :--- | :--- |
| `OrderID` | `LAUNDRY_ORDER` | `order_id` | Kept (PK) | Trim whitespace; standardize prefix; verify uniqueness. |
| `CustomerID` | `CUSTOMER` | `customer_id` | Kept (PK/FK) | Trim whitespace; deduplicate customer rows. |
| `CustomerName` | `CUSTOMER` | `full_name` | Kept | Anonymise/mask PII; apply Title Case. |
| `CustomerType` | `CUSTOMER` | `customer_type` | Kept | Standardize casing (`Regular`, `Corporate`, `VIP`). |
| `Branch` / `Location` | `BRANCH` | `branch_name`, `district` | Kept | Extract unique branches; fix spelling variations; generate `branch_id`. |
| `OrderDate` | `LAUNDRY_ORDER` | `order_date` | Kept | Convert text to standard `DATE`. |
| `PickupDate` | `LAUNDRY_ORDER` | `pickup_date` | Kept | Convert text to `DATE`; allow `NULL` for active orders. |
| `ServiceType` | `SERVICE_CATALOG` | `service_name` | Kept | Split multi-value strings; standardize spellings; assign `service_id`. |
| `Quantity` / `Weight` | `ORDER_ITEM` | `quantity` | Kept | Strip units; convert text to `NUMBER`. |
| `UnitPrice` | `SERVICE_CATALOG` | `standard_unit_price` | Kept | Strip currency symbols; convert text to `NUMBER`. |
| `PaymentMethod` | `PAYMENT_EXPENSE` | `payment_method` | Kept | Standardize values (`Cash`, `Card`, `Mobile Money`). |
| `Revenue` | `PAYMENT_EXPENSE` | `total_revenue` | Kept | Strip currency symbols/commas; convert text to `NUMBER`. |
| `LabourCost` | `PAYMENT_EXPENSE` | `labour_cost` | Kept | Strip currency symbols/commas; convert text to `NUMBER`. |
| `TotalExpense` | `PAYMENT_EXPENSE` | `total_expense` | Kept | Strip currency symbols/commas; convert text to `NUMBER`. |
| `NetProfit` | — | — | **Dropped** | Derived column (`total_revenue - total_expense`); computed via SQL Views in Phase IV. |
