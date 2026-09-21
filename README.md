# AgroStock Pro — Agricultural Warehouse Stock Control System

*Leia em português: [README.pt.md](README.pt.md)*

Power BI dashboard with automated stockout alerts for agricultural warehouses, built on a star-schema data model in Excel + Power Query + DAX.

![Dashboard preview](images/dashboard_main.png)

---

## Problem

Many small and medium agricultural warehouses and stores in Angola still track stock manually, in notebooks. This creates a common failure pattern: a product (e.g. seed, fertilizer, grain) runs out without warning, a sale is lost, and the loss is only noticed after the fact — never anticipated.

## Objective

Build a stock control system that predicts stockouts *before* they happen, using historical consumption data, and presents the result through a clear, automated visual dashboard — replacing manual daily tracking with a single-click update.

## Data Used

The project uses a star-schema model with three tables (simulated data, structured to mirror a real warehouse):

- **dim_Produto** — product dimension: ID, name, unit, minimum stock threshold, unit price (Kz), shelf life (days), category (grain / input / seed).
- **fact_Stock** — current stock snapshot: product ID, current stock, minimum stock, status.
- **fact_Movimentos** — full transaction log: date, product ID, type (in/out), quantity, unit price, origin/destination (e.g. harvest, purchase, client sale).

> **Note on data:** all figures are simulated for demonstration purposes. The structure (tables, relationships, DAX logic) is built to be directly usable with real warehouse data — only the source table would need to be replaced.

## Tools

Excel (Advanced) · Power Query · Power BI · DAX · Star Schema Data Modeling

## Methodology

1. **Data modeling:** three tables connected in a star schema (`dim_Produto` as the dimension table, `fact_Stock` and `fact_Movimentos` as fact tables), enabling clean aggregation and filtering.
2. **Stockout risk logic (DAX):**
   ```
   Dias_Restantes = Em_Stock / Média_Saída_Diária
   Status = IF(Em_Stock < Stock_Minimo, "COMPRAR", "OK")
   ```
   A product is flagged for purchase when current stock falls below its defined minimum threshold, calculated from historical daily outflow in `fact_Movimentos`.
3. **Visualization:** conditional formatting (green/red) applied to the status column and KPI cards, so risk is visible in seconds without reading any number.

## Measurable Results

Based on the simulated dataset (5 products, transaction log across Jan–Mar 2026):

- **2 of 5 products** flagged as "COMPRAR" (below minimum threshold) at the time of the snapshot.
- **Total stock value:** ~3M Kz across all products.
- **Total volume:** 240 sacks/units in stock.
- Manual daily tracking (previously ~1h/day in a notebook, per the use case this replaces) reduced to a single-click refresh.

## Limitations

- Data is simulated, not yet validated against a real warehouse's operations.
- The stockout threshold (`Stock_Minimo`) is currently a fixed value per product, not dynamically adjusted for seasonality (e.g. higher consumption during planting season).
- No integration yet with point-of-sale or automatic inventory input — updates are manual (by design, for offline use without internet dependency).

## Conclusions

The star-schema structure and DAX logic built here are directly transferable to a real warehouse: the same model would only require swapping the simulated tables for a real product and transaction log. The project demonstrates the full pipeline from raw transactional data to an automated, decision-ready dashboard.

## Future Improvements

- Replace simulated data with a real warehouse dataset (in progress — open to collaborating with a local agribusiness or cooperative for validation).
- Add seasonal adjustment to the minimum stock threshold.
- Extend the DAX model to forecast days-until-stockout per product (not just current status).
- Add a second Power BI page with monthly cost/revenue analysis from `fact_Movimentos`.

---

**Developed by Pascoal Barros**
Data Analysis & Precision Agriculture — Angola
[LinkedIn](https://www.linkedin.com/in/pascoalbarros-agtech) · [GitHub](https://github.com/pascoalbarros-agtech) · pascoalbarros58@gmail.com
