# LaundryTrack

**Student:** UWASE Honorine  |  **ID:** 20252SEN390
**Course:** Database Development with Oracle PL/SQL  |  **Instructor:** Eric Maniraguha
**Domain:** Business and Entrepreneurship (small service business: laundry and garment care)
**Keywords:** Laundry Management, Order Tracking, Payment Control

## Problem statement
Small laundries often use paper tickets, which leads to lost clothes, price disputes, unpaid orders released by mistake and no customer history.

## Objectives
**Main objective:** Build an Oracle PL/SQL database that tracks each laundry order from drop-off to payment and collection.

**Specific objectives:**
1. Record customers, items, prices, payments and collections.
2. Block the release of unpaid orders using PL/SQL rules.
3. Track order status and produce daily income and pending-order reports.
4. Audit all data changes and denied actions.

## Data source
Real, anonymised order records (target: 300 or more orders) collected from a local laundry with the owner's permission, for educational use. Customer names and phones are replaced by codes. Details and search notes are in [`docs/research/research_log.md`](docs/research/research_log.md). Data source to be confirmed with the instructor.

## Innovation
**Option:** User interface, with smart automation as an add-on.
**Tools:** Oracle APEX app (new order, payment, release, owner dashboard) calling PL/SQL procedures; a DBMS_SCHEDULER job that flags orders Ready for more than 7 days.

## Planned tables
CUSTOMER, SERVICE_PRICE, LAUNDRY_ORDER, ORDER_ITEM, PAYMENT, DAMAGE_CLAIM, ORDER_ARCHIVE, AUDIT_LOG

## Planned PL/SQL features
| Feature | Use |
|---|---|
| Procedure `create_order` | Adds an order with its items and calculates the total |
| Procedure `record_payment` | Records a payment and marks the order Paid |
| Procedure `release_order` | Marks the order Collected |
| Function `get_customer_total` | Returns a customer's total spending |
| Cursor | Lists orders Ready for more than 7 days |
| Package `laundry_pkg` | Groups the procedures and functions |
| Trigger | Blocks releasing an unpaid order |
| Audit trigger | Writes every change and denied action to AUDIT_LOG |
| Exceptions | Customer not found, order not found, unpaid release |

## Phase I – Business Process Modeling

**Selected process:** Laundry order intake, processing, payment and collection. A customer drops off clothes, the laundry washes and irons them, the customer pays, and the order is released against a valid ticket.

**Modeling tool:** diagrams.net (draw.io)

**Published diagram link:** https://viewer.diagrams.net/?tags=%7B%7D&lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=business_process_diagram.drawio&dark=auto#R%3Cmxfile%3E%3Cdiagram%20name%3D%22Laundry%20Order%20Intake%20and%20Processing%22%20id%3D%22YxhVezcfj16jEfzxGYLF%22%3E7V1bc5s4FP41mck%2BbAeBufjRsd000zjx2sl0%2BtSRQTFMAHkFjuP99SuJi7nFJlSAM2naSeAYBDnfx9F3jiRyoYy912sCN%2FYMW8i9kCXr9UKZXMjycKDS78ywjwyqLEWGNXGsyAQOhqXzH4qNyWFbx0JB7sAQYzd0NnmjiX0fmWHOBgnBu%2FxhT9jNX3UD16hkWJrQLVt%2FOFZoR1ZDlQ72b8hZ28mVgRR%2F4sHk4NgQ2NDCu4xJmV4oY4JxGG15r2PkMt8lfonO%2B%2FrGp%2BmNEeSHdU6YaO4ueJj%2F%2B8%2FMmP%2F7evNdmure36pRbiZuOQj3iROCneO50Kd7V3boudQI6KZpO651C%2Fd4y04MQmg%2BJ3tXBAUUynnSKiiYZpDxQ2LtYeL8h%2F0QurGBtkPCmAeKmjtiyS4RH7aznRAtN9Bkx%2B0o9ajtiR61jO8aJPtRS2DA9h3XHWMXE%2F5bKU8q%2B8cvSfAzynyi8S%2F6SeSJF%2BhuY0%2Fcwq1vkf0DYXciS1fbwPFRENDNOcFmtHV5TyxE6MYNvelndCGPDx87%2Fjreh3uP%2BYaBpkFvwy8m0XtwKYsd7P8VXxqREL1mcImxvUbYQyHZ00PsDP2GCft2B67KwIiNcTuDeHef34Xxk7JOm06vtqD3BP01dWz%2BcqpRvtzgXReDboiID0N0haljgyyB6Ubm9zyYOK3fQ%2FFhmeJHn4WTxD9C0ZidBaZZEBlPZiXTTAOtnk5wV6li4ngbhNQ3pBFPgFbFkyRWx%2B0o6tvEyIH0XkQ0qS4iwxIiG%2Bz4Ib8h9Yr%2Bl77I9C7HyY8LdcIeL2aP9vM2vfJYwI3F8%2BNj9ap2QcEmVx%2Bbb5f%2BpwSw4Yb9Jt7rmnWXX1Ybz%2F8SG682iDjUw4hxg7qRdnFofjBVhrwMK1c0JK0Je4yogUZh1%2BG8TbdYhPYtSCx2G3tvhdmJa%2BQjwmhboqyKDGtQRVlDXik8ODLOObSnvIUr5M5x4LC4RY9Z4ZAyM3PAyHXW7IMQb44zWRoR4rzQ7l7W6IUAj%2FMO5ScNjC4ObRQ0IntC62yQyjNdU%2FNBqjXiV3TadYnPkUVWDPYJLkBiJsFDrej4%2BNfRcFQC6RvlDrXcv3CcUjhSnLK92DXFkHVxNvbZz7utt2oYp9QydEDJYwfUAnhHurPfA0%2F%2BsOAtULglPvcjf5oeHPMZhRn0njDJqY%2FWwJKHXaGlfFi0qDRkBgfS7zO8cpjsSpGa0WeKfRwBBgO7vefK6AqqwR850IMcQH49JfBkmMisFK8rQx2okkAlkCROcSCij2A7%2Fb0syV31%2BGrvyQeNP3I1fpa20lStUfJBeRVGii0MKZEgy2QbQJWWeerkIftC7iIeLK0mWAmqXfcZKWAlQJaItsmF8kFMr%2FaZfqMoxmgMulrw2NSNLNNbe8L05qB9lt6D2NhbbQORvYePuSHtPtCr6W4DKvyvd41CkKgu5A7tMqxPnwblazOil8OTmq9vKYXopOtt8byiUvtBgtMCrZ0g6i4O4SnF6HJMEAxZbBo%2FLh%2FuZ9PFX1F4GosMT4U%2BZVAIT0ZbqNUtPp4faryPp6abEHlBVLReIibL%2BAZ5cUz0Rv6fAno7erybLH7%2Bul9Mpovcwewwbv118zCd%2FSbeWgXe2nG8AdBaQlyvW9w8P8RvgmCLqsoDIc49uqIUQz6SaoV8E8ht6Ty9bhnu%2FCAa24gPPCUQZUePDlovHVtahjDcBh2ovWJdB7Ql9%2FS6Vbg%2Fcu8zyL24YpChPmNPW2IPDPSO1J5et3z5h%2BafgeZpRD9kNth%2FcoiH2mS7Jusdsb1uBbjlDvl0pbOEzHeENkzLxpXLrAyeQfKcQSztix%2F9DYtSvFN%2BbNwpl%2BEDhakXQC%2BI3vYEVd0q5yeOVu1X8GmjnsPm0pxFDf8bcq1slh%2Bxvtm0ptMVfTAs1GFkuS2un0eRuMb8jBI0C5q%2BwwC9Ea4eNxbP2ssBK0r%2FDmMyvxe4KrIJtQgmyIPZYipYt3rc3vgMAhRLvQrJoaYrsNH4TGGa4nIfhMhrBBeomOJ3aoRG0Y%2FAdWJKoVKmx7sv18GkQr1uNTYhWNclhJQ7FVEgJA6KZugQXsSTbp0gzBYJxlmhCV1z60b1vAfMWFxddFjycsTcMZ%2B3TBBNojN%2BJ1JUzFEEBYmqFfKx1oYG9bp13Aq8%2FyicxgontAnesTnTGZnjoSDgk%2FYbBFJRImeJ%2BGy45WxJv9%2Fh0HmiJ0UTp94qmkqXCwStfTMBVCNZU5R8spbID%2BGPQtIhfMDQlw5MzEc%2FZ9O7hwxatD%2FCJNY2QgeeipXSYlKWTCsUj1PdKnfLODVYZTEiph3NIeVClWnQezdf6HuwIZvQCNiZMyqH7LjKPWlx0LCUZLSHXd0qd4trFk4nGE1lKTX%2BwOS56dqFBopUVdsSBkbdQm0CadfR8O108AefPcrGdSfcydlk8Iawviyd551TekmimIwuKSN2gs%2F1JF6TaBGU6BSxMJ5raF09iXVLkxX4fhbh94kK8RPoUe3JdALO9kYzJ17aF8%2BWaK8mLxUGWtuTeXVruucX2OZR1eM9NXmuzVsIW0UFIQ%2Fz6yHbi1t1y5TtKQhJUiVUCZ0kgQlV2acVBNuvIsMRZXG%2F85sKC%2F39wkJXWhMWdSuTCdLnMyaW5lKT0Wx0Pf01vh3d5OZ4Vc4aG1HpwGcBYm%2BD%2FAAKXYekFcNoYV3ysK2hTaNuobBlGN8Mo6uBpkqDE4XCCXTcPQ%2BTG0zC7Lq%2FyxvfpK6KdOQc%2BVa6wn3sQscL4rmcQicfacW0TC6q%2FbbAjKuAyCq9NKL8%2BgS8JSY60lhCyQzgrN0kHFI%2F23iNfehOD9arAyWkPPxRWzREr9HRQak3XE6QSx%2B4l%2FxvJdx%2FSecnxn%2Fl91R04D%2BtV%2F8Bkf4rB5wO%2FKf36j9ZpP%2FKZbMO%2FGeUwvTPaFl8bz5VRPq0LEY78OmwV%2F8NPjwnhyVO3uFeXaqKdGl50XH7Lo2Gl3vznybQf3ofj7Qu9eo%2FXaT%2F%2BpCJUVW7N%2F8ZAv2XFOi79d%2BgV%2F%2BJTFOSAni3%2FtPPTeak7%2FX7wE5Vz6yfBpLIfMYQqn0sGNhp6eVdDu7PmyKzm4Qs3XbbvSpxIIlMZXShujFHR%2BRbI%2FZqU9bMBvmR5avjJnX4mt5Oxh6y85r4hKaRi0jYLxJCkyK5Dyr3WmgDksgUKFkv260D5X4dKDQH6oOByRvQCt29lF2J0Z97RaZIWh8SX%2B81RQKSyBxJ78WBg0p%2B9ulTkXmT3ofE1%2FtVoEBkjtQLKZO5hWeUeAKRSVIvPVGyZK7oVOlyLqAj4qdSPczfmpkcEE%2B9O7Q8Z4aLzOvJSouI1CxUpRNkqbAYunAC3Yju4YC0gGVGAAjN6fooJeq9jpgCIDSn62XItNdaIgAiUzFDaCbRRlJslAdY00kvEnMle0l5vDqhDVTo7uFPgkRh5PB3VZTp%2Fw%3D%3D%3C%2Fdiagram%3E%3C%2Fmxfile%3E

**Supporting explanation:** [business_process_explanation.pdf](docs/business_process/business_process_explanation.pdf)

## References
See the full list in [`docs/research/research_log.md`](docs/research/research_log.md). Sources are added only after they have been read.

- Geasela, Y. M., & Leonardy, R. F. (n.d.). *Perancangan sistem basis data terintegrasi untuk optimalisasi layanan laundry*. https://journal.ubm.ac.id/index.php/jbase/article/download/8282/3252
- Ibrahim, L. A. (2017). *Laundry management system: Design and implementation* [Diploma project report]. https://www.academia.edu/41847521/Laundry_Management_System_Design_and_Implementation
- Oracle. (n.d.). *Oracle Database PL/SQL language reference*. https://docs.oracle.com/en/database/oracle/oracle-database/

## Phase 0 files
| Deliverable | Location |
|---|---|
| Research log | `docs/research/research_log.md` |
| Proposal (PDF) | `docs/proposal/phase0_proposal.pdf` |
| Presentation (12 slides) | `docs/presentation/laundrytrack_phase0.pptx` |
