# LaundryTrack logical data dictionary (draft)

Logical columns only; Oracle data types and sizes are intentionally omitted.

## CUSTOMERS
| Column | Meaning | Key | Required? | Unique? |
|---|---|---|---|---|
| customer_id | Internal customer identifier | PK | Yes | Yes |
| customer_code | Anonymous customer reference | Candidate key | Yes | Yes |

## ORDERS
| Column | Meaning | Key | Required? | Unique? |
|---|---|---|---|---|
| order_id | Internal order identifier | PK | Yes | Yes |
| order_code | Source order reference | Candidate key | Yes | Yes |
| customer_id | Customer who placed order | FK → CUSTOMERS | Yes | No |
| order_date | Date/time order received | — | Yes | No |
| current_status | Current order status | Controlled value | Yes | No |

## SERVICES
| Column | Meaning | Key | Required? | Unique? |
|---|---|---|---|---|
| service_id | Internal service identifier | PK | Yes | Yes |
| service_code | Service code | Candidate key | Yes | Yes |
| service_name | Service label | — | Yes | No |

## ORDER_ITEMS
| Column | Meaning | Key | Required? | Unique? |
|---|---|---|---|---|
| order_item_id | Internal line identifier | PK | Yes | Yes |
| order_id | Parent order | FK → ORDERS | Yes | No |
| service_id | Service applied | FK → SERVICES | Yes | No |
| item_type | Garment/item category | — | Yes | No |
| quantity | Number of units | Positive-value rule | Yes | No |
| unit_price_snapshot | Agreed unit price at order time | — | Yes | No |

## PAYMENTS
| Column | Meaning | Key | Required? | Unique? |
|---|---|---|---|---|
| payment_id | Payment identifier | PK | Yes | Yes |
| order_id | Order receiving payment | FK → ORDERS | Yes | No |
| payment_date | Date/time received | — | Yes | No |
| payment_method | Payment channel | Controlled value | Yes | No |
| amount | Amount received | Nonnegative rule | Yes | No |

## ORDER_STATUS_HISTORY
| Column | Meaning | Key | Required? | Unique? |
|---|---|---|---|---|
| status_history_id | Status event identifier | PK | Yes | Yes |
| order_id | Related order | FK → ORDERS | Yes | No |
| status | Status after change | Controlled value | Yes | No |
| changed_at | Date/time recorded | — | Yes | No |

## COLLECTIONS
| Column | Meaning | Key | Required? | Unique? |
|---|---|---|---|---|
| collection_id | Collection identifier | PK | Yes | Yes |
| order_id | Collected order | FK → ORDERS | Yes | No; make unique if one collection per order |
| collected_at | Handover date/time | — | Yes | No |
| collected_by_code | Anonymous staff reference, if available | — | No | No |

## Relationships and cardinality
- CUSTOMERS 1 to 0..many ORDERS; each order has exactly one customer.
- ORDERS 1 to 1..many ORDER_ITEMS; each item line belongs to one order.
- SERVICES 1 to 0..many ORDER_ITEMS; each line uses one service.
- ORDERS 1 to 0..many PAYMENTS; supports partial payments.
- ORDERS 1 to 0..many ORDER_STATUS_HISTORY.
- ORDERS 1 to 0..1 COLLECTIONS; each collection belongs to one order.

## Database business rule
An order may be collected only when its status is READY and its outstanding balance is zero. Enforce this in PL/SQL/database logic, not only in the user interface.

Validate all entities and relationships against the real dataset before finalizing.
