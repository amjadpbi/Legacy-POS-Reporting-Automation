# Bakery Project - Forensic Audit

Read-only forensic reconstruction of `D:\Power BI\PowerBI_Bakery_Project`. This audit covers only this project. No Power BI refresh, external database/service connection, publish, or GitHub push was performed. No existing project, source, configuration, documentation, image, or archive file was modified.

## 1. Executive Summary

### Finding
The actual Power BI project boundary is the nested `05_PowerBI` folder. It contains a thick PBIP project named `Bakery_Analytics`, its PBIR report, its TMDL semantic model, a PBIX artifact, and local Power BI runtime/cache files. The parent folders contain source data, configuration, documentation, and exports used to understand the project context.

### Evidence
- `05_PowerBI/Bakery_Analytics.pbip`
- `05_PowerBI/Bakery_Analytics.Report/definition.pbir`
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition.pbism`
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/`
- `01_SourceData/`
- `02_Configuration/`
- `03_Documentation/`
- `04_Exports/`

### Classification
OBSERVED FACT

### Finding
The active model uses a required M parameter, `pDataFolderPath`, pointing to `D:\Power BI\PowerBI_Bakery_Project\01_SourceData`. Active Power Query definitions use `Folder.Files`, `Csv.Document`, and a custom sales-cleaning function to load and transform Sales, Customers, Products, Expenses, and Payments files.

### Evidence
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/expressions.tmdl`
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/model.tmdl`

### Classification
OBSERVED FACT

### Finding
The active model contains staging queries, dimensions, and final fact tables. The final semantic model has 12 TMDL table files, 50 measures, 11 relationship declarations, and one authored calculated column in `FactPayment`.

### Evidence
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/tables/`
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/relationships.tmdl`
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/tables/_Measures.tmdl`
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/tables/FactPayment.tmdl`

### Classification
OBSERVED FACT

### Finding
The source environment contains 4,082 CSV files, organized into Customers, Expenses, Payments, Products, and Sales families in both `01_SourceData` and `Simulated Raw Data`. The active parameter points to `01_SourceData`; the active model does not reference `Simulated Raw Data` in its current M definitions.

### Evidence
- `01_SourceData/`
- `Simulated Raw Data/`
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/expressions.tmdl`
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/tables/*.tmdl`

### Classification
OBSERVED FACT

### Finding
The project represents a bakery/POS financial and payment-analysis scenario. The active report and model implement financial, payment, customer, product, location, date, expense, and sales analysis. The Customer and Operation page definitions exist but currently contain no visual JSON files, so their intended historical content is not established.

### Evidence
- `05_PowerBI/Bakery_Analytics.Report/definition/pages/pages.json`
- `05_PowerBI/Bakery_Analytics.Report/definition/pages/*/page.json`
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/tables/_Measures.tmdl`
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/tables/*.tmdl`

### Classification
OBSERVED FACT for present artifacts; UNKNOWN for the historical purpose of empty pages.

## 2. Project Inventory

### Finding
The important project artifacts are:

| Artifact | Relative path | Purpose | Active/reference status |
|---|---|---|---|
| PBIP | `05_PowerBI/Bakery_Analytics.pbip` | Power BI project entry point | Active project artifact |
| PBIR entry | `05_PowerBI/Bakery_Analytics.Report/definition.pbir` | Report-to-model binding | Active report metadata |
| PBIR definition | `05_PowerBI/Bakery_Analytics.Report/definition/` | Report, pages, visuals, themes, settings | Active report metadata |
| PBISM | `05_PowerBI/Bakery_Analytics.SemanticModel/definition.pbism` | Semantic-model entry point | Active model artifact |
| TMDL definition | `05_PowerBI/Bakery_Analytics.SemanticModel/definition/` | Model, tables, relationships, expressions, culture | Active model metadata |
| TMDL script | `05_PowerBI/Bakery_Analytics.SemanticModel/TMDLScripts/Script 1.tmdl` | Alternate/create-or-replace development script | Historical/stale reference; not the active definition |
| DAX query | `05_PowerBI/Bakery_Analytics.SemanticModel/DAXQueries/Query 1.dax` | DAX query-view artifact | Present; runtime use not established |
| CSV source data | `01_SourceData/` | Active parameter-targeted file families | Active source location according to M parameter |
| Duplicate/simulated CSV tree | `Simulated Raw Data/` | Alternate source-data tree and ZIP | Present; active use not established |
| Date configuration CSV | `02_Configuration/DimDate.csv` | Configuration-side date artifact | Present; active model generates `DimDate` in M instead |
| Mapping workbook | `02_Configuration/ColumnMappingConfig.xlsx` | Column mapping definitions | Not referenced by active M; referenced by stale TMDL script |
| Validation workbook | `02_Configuration/ValidationRulesConfig.xlsx` | Validation rule definitions | Not referenced by active semantic-model definition |
| Documentation | `03_Documentation/It standardizes all files into a single clean output regardless of.docx` | Binary documentation artifact | Present; contents not parsed here |
| PDF export | `04_Exports/Bakery_Analytics.pdf` | Report export | Present; internal contents not reverse-engineered |
| PBIX | `05_PowerBI/Bakery_Analytics.pbix` | Binary Power BI artifact | Present; synchronization with PBIP is UNKNOWN |
| ZIP archive | `Simulated Raw Data/Bakery_POS_Data_3Years.zip` | Archived source-data artifact | Present; not extracted or inspected |
| Images | Project tree image files, if any | Supporting assets | Inventoried by extension; image contents not used as implementation evidence |

### Evidence
- Recursive file inventory under `D:\Power BI\PowerBI_Bakery_Project`
- `05_PowerBI/`
- `01_SourceData/`
- `02_Configuration/`
- `03_Documentation/`
- `04_Exports/`
- `Simulated Raw Data/`

### Classification
OBSERVED FACT. Presence is not treated as proof of active use.

### Finding
The project contains 4,082 CSV files in total: 2,300 under `01_SourceData`, one `DimDate.csv` under `02_Configuration`, and 1,781 under `Simulated Raw Data`.

### Evidence
- Recursive CSV inventory of `01_SourceData`, `02_Configuration`, and `Simulated Raw Data`.
- `01_SourceData`: 36 Customers, 36 Expenses, 1,096 Payments, 36 Products, 1,096 Sales.
- `02_Configuration`: 1 `DimDate.csv`.
- `Simulated Raw Data`: 36 Customers, 36 Expenses, 577 Payments, 36 Products, 1,096 Sales.

### Classification
OBSERVED FACT

## 3. Business Problem

### Finding
The active implementation addresses bakery/POS business analysis involving income, expenses, profit, cash flow, payment collection, payment fees, payment status/reconciliation, payment aging, customer, product, location, and sales analysis.

### Evidence
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/tables/_Measures.tmdl`
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/tables/FactSalesHeader.tmdl`
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/tables/FactSalesLine.tmdl`
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/tables/FactPayment.tmdl`
- `05_PowerBI/Bakery_Analytics.Report/definition/pages/*/visuals/*/visual.json`

### Classification
OBSERVED FACT

### Finding
The creator-provided history says the project originated from a freelance-platform project/scenario, and that the business scenario and requirements were discussed with an AI model. This history is not independently verifiable from the current filesystem artifacts.

### Evidence
- Creator-provided history supplied for this audit.

### Classification
CREATOR-PROVIDED HISTORY

### Finding
The project is described as using synthetic/generated data only through creator-provided history and surrounding file structure; it is not established as real client production data. The current files do not independently prove the original generation event or client provenance.

### Evidence
- Creator-provided history states that the AI-assisted Python process generated synthetic CSV data.
- `01_SourceData/` and `Simulated Raw Data/` contain patterned, date-organized CSV families.
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/expressions.tmdl` consumes local files rather than an external production connector.

### Classification
CREATOR-PROVIDED HISTORY plus INFERENCE; production/client status is UNKNOWN.

### Finding
The current artifacts do not establish a formal enterprise architecture specification or an external client production system.

### Evidence
- No formal architecture specification was found in the project inventory.
- The creator-provided history describes learning-oriented development.

### Classification
UNKNOWN / CREATOR-PROVIDED HISTORY.

## 4. Development Context

### Finding
Creator-provided development sequence:

```text
Freelance project scenario
  -> business requirements/scenario discussion
  -> AI-assisted exploration
  -> Python-generated synthetic data
  -> folder-distributed CSV files
  -> folder-based Power Query ingestion
  -> parameter-controlled source path
  -> cleaning/transformation
  -> staging queries
  -> final loaded tables
  -> semantic model
  -> Power BI dashboard
```

This sequence is historical context. Each stage is separately tested against filesystem evidence below.

### Evidence
- Creator-provided history supplied for this audit.
- `01_SourceData/`
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/expressions.tmdl`
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/tables/*.tmdl`
- `05_PowerBI/Bakery_Analytics.Report/definition/`

### Classification
CREATOR-PROVIDED HISTORY plus OBSERVED FACT for the stages supported by files.

### Finding
No Python generator file exists inside `D:\Power BI\PowerBI_Bakery_Project`. Therefore the existence, exact code, seed, and execution history of the Python generation step cannot be verified from this project folder. The source CSV outputs are present.

### Evidence
- Recursive `.py` inventory under `D:\Power BI\PowerBI_Bakery_Project`: no Python files found.
- `01_SourceData/`
- `Simulated Raw Data/`

### Classification
OBSERVED FACT for the absent local Python file; UNKNOWN for its exact implementation and execution history.

## 5. Source Data Architecture

### Finding
The active source-data folder contains five file families:

| Family | Files | Filename pattern | Observed range/pattern |
|---|---:|---|---|
| Customers | 36 | `Customers_YYYYMMDD.csv` | Monthly snapshots from `Customers_20220128.csv` through `Customers_20241228.csv` |
| Expenses | 36 | `Expenses_YYYYMMDD.csv` | Monthly snapshots from `Expenses_20220101.csv` through `Expenses_20241201.csv` |
| Payments | 1,096 | `Payments_YYYYMMDD.csv` | Daily files from `Payments_20220101.csv` through `Payments_20241231.csv` |
| Products | 36 | `Products_YYYYMMDD.csv` | Monthly snapshots from `Products_20220128.csv` through `Products_20241228.csv` |
| Sales | 1,096 | `Sales_YYYYMMDD.csv` | Daily files from `Sales_20220101.csv` through `Sales_20241231.csv` |

### Evidence
- `01_SourceData/Customers/`
- `01_SourceData/Expenses/`
- `01_SourceData/Payments/`
- `01_SourceData/Products/`
- `01_SourceData/Sales/`

### Classification
OBSERVED FACT

### Finding
The active source schemas are consistent for Customers, Expenses, and Products. Payments has two header variants. Sales has substantial header variation: 562 header variants were observed among 1,096 files, including normal headers, report/junk lines, alternate names, and abbreviated fields. This variation is directly relevant to the custom sales-cleaning function.

### Evidence
- First-line/header inventory across `01_SourceData/*/*.csv`.
- `01_SourceData/Sales/`
- `01_SourceData/Payments/`
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/expressions.tmdl`, `fnclean_sales`

### Classification
OBSERVED FACT

### Finding
Representative active source schemas are:

- Sales: order/customer/product/date/quantity/price/discount/tax/payment/location/lead-time/status fields, with source variations.
- Customers: `CustomerID`, `CustomerName`, `CustomerType`, `Email`, `Phone`, `RegistrationDate`, `IsActive`.
- Products: `ProductID`, `ProductName`, `Category`, `SubCategory`, `UnitCost`, `CurrentPrice`, `IsActive`.
- Expenses: `ExpenseID`, `CategoryName`, `ExpenseAmount`, `ExpenseDate`, `VendorName`, `Description`.
- Payments: `PaymentID`, `SalesOrderID`, `PaymentAmount`, `PaymentDate`, `PaymentStatus`, `PaymentMethod`, `FeeAmount`, `ReconciliationDate`.

### Evidence
- Representative files under `01_SourceData/Customers/`, `Products/`, `Expenses/`, `Payments/`, and `Sales/`.
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/expressions.tmdl`

### Classification
OBSERVED FACT

### Finding
`Simulated Raw Data` duplicates the Customers, Expenses, Products, and Sales family shapes and contains a shorter Payments series ending at `Payments_20230731.csv`. Its relationship to the active model is not established because the active parameter points to `01_SourceData` and the active M expressions contain no reference to `Simulated Raw Data` or the ZIP archive.

### Evidence
- `Simulated Raw Data/`
- `Simulated Raw Data/Bakery_POS_Data_3Years.zip`
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/expressions.tmdl`

### Classification
OBSERVED FACT for the separation; UNKNOWN for historical purpose.

## 6. Python Data Generation

### Finding
Creator-provided history states that an AI model helped generate the Python script used to create synthetic CSV data, and that the generated data was distributed into folders to represent the source-data environment.

### Evidence
- Creator-provided history supplied for this audit.
- Folder-distributed CSV artifacts under `01_SourceData/` and `Simulated Raw Data/`.

### Classification
CREATOR-PROVIDED HISTORY

### Finding
No Python generation script is present inside the Bakery project. The project therefore does not provide filesystem evidence for the generator's datasets, business rules, random seeds, date ranges, reproducibility, or whether it generated facts, dimensions, or both.

### Evidence
- Recursive file inventory under `D:\Power BI\PowerBI_Bakery_Project` found no `.py` files.
- CSV families under `01_SourceData/` and `Simulated Raw Data/`.

### Classification
OBSERVED FACT for the missing script; UNKNOWN for generator implementation details.

### Finding
The source artifacts show snapshot-style dimension/reference families (Customers and Products), daily Sales and Payments, and monthly Expenses. They do not by themselves establish whether Python generated facts only, dimensions/reference data only, or both.

### Evidence
- `01_SourceData/` family counts and filenames.
- `Simulated Raw Data/` family counts and filenames.

### Classification
UNKNOWN

### Finding
The generated-file outputs are present as source artifacts, but the exact correspondence between the creator's historical Python run, the two source trees, the ZIP archive, and the active `01_SourceData` path is not fully recoverable.

### Evidence
- Creator-provided history.
- `01_SourceData/`
- `Simulated Raw Data/`
- `Simulated Raw Data/Bakery_POS_Data_3Years.zip`
- No local Python script.

### Classification
UNKNOWN

## 7. Power Query / Folder Ingestion

### Finding
The active sales ingestion chain is:

```text
pDataFolderPath\Sales\
  -> Folder.Files
  -> fnclean_sales([Content])
  -> expanded standardized columns
  -> filename-derived OrderDate
  -> stgSales_Combined
```

### Evidence
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/expressions.tmdl`, expressions `pDataFolderPath`, `fnclean_sales`, and `stgSales_Combined`.

### Classification
OBSERVED FACT

### Finding
`fnclean_sales` reads each binary as UTF-8 text, removes empty lines and lines beginning `Report Generated` or `Sales Export`, standardizes variant headers to a fixed 12-column header, keeps lines containing `OrderID` or `ORD-`, rebuilds CSV text, promotes headers, renames variants, parses numeric fields, and converts remaining fields to target types.

### Evidence
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/expressions.tmdl`, `fnclean_sales`.
- Representative file `01_SourceData/Sales/Sales_20220101.csv` begins with report/export lines before data.

### Classification
OBSERVED FACT

### Finding
The active Customers, Products, Expenses, and Payments staging queries use `Folder.Files` against their respective subfolders, filter `.csv` files, parse with `Csv.Document`, promote headers, expand expected columns, and apply family-specific transformations.

### Evidence
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/expressions.tmdl`, `stgCustomer_Combined`, `stgProduct_Combined`, `stgExpenses`, and `stgPayments_Combined`.

### Classification
OBSERVED FACT

### Finding
Customer and Product staging sort files by filename descending and deduplicate by natural ID, retaining the first row after sorting. Both add an unknown row with key `-1` and then project typed final dimension columns.

### Evidence
- `expressions.tmdl`, `stgCustomer_Combined` and `stgProduct_Combined`.

### Classification
OBSERVED FACT

### Finding
Payments staging parses payment and reconciliation dates, converts payment and fee amounts, adds `IsReconciled = [PaymentStatus] = "Received"`, and adds `LoadDateTime = DateTime.LocalNow()`. The final `FactPayment` query removes the source filename and load timestamp.

### Evidence
- `expressions.tmdl`, `stgPayments_Combined`.
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/tables/FactPayment.tmdl`.

### Classification
OBSERVED FACT

### Finding
The active M definitions contain joins, grouping, appends, deduplication, indexing, type conversion, date parsing, filtering, column selection, and calculated fields. No active custom validation-function framework or active workbook-driven mapping expression is present.

### Evidence
- `expressions.tmdl`.
- `FactSalesHeader.tmdl`, `FactSalesLine.tmdl`, `FactExpense.tmdl`, `FactPayment.tmdl`, `DimLocation.tmdl`.

### Classification
OBSERVED FACT

### Finding
The active model does not contain an explicit `Excel.Workbook` call, does not reference either configuration workbook, and does not reference `Simulated Raw Data` or the ZIP archive. Therefore those artifacts are not established as active ingestion inputs.

### Evidence
- Recursive search of active `05_PowerBI/Bakery_Analytics.SemanticModel/definition/` for `Excel.Workbook`, `ColumnMappingConfig`, `ValidationRulesConfig`, `Simulated Raw Data`, and `Bakery_POS_Data_3Years.zip`.

### Classification
OBSERVED FACT

### Finding
Query folding cannot be established from local M definitions. The source is folder/file based, and no runtime diagnostics or source execution was performed.

### Evidence
- `expressions.tmdl`.
- No refresh or Power Query runtime execution was performed.

### Classification
UNKNOWN

## 8. Parameters

### Finding
The active model has one required text parameter: `pDataFolderPath`, with value `D:\Power BI\PowerBI_Bakery_Project\01_SourceData`.

### Evidence
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/expressions.tmdl`, expression `pDataFolderPath`.
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/model.tmdl`, query group `01_Parameters` and query order.

### Classification
OBSERVED FACT

### Finding
The parameter controls the base path used by the active Sales, Customers, Products, Expenses, and Payments staging expressions. Changing it would change the folder root those expressions call, subject to the expected subfolder names and schemas.

### Evidence
- `expressions.tmdl`: `Folder.Files(pDataFolderPath & "\\Sales\\")`, and equivalent Customers, Products, Expenses, and Payments expressions.

### Classification
OBSERVED FACT for the expression dependency; runtime redirection was not tested.

### Finding
No other active M parameter was identified. The mapping workbook is not an active parameter/configuration source.

### Evidence
- `expressions.tmdl` expression declarations.
- `TMDLScripts/Script 1.tmdl` is separate from active `definition/` and uses an obsolete path.

### Classification
OBSERVED FACT

## 9. Configuration System

### Finding
`ColumnMappingConfig.xlsx` contains one sheet, `MappingTable`, with 55 worksheet rows including the header and 6 columns: `FileType`, `SourceColumn`, `TargetColumn`, `DataType`, `IsRequired`, and `DefaultValue`. Its sample rows define alternate Sales source names such as `Order ID` and `OrderID` mapping to `SalesOrderID`.

### Evidence
- `02_Configuration/ColumnMappingConfig.xlsx`, sheet `MappingTable`.
- Read-only workbook inspection.

### Classification
OBSERVED FACT

### Finding
`ValidationRulesConfig.xlsx` contains one sheet, `ValidationRules`, with 21 worksheet rows including the header and 8 columns: `FileType`, `ColumnName`, `RuleType`, `RuleDescription`, `RuleExpression`, `Severity`, `Action`, and `DefaultValue`. Sample rules include `NotNull`, `PositiveNumber`, `RangeCheck`, `DateNotFuture`, and `DefaultIfNull`.

### Evidence
- `02_Configuration/ValidationRulesConfig.xlsx`, sheet `ValidationRules`.
- Read-only workbook inspection.

### Classification
OBSERVED FACT

### Finding
The active model does not consume either workbook. A separate historical `TMDLScripts/Script 1.tmdl` contains `ColumnMapping = Excel.Workbook(File.Contents(...ColumnMappingConfig.xlsx...))`, but that script points to `D:\Power BI\X PowerBI_Bakery_Project\...`, differs from the active parameter path, and is outside the active `definition/` model.

### Evidence
- `05_PowerBI/Bakery_Analytics.SemanticModel/TMDLScripts/Script 1.tmdl`.
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/expressions.tmdl`.
- `05_PowerBI/Bakery_Analytics.SemanticModel/TMDLScripts/.pbi/tmdlScripts.json`.

### Classification
OBSERVED FACT; historical/alternate status is INFERENCE.

### Finding
Dynamic column mapping is not established in the active implementation. The active M uses hard-coded column names in expansion, selection, renaming, and type-conversion steps.

### Evidence
- `expressions.tmdl`, `Table.ExpandTableColumn`, `Table.SelectColumns`, `Table.RenameColumns`, and `Table.TransformColumnTypes` calls.
- No active reference to `ColumnMappingConfig.xlsx`.

### Classification
OBSERVED FACT

## 10. Data Validation / Quality

### Finding
Implemented ingestion cleaning includes removal of empty/junk sales lines, standardization of sales header variants, numeric parsing with `try ... otherwise null`, date parsing, schema expansion, and explicit type conversion.

### Evidence
- `expressions.tmdl`, `fnclean_sales`, `stgCustomer_Combined`, `stgProduct_Combined`, `stgExpenses`, and `stgPayments_Combined`.

### Classification
IMPLEMENTED

### Finding
Implemented unknown-member handling exists for Customers, Products, and Locations through explicit rows using key `-1`. No equivalent unknown row was observed for `DimExpCategory`.

### Evidence
- `expressions.tmdl`, `stgCustomer_Combined`, `stgProduct_Combined`, and `DimLocation.tmdl`.
- `DimExpCategory.tmdl`.

### Classification
IMPLEMENTED for the listed dimensions; not established for expense category.

### Finding
The configuration workbook contains documented rule definitions, but active execution of those rules is not present in the current model. No active validation-rule execution, rejected-row table, flagged-row table, validation log, or error-report table was found.

### Evidence
- `02_Configuration/ValidationRulesConfig.xlsx`.
- Active `05_PowerBI/Bakery_Analytics.SemanticModel/definition/expressions.tmdl`.
- Active TMDL table inventory.

### Classification
DOCUMENTED CONFIGURATION / NOT ESTABLISHED AS IMPLEMENTED VALIDATION.

### Finding
The active M uses left-outer joins for surrogate-key lookups. The current files do not establish the runtime result for unmatched keys, null foreign keys, malformed rows outside observed representatives, or referential integrity after refresh.

### Evidence
- `FactSalesHeader.tmdl`, `FactSalesLine.tmdl`, `FactPayment.tmdl`, `FactExpense.tmdl`.
- No runtime refresh or data-quality execution was performed.

### Classification
OBSERVED FACT for join type; UNKNOWN for runtime data-quality outcomes.

### Finding
The Sales source family contains 562 observed header variants, while the active function recognizes a finite set of variant patterns and standardizes them. Whether every observed variant is successfully parsed is not established without executing the queries.

### Evidence
- Header-variant scan of `01_SourceData/Sales/`.
- `expressions.tmdl`, `fnclean_sales`.

### Classification
OBSERVED FACT plus UNKNOWN for full runtime coverage.

## 11. Staging and Final Tables

### Finding
The active query-group architecture is:

```text
01_Parameters
  -> 02_Functions
  -> 03_Staging - Sales
  -> 04_Staging - Payments
  -> 05_Staging - Products
  -> 06_Staging - Customers
  -> 07_Staging - Expenses
  -> 08_Dimension Tables
  -> 9_Fact Tables
```

### Evidence
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/model.tmdl`.
- Query groups in `expressions.tmdl` and table partitions.

### Classification
OBSERVED FACT

### Finding
The active lineage is:

```text
01_SourceData/Sales/*.csv
  -> fnclean_sales
  -> stgSales_Combined
  -> DimLocation
  -> FactSalesHeader
  -> FactSalesLine

01_SourceData/Customers/*.csv
  -> stgCustomer_Combined
  -> DimCustomer
  -> FactSalesHeader / FactSalesLine lookups

01_SourceData/Products/*.csv
  -> stgProduct_Combined
  -> DimProduct
  -> FactSalesLine lookup

01_SourceData/Expenses/*.csv
  -> stgExpenses
  -> DimExpCategory
  -> FactExpense

01_SourceData/Payments/*.csv
  -> stgPayments_Combined
  -> FactPayment

M-generated dates 2022-01-01 through 2024-12-31
  -> DimDate
  -> date-key lookups in fact queries
```

### Evidence
- `expressions.tmdl`.
- `FactSalesHeader.tmdl`.
- `FactSalesLine.tmdl`.
- `FactExpense.tmdl`.
- `FactPayment.tmdl`.
- `DimLocation.tmdl`.
- `DimDate.tmdl`.

### Classification
OBSERVED FACT

### Finding
The final loaded tables are the semantic-model sources: `DimCustomer`, `DimDate`, `DimExpCategory`, `DimLocation`, `DimPaymentMethod`, `DimProduct`, `FactExpense`, `FactPayment`, `FactSalesHeader`, `FactSalesLine`, `Payment Aging Categories`, and `_Measures`.

### Evidence
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/tables/*.tmdl`.
- `model.tmdl` table references.

### Classification
OBSERVED FACT

### Finding
The creator-provided memory of a staging-to-final architecture is supported by active TMDL query groups and table partitions. The active implementation does not show a separate loaded staging-table layer in the final table inventory; staging expressions are intermediate queries feeding final dimensions and facts.

### Evidence
- `model.tmdl`, query groups and query order.
- `expressions.tmdl` staging expressions.
- Final table TMDL files.

### Classification
OBSERVED FACT plus CREATOR-PROVIDED HISTORY.

## 12. Semantic Model

### Finding
The model contains 12 TMDL tables:

- Dimensions: `DimCustomer`, `DimDate`, `DimExpCategory`, `DimLocation`, `DimPaymentMethod`, `DimProduct`.
- Facts: `FactExpense`, `FactPayment`, `FactSalesHeader`, `FactSalesLine`.
- Helper/measure tables: `Payment Aging Categories`, `_Measures`.

### Evidence
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/tables/`.
- `model.tmdl`.

### Classification
OBSERVED FACT

### Finding
The architecture is star-like with shared date, customer, location, payment-method, product, and expense-category dimensions around fact tables. Sales is split into a header fact and line fact, with a header-to-line relationship.

### Evidence
- `relationships.tmdl`.
- `FactSalesHeader.tmdl`.
- `FactSalesLine.tmdl`.
- Dimension TMDL files.

### Classification
INFERENCE from observed relationships and table roles.

### Finding
Apparent table grains from the active M are:

| Table | Apparent grain | Evidence |
|---|---|---|
| `FactSalesHeader` | One grouped row per `OrderID` | `Table.Group(... {"OrderID"} ...)` in `FactSalesHeader.tmdl` |
| `FactSalesLine` | One source sales line within an order before `LineNumber` is removed | Grouping/indexing by `OrderID` in `FactSalesLine.tmdl`; persisted line identifier is removed |
| `FactPayment` | One source payment record with generated `PaymentKey` | `FactPayment.tmdl` and source Payment schema |
| `FactExpense` | One source expense record with generated `ExpenseKey` | `FactExpense.tmdl` and source Expense schema |
| Dimensions | Latest snapshot row per natural ID for Customers/Products; distinct location/category/method members for other dimensions | Staging and dimension M definitions |

Exact source uniqueness and runtime grain are UNKNOWN.

### Classification
INFERENCE from M transformations; exact runtime grain UNKNOWN.

### Finding
All table partitions inspected are Import mode. The active model has no observed incremental-refresh `RangeStart`/`RangeEnd` parameters or policy.

### Evidence
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/tables/*.tmdl`.
- `expressions.tmdl`.
- `model.tmdl`.

### Classification
OBSERVED FACT

### Finding
The model has 11 relationship declarations. One is explicitly inactive: `FactSalesLine.DateKey -> DimDate.DateKey`. The TMDL does not explicitly establish all cardinalities or cross-filter directions.

### Evidence
- `relationships.tmdl`.

### Classification
OBSERVED FACT

### Finding
The model contains hidden technical/key columns in multiple tables. `FactPayment` contains one authored calculated column, `Days Outstanding`, based on payment status and date difference. `Payment Aging Categories` is a disconnected calculated/helper table according to the model inventory and measure references.

### Evidence
- Table TMDL files under `definition/tables/`.
- `_Measures.tmdl` references to Payment Aging Categories.
- `FactPayment.tmdl`.

### Classification
OBSERVED FACT

### Finding
No RLS role files, calculation-group declarations, or object-level security structures were found in the active semantic-model definition.

### Evidence
- Recursive active TMDL inventory under `05_PowerBI/Bakery_Analytics.SemanticModel/definition/`.

### Classification
OBSERVED FACT

## 13. DAX / Business Logic

### Finding
The deployed measure store contains 50 measures in `_Measures.tmdl`, organized into base, breakdown, and time-series display folders.

### Evidence
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/tables/_Measures.tmdl`.

### Classification
OBSERVED FACT

### Finding
Core financial measures are `Total Income`, `Total Expenses`, `Net Profit`, `Profit Margin %`, and `Cash Flow`. Income uses `FactSalesHeader[NetAmount]`; expenses use `FactExpense[ExpenseAmount]`; net profit and cash flow derive from those measures.

### Evidence
- `_Measures.tmdl`, measures `Total Income`, `Total Expenses`, `Net Profit`, `Profit Margin %`, and `Cash Flow`.

### Classification
OBSERVED FACT

### Finding
Payment logic includes received and pending amounts, collection rate, payment fees, payment-fee percentage, and payment aging categories. The measures filter `FactPayment` by status, amount, fee, and date context.

### Evidence
- `_Measures.tmdl`.
- `FactPayment.tmdl`.
- `Payment Aging Categories.tmdl`.

### Classification
OBSERVED FACT

### Finding
Time-series logic includes month-over-month and year-over-year revenue, expenses, cash flow, profit margin, and collection-rate measures, plus color-returning measures used by report formatting.

### Evidence
- `_Measures.tmdl`, display folder `TIMESERIES`.
- `Bakery_Analytics.Report/definition/pages/*/visuals/*/visual.json`.

### Classification
OBSERVED FACT

### Finding
Potential concerns visible in static DAX include `YoY Expense Color` summing `FactExpense[ExpenseDate]` rather than `FactExpense[ExpenseAmount]`, and several comparison measures returning formatted text instead of numeric values. These are static observations; no runtime correctness or performance conclusion was made.

### Evidence
- `_Measures.tmdl`, measures `YoY Expense Color`, `MoM Revenue`, `YoY Revenue`, `MoM Expenses`, `YoY Expenses`, and related measures.

### Classification
POTENTIAL CONCERN

### Finding
The model uses fact-date columns to determine maximum dates in several time-series measures while applying date filters through `DimDate`. The exact behavior under all filter contexts was not executed.

### Evidence
- `_Measures.tmdl`, time-series measures.
- `relationships.tmdl`.

### Classification
OBSERVED STATIC PATTERN; UNKNOWN runtime behavior.

### Finding
No DAX query execution, performance trace, refresh, or semantic-model runtime validation was performed.

### Evidence
- `05_PowerBI/Bakery_Analytics.SemanticModel/DAXQueries/Query 1.dax` was inventoried but not executed.
- No external model connection was made.

### Classification
UNKNOWN

## 14. Power BI Report / PBIR

### Finding
Five PBIR page definitions exist:

| Page | Visibility/type | Visual JSON files | Observable role |
|---|---|---:|---|
| `Financial` | Visible | 47 | Main financial analysis page |
| `Operation` | Visible | 0 | Page definition exists; current visuals not present |
| `Customer` | Visible | 0 | Page definition exists; current visuals not present |
| `Tool Tip` | `HiddenInViewMode`, type `Tooltip` | 3 | Tooltip page |
| `Payment Details Analysis` | `HiddenInViewMode` | 23 | Payment detail/drillthrough-oriented page |

Total visual JSON files: 73.

### Evidence
- `05_PowerBI/Bakery_Analytics.Report/definition/pages/pages.json`.
- `05_PowerBI/Bakery_Analytics.Report/definition/pages/*/page.json`.
- `05_PowerBI/Bakery_Analytics.Report/definition/pages/*/visuals/*/visual.json`.

### Classification
OBSERVED FACT

### Finding
The Financial page contains cards, column charts, a clustered bar chart, a donut chart, a pivot table, slicers, navigation, shapes, and textboxes. Its visual metadata references income, expenses, profit margin, cash flow, month-over-month/year-over-year logic, location, and payment method.

### Evidence
- `05_PowerBI/Bakery_Analytics.Report/definition/pages/93ab71a60a1be1e56a06/page.json`.
- Visual files beneath the same page directory.
- `_Measures.tmdl`.

### Classification
OBSERVED FACT

### Finding
The Payment Details Analysis page contains 23 visuals including action buttons, bar/column charts, cards, a shape, a slicer, a table, and textboxes. Its metadata contains payment-related measures and filters. The page is hidden in view mode.

### Evidence
- `05_PowerBI/Bakery_Analytics.Report/definition/pages/8c2f7f906d552e6884c6/page.json`.
- Visual files beneath the same page directory.
- Payment bookmark/filter metadata where present in visual JSON.

### Classification
OBSERVED FACT

### Finding
The Tool Tip page is hidden, uses `ActualSize`, and has 3 visuals: 2 cards and 1 `cardVisual`.

### Evidence
- `05_PowerBI/Bakery_Analytics.Report/definition/pages/d15c09228c1066bb22b7/page.json` is the hidden tooltip page metadata.
- Visual files beneath the tooltip page directory.

### Classification
OBSERVED FACT

### Finding
The report declares base theme `CY26SU02`, custom/shared theme `Frontier`, and public custom visual `ZebraBITablesBAE31B370F254F808553548EFB35BFA5`. The corresponding theme files are present under `StaticResources/SharedResources/`.

### Evidence
- `05_PowerBI/Bakery_Analytics.Report/definition/report.json`.
- `05_PowerBI/Bakery_Analytics.Report/StaticResources/SharedResources/BaseThemes/CY26SU02.json`.
- `05_PowerBI/Bakery_Analytics.Report/StaticResources/SharedResources/BuiltInThemes/Frontier.json`.

### Classification
OBSERVED FACT

### Finding
No authored bookmark-state metadata was identified in the report inventory. Navigation and interactions are only reported where represented in PBIR metadata; runtime rendering and interaction behavior were not tested.

### Evidence
- `05_PowerBI/Bakery_Analytics.Report/definition/` inventory.
- Visual JSON files.

### Classification
UNKNOWN for runtime behavior; OBSERVED FACT for the local file inventory.

## 15. End-to-End Data Lineage

### Finding
The active file-defined lineage is:

```text
01_SourceData/<family>/*.csv
  -> pDataFolderPath
  -> Folder.Files family staging
  -> cleaning/type/date/header transformations
  -> dimensions and fact transformations
  -> Import-mode TMDL semantic model
  -> DAX measures
  -> PBIR report visuals
```

More specifically:

```text
Sales files -> fnclean_sales -> stgSales_Combined
  -> DimLocation / FactSalesHeader -> FactSalesLine
Customers -> stgCustomer_Combined -> DimCustomer -> sales fact lookups
Products -> stgProduct_Combined -> DimProduct -> FactSalesLine lookup
Expenses -> stgExpenses -> DimExpCategory -> FactExpense
Payments -> stgPayments_Combined -> FactPayment
M-generated date range -> DimDate -> fact date-key lookups
```

### Evidence
- `expressions.tmdl`.
- Final fact/dimension table partitions.
- `model.tmdl` query order and table references.
- `relationships.tmdl`.
- `_Measures.tmdl`.
- PBIR visual files.

### Classification
OBSERVED FACT

### Finding
The creator-provided sequence includes Python generation and folder distribution before Power Query ingestion, but the local project does not contain the Python generator and cannot prove the exact historical handoff from generated files to `01_SourceData`.

### Evidence
- Creator-provided history.
- No `.py` file under the project.
- `01_SourceData/`, `Simulated Raw Data/`, and ZIP archive.

### Classification
CREATOR-PROVIDED HISTORY plus UNKNOWN historical handoff.

## 16. Analytical Capabilities

### Finding
The implemented analytical capabilities evidenced by deployed measures, model objects, and PBIR metadata are:

| Capability | Evidence |
|---|---|
| Income/revenue | `_Measures.tmdl`: `Total Income`; `FactSalesHeader.NetAmount` |
| Expenses | `_Measures.tmdl`: `Total Expenses`; `FactExpense` |
| Net profit and margin | `_Measures.tmdl`: `Net Profit`, `Profit Margin %` |
| Cash flow | `_Measures.tmdl`: `Cash Flow`, MoM/YoY cash-flow measures |
| Payment collection | `_Measures.tmdl`: `Payments Received`, `Payments Pending`, `Collection Rate %` |
| Payment fees | `_Measures.tmdl`: `Payment Fees Total`, `Payment Fees % of Revenue` |
| Payment reconciliation/status | `FactPayment.tmdl`: `PaymentStatus`, `ReconciliationDate`, `IsReconciled` |
| Payment aging | `FactPayment.tmdl`: `Days Outstanding`; `Payment Aging Categories.tmdl`; payment page metadata |
| Sales time trends | `_Measures.tmdl`: MoM/YoY revenue and date model |
| Expense time trends | `_Measures.tmdl`: MoM/YoY expenses |
| Product/customer/location analysis | Dimension tables, sales facts, Financial/Customer page definitions and visual bindings |
| Operational sales attributes | Sales source fields for location, lead time, order status, payment method, and transaction dates |

### Classification
OBSERVED FACT

### Finding
Production, inventory, waste, forecasting, and other bakery capabilities are not claimed as implemented unless supported by the active model/report evidence. The current active model contains sales, payment, expense, customer, product, location, and date structures, but no separate production or inventory fact table was found.

### Evidence
- Active table inventory under `05_PowerBI/Bakery_Analytics.SemanticModel/definition/tables/`.
- Active report visual inventory.

### Classification
OBSERVED FACT for absent table types; UNKNOWN for any historical or external functionality.

## 17. Automation / Reusability

### Finding
The active model automatically discovers CSV files in five named subfolders below the parameter path using `Folder.Files`. Adding files to those folders could be discovered by the expressions if the files meet the expected extension, schema, naming, and content assumptions.

### Evidence
- `expressions.tmdl`: five `Folder.Files` calls and `pDataFolderPath`.
- Family-specific `Table.ExpandTableColumn` calls.

### Classification
OBSERVED FACT for discovery logic; runtime behavior for new files is UNKNOWN.

### Finding
The active pipeline does not establish fully schema-agnostic ingestion. Customers, Products, Expenses, and Payments expand fixed column lists. Sales handles observed header variants through explicit replacement rules and filename-derived dates, but its full 562-variant source coverage has not been executed.

### Evidence
- `expressions.tmdl`.
- Header-variant scan of `01_SourceData/Sales/` and `01_SourceData/Payments/`.

### Classification
OBSERVED FACT plus UNKNOWN full coverage.

### Finding
Changing `pDataFolderPath` is the implemented path-control mechanism. Dynamic workbook-driven column mapping is not active. Validation rules are not active in the current model definition.

### Evidence
- `expressions.tmdl`.
- `ColumnMappingConfig.xlsx` and `ValidationRulesConfig.xlsx`.
- `TMDLScripts/Script 1.tmdl`.

### Classification
OBSERVED FACT

### Finding
Transformations are distributed across a shared sales function, family staging queries, dimension partitions, and fact partitions. No active reusable validation function or active workbook-driven transformation framework was found.

### Evidence
- `expressions.tmdl`.
- Final table TMDL partitions.

### Classification
OBSERVED FACT

## 18. Documentation vs Implementation

### Finding
The creator-provided history documents a target sequence including Python generation, folder-distributed data, folder ingestion, parameterization, cleaning, staging, final tables, semantic model, and dashboard. The active filesystem confirms the folder ingestion, parameter, cleaning, staging, final-table, model, and report stages, but not the original Python implementation because no Python file is present.

### Evidence
- Creator-provided history.
- `expressions.tmdl`.
- `model.tmdl`.
- `definition/` report files.
- Recursive project inventory showing no `.py` file.

### Classification
CREATOR-PROVIDED HISTORY plus OBSERVED FACT.

### Finding
The two configuration workbooks document mapping and validation concepts, but active implementation does not consume them. The stale TMDL script contains mapping-workbook logic, so the project contains evidence of an alternate or historical configuration-driven stage that is not part of the current active definition.

### Evidence
- `02_Configuration/ColumnMappingConfig.xlsx`.
- `02_Configuration/ValidationRulesConfig.xlsx`.
- `TMDLScripts/Script 1.tmdl`.
- Active `definition/expressions.tmdl`.

### Classification
OBSERVED FACT; alternate/historical interpretation is INFERENCE.

### Finding
The parent project contains both `01_SourceData` and `Simulated Raw Data`, but only `01_SourceData` is named by the active parameter. The archive and alternate tree may be historical or distribution artifacts; their exact role is not established.

### Evidence
- `expressions.tmdl`, `pDataFolderPath`.
- `Simulated Raw Data/`.
- `Bakery_POS_Data_3Years.zip`.

### Classification
OBSERVED FACT plus UNKNOWN historical role.

### Finding
The current report contains page definitions for Customer and Operation without current visual JSON files. The documentation/source history does not establish whether these pages were intentionally empty, cleared, or incomplete.

### Evidence
- `pages.json`.
- `Customer/page.json`.
- `Operation/page.json`.
- Visual inventory.

### Classification
OBSERVED FACT plus UNKNOWN historical explanation.

## 19. Development History / Implementation Boundary

### Implemented / Actually Built

#### Finding
The active filesystem demonstrates a nested PBIP/PBIR/TMDL Power BI solution, folder-based Power Query ingestion from `01_SourceData`, staging queries, final dimensions/facts, a semantic model, deployed DAX measures, and a PBIR report.

#### Evidence
- `05_PowerBI/Bakery_Analytics.pbip`
- `05_PowerBI/Bakery_Analytics.Report/definition/`
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/`
- `01_SourceData/`

#### Classification
IMPLEMENTED

#### Finding
Creator-provided history states that the business scenario was explored with an AI model, the AI model helped create the Python generator, and the generated data was distributed into source folders. The current project contains the resulting CSV folder structure but not the Python generator itself.

#### Evidence
- Creator-provided history.
- `01_SourceData/`
- `Simulated Raw Data/`
- No `.py` file under the project.

#### Classification
CREATOR-PROVIDED HISTORY; resulting source artifacts OBSERVED; generator implementation UNKNOWN.

#### Finding
Folder-based ingestion, the `pDataFolderPath` parameter, cleaning/transformation logic, staging expressions, final loaded tables, the semantic model, DAX, and the Power BI report are implemented in the active project definition.

#### Evidence
- `expressions.tmdl`
- `model.tmdl`
- `definition/tables/*.tmdl`
- `definition/relationships.tmdl`
- `Bakery_Analytics.Report/definition/`

#### Classification
IMPLEMENTED

### Designed / Planned but Not Implemented

#### Finding
Configuration-driven column mapping and validation are represented by workbooks and a separate TMDL script, but are not implemented in the current active model definition. They are therefore treated as planned, alternate, or historical concepts rather than active functionality.

#### Evidence
- `02_Configuration/ColumnMappingConfig.xlsx`
- `02_Configuration/ValidationRulesConfig.xlsx`
- `TMDLScripts/Script 1.tmdl`
- Active `definition/expressions.tmdl` has no corresponding workbook references.

#### Classification
PLANNED / NOT IMPLEMENTED in the active definition.

#### Finding
A separate, fully reusable schema-validation framework, automatic validation logging, and configuration-driven rejection/flagging workflow are not established as implemented. The active M contains cleaning and parsing logic, but not the workbook rule engine described by the validation workbook.

#### Evidence
- Active `expressions.tmdl`.
- `ValidationRulesConfig.xlsx`.
- Active table inventory.

#### Classification
PLANNED / NOT IMPLEMENTED or UNKNOWN; active cleaning is IMPLEMENTED.

### Historical Context

#### Finding
The exact historical sequence connecting the AI-assisted scenario, Python generation, source-folder distribution, the alternate `Simulated Raw Data` tree, ZIP archive, and active `01_SourceData` path cannot be fully recovered from the current files.

#### Evidence
- Creator-provided history.
- `01_SourceData/`
- `Simulated Raw Data/`
- `Simulated Raw Data/Bakery_POS_Data_3Years.zip`
- No local Python generator.

#### Classification
CREATOR-PROVIDED HISTORY / UNKNOWN.

#### Finding
Whether the PBIX, PDF export, PBIP, stale TMDL script, configuration workbooks, and active PBIR/TMDL definitions represent one synchronized final state is UNKNOWN.

#### Evidence
- `05_PowerBI/Bakery_Analytics.pbix`
- `04_Exports/Bakery_Analytics.pdf`
- `05_PowerBI/Bakery_Analytics.pbip`
- `TMDLScripts/Script 1.tmdl`
- Configuration workbooks and active definition files.

#### Classification
UNKNOWN / HISTORICAL RECONSTRUCTION BOUNDARY.

## 20. Data Quality & Reproducibility

### Finding
The active source families show intentional structural variation in Sales headers and a smaller Payments header variation. The active M contains explicit cleaning for those variations, but the full refresh result was not tested.

### Evidence
- Header scan of `01_SourceData/Sales/` and `01_SourceData/Payments/`.
- `expressions.tmdl`, `fnclean_sales` and family staging expressions.

### Classification
OBSERVED FACT; runtime outcome UNKNOWN.

### Finding
The active Sales header function contains a type inconsistency: `fnclean_sales` converts `PaymentMethod` to text, while `FactSalesHeader` groups `PaymentMethod` with `Int64.Type`. This is a static implementation observation, not a confirmed refresh failure.

### Evidence
- `expressions.tmdl`, `fnclean_sales`.
- `FactSalesHeader.tmdl`, `Table.Group` definition.

### Classification
POTENTIAL CONCERN.

### Finding
The model uses left-outer joins for key lookups and explicit unknown rows for some dimensions. Actual null-key counts, unmatched-key counts, duplicate business IDs after snapshot selection, and referential integrity were not exhaustively profiled.

### Evidence
- Final fact TMDL partitions.
- `stgCustomer_Combined`, `stgProduct_Combined`, and `DimLocation` expressions.
- No full refresh/profile was performed.

### Classification
UNKNOWN runtime data quality.

### Finding
The active date dimension is generated in M for `2022-01-01` through `2024-12-31`. No incremental-refresh policy was observed, and the parameter/source structure is machine/path dependent.

### Evidence
- `DimDate.tmdl`.
- `model.tmdl`.
- `expressions.tmdl`.

### Classification
OBSERVED FACT; reproducibility impact is a POTENTIAL GAP.

### Finding
The project is not fully reproducible from the current project folder alone because the Python generator is absent, the active parameter uses an absolute local path, and the active runtime/database/service state was not tested. The CSV source files are present locally.

### Evidence
- No `.py` file under the project.
- `expressions.tmdl`, `pDataFolderPath`.
- `01_SourceData/`.

### Classification
OBSERVED FACT and POTENTIAL GAP.

## 21. Gaps / Unknowns

### Confirmed Gaps

- No Python generation script is present inside the Bakery project.
- Active M does not consume `ColumnMappingConfig.xlsx` or `ValidationRulesConfig.xlsx`.
- Active M does not reference `Simulated Raw Data` or the ZIP archive.
- Customer and Operation page definitions have no current visual JSON files.
- The active model contains no explicit validation log/rejected-row/error table driven by `ValidationRulesConfig.xlsx`.
- The active source parameter is an absolute local path: `D:\Power BI\PowerBI_Bakery_Project\01_SourceData`.
- The stale TMDL script references an obsolete `D:\Power BI\X PowerBI_Bakery_Project\...` path.

### Potential Gaps

- Full refresh may fail or coerce data because of source header variation and the `PaymentMethod` type inconsistency in `FactSalesHeader`.
- Files added to source folders may fail if they do not meet fixed schema/filename/content assumptions.
- Left-outer joins may produce null surrogate keys when source keys do not match dimensions.
- The PDF/PBIX may not match the current PBIP/PBIR/TMDL state.
- Snapshot deduplication based on filename ordering may not correspond to business-effective dates if filenames are not authoritative.

### Unknown

- Actual refresh success, query folding, runtime DAX correctness, report rendering, and interaction behavior.
- Exact Python generator code, random seeds, source-generation rules, and historical execution sequence.
- Exact role of every CSV after generation/distribution and any relationship between the two source trees.
- ZIP archive contents and whether they differ from current source folders.
- Contents of the DOCX and PDF beyond their file identity.
- PBIX internal metadata and synchronization with PBIP.
- Runtime cardinality/cross-filter behavior beyond what TMDL explicitly serializes.
- Actual source referential integrity, null rates, malformed-row outcomes, and validation results.
- Whether Customer and Operation pages were historically populated or intentionally left empty.
- Service publication, usage, or downstream lineage.

## 22. Portfolio-Relevant Facts

- Nested Power BI PBIP project under `05_PowerBI/Bakery_Analytics.pbip`.
- Thick PBIP report/model binding through `definition.pbir` and local semantic-model path.
- Folder-based Power Query ingestion controlled by a required source-folder parameter.
- Five source file families: Sales, Payments, Customers, Products, and Expenses.
- 4,082 CSV artifacts across active/alternate source trees and configuration data.
- Active source location contains 1,096 daily Sales files, 1,096 daily Payments files, 36 monthly Customer snapshots, 36 monthly Product snapshots, and 36 monthly Expense snapshots.
- Sales ingestion includes a custom function for junk-line removal, header normalization, numeric cleaning, and type conversion.
- Active model has staging queries, six dimensions, four primary fact tables, a payment-aging helper table, and a centralized 50-measure table.
- Sales is modeled as header and line facts; Payments and Expenses are separate facts.
- Model contains 11 relationship declarations and one inactive SalesLine-to-Date relationship.
- Implemented analytical areas include income, expenses, profit margin, cash flow, payments received/pending, collection rate, payment fees, reconciliation status, payment aging, time comparisons, and customer/product/location analysis.
- Configuration workbooks and a stale TMDL script document mapping/validation concepts, but they are not consumed by the active model definition.
- Creator-provided history identifies AI-assisted scenario exploration and Python-generated synthetic data, but the Python generator is not present in this project folder.

## 23. Evidence Index

### Project boundary and PBIP

- `05_PowerBI/Bakery_Analytics.pbip` - PBIP entry point.
- `05_PowerBI/Bakery_Analytics.Report/definition.pbir` - report/model binding.
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition.pbism` - semantic-model entry point.
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/model.tmdl` - query groups, query order, model metadata, table references.

### Power Query and lineage

- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/expressions.tmdl` - parameter, sales function, staging queries, and M transformations.
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/tables/FactSalesHeader.tmdl` - grouped sales header transformation.
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/tables/FactSalesLine.tmdl` - sales line transformation and lookups.
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/tables/FactPayment.tmdl` - payment fact transformation.
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/tables/FactExpense.tmdl` - expense fact transformation.
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/tables/DimLocation.tmdl` - location dimension from sales staging.
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/tables/DimDate.tmdl` - generated date dimension.
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/relationships.tmdl` - relationship endpoints and inactive relationship.

### Semantic model and DAX

- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/tables/*.tmdl` - all active model tables, columns, partitions, calculated objects, and metadata.
- `05_PowerBI/Bakery_Analytics.SemanticModel/definition/tables/_Measures.tmdl` - deployed measures.
- `05_PowerBI/Bakery_Analytics.SemanticModel/DAXQueries/Query 1.dax` - DAX query-view artifact, not executed.
- `05_PowerBI/Bakery_Analytics.SemanticModel/TMDLScripts/Script 1.tmdl` - alternate/stale script with mapping workbook reference and obsolete path.

### Source and configuration

- `01_SourceData/Sales/` - active Sales file family.
- `01_SourceData/Payments/` - active Payments file family.
- `01_SourceData/Customers/` - active Customer snapshots.
- `01_SourceData/Products/` - active Product snapshots.
- `01_SourceData/Expenses/` - active Expense snapshots.
- `02_Configuration/DimDate.csv` - configuration-side date CSV; active model generates date dimension in M.
- `02_Configuration/ColumnMappingConfig.xlsx` - mapping workbook, not active model input.
- `02_Configuration/ValidationRulesConfig.xlsx` - validation workbook, not active model input.
- `Simulated Raw Data/` - alternate/duplicate source tree and archive.
- `Simulated Raw Data/Bakery_POS_Data_3Years.zip` - archive, not inspected.

### Documentation and report

- `03_Documentation/It standardizes all files into a single clean output regardless of.docx` - documentation artifact, not parsed.
- `04_Exports/Bakery_Analytics.pdf` - report export, not reverse-engineered.
- `05_PowerBI/Bakery_Analytics.Report/definition/pages/pages.json` - page order.
- `05_PowerBI/Bakery_Analytics.Report/definition/pages/*/page.json` - page names, visibility, type, and canvas metadata.
- `05_PowerBI/Bakery_Analytics.Report/definition/pages/*/visuals/*/visual.json` - visual types, fields, measures, filters, and interaction metadata.
- `05_PowerBI/Bakery_Analytics.Report/definition/report.json` - themes, custom visual, and report settings.

## 24. Audit Limitations

- This is a local, read-only file reconstruction.
- No Power BI Desktop refresh or report rendering was performed.
- No SQL/database/service connection was made.
- No DAX query or M query was executed.
- The 4,082 CSV files were inventoried by family and representative structure; they were not exhaustively profiled row by row.
- XLSX workbook sheets and representative rows were inspected read-only, but no workbook was used as an active model source unless active M evidence supported it.
- The DOCX, PDF, PBIX, and ZIP contents were not reverse-engineered.
- The creator-provided development history is explicitly separated from filesystem evidence.
- No claim is made that the report renders correctly, refreshes successfully, or matches the PBIX/PDF artifact.
