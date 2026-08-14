# DAX Training Dataset - RetailPulse Academy

This synthetic retail dataset is designed for DAX practice in Power BI Desktop and DAX Studio.
It is intentionally broader than a tidy little sample model, because real BI work apparently enjoys hiding traps under every table. 🙃

## Files included

- `FactSales.csv` - 60,000 sales order lines from 2022-01-01 to 2025-12-31.
- `DimDate.csv` - full calendar with fiscal, ISO week, month and working-day columns.
- `DimProductCategory.csv`, `DimProductSubcategory.csv`, `DimProduct.csv` - product hierarchy.
- `DimCustomer.csv`, `DimGeography.csv`, `DimStore.csv`, `DimEmployee.csv` - core business dimensions.
- `FactReturns.csv`, `FactInventorySnapshot.csv`, `FactBudget.csv`, `FactExchangeRate.csv`, `FactFinanceLedger.csv`, `FactCustomerSatisfaction.csv`, `FactWebSession.csv` - extra facts for practical DAX scenarios.
- `DimAccount.csv` - parent-child account hierarchy with unary signs.
- `DimInterest.csv`, `BridgeCustomerInterest.csv`, `BridgeProductInterest.csv` - many-to-many bridge practice.
- Disconnected/helper tables: `DimSegmentBands.csv`, `WhatIfDiscount.csv`, `WhatIfTopN.csv`, `CurrencySelector.csv`, `MetricSelector.csv`.
- `Relationships_To_Create.csv` - relationship setup guide.
- `DataDictionary_Tables.csv`, `DataDictionary_Columns.csv` - metadata.
- `Exercise_Map.csv` - maps dataset areas to DAX learning topics.
- `DAX_Starter_Measures.dax` - starter measures.

## Power BI import steps

1. In Power BI Desktop, use **Get data > Text/CSV** or **Folder** and import all CSV files.
2. Set data types:
   - `*DateKey`, `MonthStartDateKey`, `ProductKey`, `CustomerKey`, etc. as Whole Number.
   - `Date`, `BirthDate`, `JoinDate`, `OpenDate`, `AvailableForSaleDate` as Date.
   - Amount/rate columns as Decimal Number.
3. Create relationships listed in `Relationships_To_Create.csv`.
4. Mark `DimDate` as the Date table using `DimDate[Date]`.
5. Keep these relationships inactive:
   - `DimDate[DateKey]` -> `FactSales[ShipDateKey]`
   - `DimDate[DateKey]` -> `FactSales[DeliveryDateKey]`
   Use `USERELATIONSHIP` in measures.
6. Do not relate disconnected tables unless a specific exercise says to use `TREATAS` or selection logic.

## Main model grain

- `FactSales`: one row per order line.
- `FactBudget`: one row per month, store and product category.
- `FactInventorySnapshot`: one row per month-end, store and product.
- `FactExchangeRate`: one row per date and currency.
- `FactFinanceLedger`: one row per finance posting.
- `FactWebSession`: one row per web session.

## Suggested first validation measures

```DAX
Sales Amount := SUM ( FactSales[SalesAmount] )
Orders := DISTINCTCOUNT ( FactSales[OrderNumber] )
Gross Margin := [Sales Amount] - SUM ( FactSales[CostAmount] )
Gross Margin % := DIVIDE ( [Gross Margin], [Sales Amount] )
```

Build a matrix by `DimDate[Year]` and `DimProductCategory[ProductCategoryName]`.
If totals make sense, congratulations: the model has not yet betrayed you.
