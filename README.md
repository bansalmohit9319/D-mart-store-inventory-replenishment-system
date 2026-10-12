**DMart Store Inventory & Supplier Replenishment System**

A database project focused on a practical retail problem: understanding what stock is available, knowing when a store needs replenishment, and spotting issues in incoming supplier orders.
I built this project as a SQL Developer exercise using SQLite and Google Colab. The notebook brings together related retail records and uses SQL to turn them into reports that inventory and procurement teams could use to investigate shortages, receipt mismatches, and other operational exceptions.
Why I built this project
Retail inventory data is connected. A product's stock position can depend on sales, goods received, returns, transfers, damage, and other posted movements. Purchase orders also need to be compared with the goods that actually arrive. When these records are reviewed separately, it can be difficult to see where action is needed.
The goal of this project is to organize the records in a relational database and use SQL to answer those day-to-day operational questions in a consistent way.
What the project covers
- Relational database design and an ER diagram.
- Primary keys, foreign keys, a composite key, constraints, and indexes.
- A generated, repeatable dataset with controlled cases for testing business rules.
- Consumption of a prebuilt internal API using Python Requests.
- HTTP response checks, JSON structure validation, and raw JSON storage.
- Loading validated data into SQLite.
- SQL reports for inventory movements, sales, purchase-order receipt reconciliation, supplier risk, demand trends, and exception analysis.
- Replenishment recommendations using stock levels, recent sales, and preferred-supplier status.
- Transaction examples showing an approved adjustment being committed and a temporary adjustment being rolled back.
- A proposed seven-step automation workflow, a scheduled exception report, and cron expression checks.
- Validation helpers that check query columns, row counts, expected key results, and workflow order.
Technologies used
- Google Colab — notebook environment for running the project.
- Python — setup, dataset preparation, API requests, and validation helpers.
- SQLite — relational database used by the project.
- SQL — joins, filtering, aggregation, subqueries, CTEs, window functions, reporting, and transaction control.
- Requests and JSON — API communication and data exchange.
- Pandas — loading and displaying query results.
- Graphviz — rendering the ER diagram and workflow diagrams.
Database overview
The database uses seven core tables:
Table	Purpose
products	Product details, category, cost, reorder level, target stock, and preferred supplier.
locations	Store and warehouse information.
suppliers	Supplier details and operating status.
purchase_orders	Purchase-order headers, destinations, dates, and order status.
purchase_order_items	Products and quantities belonging to each purchase order.
goods_receipts	Received quantities, receipt dates, damaged quantities, and supplier references.
inventory_movements	Stock-related events such as sales, receipts, returns, transfers, damage, and adjustments.


The purchase-order line is identified by the composite key (po_id, product_id). The project also uses system-managed tables for the inventory adjustment ledger, generated inventory exceptions, and API ingestion run details.
SQL analysis
The notebook includes thirteen SQL exercises. Together, they practise core SQL techniques and connect them to inventory and supplier operations, including:
- Filtering, sorting, and aggregating inventory data.
- Joining products, suppliers, purchase orders, receipts, locations, and movements.
- Finding receipt mismatches and missing records.
- Using set operations and functions such as CASE, COALESCE, and date functions.
- Using CTEs to split longer calculations into readable stages.
- Identifying products selling above the portfolio average.
- Preparing store-product replenishment recommendations.
- Ranking product demand within categories.
- Calculating daily cumulative sales, moving averages, and day-over-day changes.
- Finding the first and latest eligible receipt for each product.
- Ranking products by open-exception counts within their categories.
The review date used for as-of operational checks is 1 August 2026. Events after the review cutoff should not be included in results intended to represent that date.
Inventory exceptions and replenishment
The project generates exception records from defined business rules. Examples include low-stock reorder conditions, missing receipts, short or over receipts, late receipts, duplicate supplier references, damage spikes, and pending transfers.
The scheduled exception report groups open cases by exception type and severity, with counts and expected-versus-actual values. The replenishment report is designed to identify active store-product combinations below their reorder level and show the preferred supplier's status alongside the recommended quantity.
These reports are intended to help a user see what needs checking next, rather than having to inspect individual records one by one.
Transaction management
Two exercises demonstrate explicit SQLite transaction handling:
- T1 — COMMIT: save an approved +12-unit cycle-count correction for product P0005 at location LOC001.
- T2 — ROLLBACK: insert a temporary -9-unit damage correction for product P0008 at location LOC002, then roll it back so it is not retained.
This shows the difference between a change that is approved and should persist, and a temporary change that must be undone.
Workflow automation and cron
The proposed replenishment workflow has seven steps:
1. Scheduler or cron trigger.
2. Call the internal API.
3. Validate HTTP status and JSON response structure.
4. Save raw JSON for traceability.
5. Load validated records into SQLite.
6. Generate the SQL exception and replenishment report.
7. Log and monitor run status, row counts, and errors.
The notebook also validates these example five-field cron expressions:
Schedule	Cron expression
Daily at 8 AM, 12 PM, and 4 PM	0 8,12,16 * * *
Every weekday at 5 PM	0 17 * * 1-5
Every day at midnight	0 0 * * *
Every Sunday at 2 AM	0 2 * * 0


These are scheduling examples, not a live production scheduler. The internal API and SQLite runtime are prepared for the active notebook session, and Google Colab does not keep a persistent cron process running after its runtime stops.
How to run the project
1. Open the .ipynb notebook in this repository.
2. Open the notebook in Google Colab.
3. Run the cells from top to bottom so that the dataset, internal API access, SQLite database, and validation helpers are prepared in order.
4. Review the displayed SQL results, diagrams, transaction checks, and activity statuses.
Some resources are created at runtime, so run the setup cells first in a fresh Colab session. The internal API is provided by the project setup; this notebook does not create a production API service.
Validation approach
The notebook includes checks for expected SQL output columns, row counts, and stable first-row/key results. It also validates API behaviour, transaction persistence or rollback, workflow order, and cron expression values.
A query running without a SQL error is not enough by itself; its results must also match the expected business output. If an activity shows Needs Review, its result should be investigated before treating that activity as complete.
Project structure
The repository is intended to contain the main Google Colab notebook and this README file. Add any supporting project files that are needed to reproduce the notebook, but do not upload credentials, private tokens, or other sensitive information.
Limitations
This is a learning and demonstration project built around a runtime-generated dataset, a prebuilt temporary internal API, and SQLite in Google Colab. It does not include a production frontend, a deployed production API, cloud infrastructure, or a persistent scheduler. The dataset is for demonstrating database design and SQL analysis, not for representing actual DMart operational data.
What I learned
Working on this project helped me connect SQL concepts with a broader workflow. It gave me practice with relational modelling, joins, CTEs, window functions, data validation, API-based data ingestion, transaction handling, and report design. It also reinforced that reliable database work involves checking data quality and expected results, not just getting a query to execute.
Project: DMart Store Inventory & Supplier Replenishment System
Role: SQL Developer
Environment: Google Colab and SQLite
