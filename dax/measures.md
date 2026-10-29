# DAX measures

Table names used below: `'e_commerce_data ecommerce_data'` is the sales table and `Calender` is the calendar (date) table.

## Calendar table

Creates one row for every date between the first and the last order date. It is used by all the time-intelligence measures.

```dax
Calender = CALENDAR(MIN('e_commerce_data ecommerce_data'[order_date]), MAX('e_commerce_data ecommerce_data'[order_date]))
```

## Base measure

```dax
Profit Margin = SUM('e_commerce_data ecommerce_data'[profit_per_order]) / SUM('e_commerce_data ecommerce_data'[sales_per_order])
```

## Year to date (YTD)

```dax
YTD Sales = TOTALYTD(SUM('e_commerce_data ecommerce_data'[sales_per_order]), Calender[Date])

YTD Profit = TOTALYTD(SUM('e_commerce_data ecommerce_data'[profit_per_order]), Calender[Date])

YTD Quantity = TOTALYTD(SUM('e_commerce_data ecommerce_data'[order_quantity]), Calender[Date])

YTD Profit Margin = TOTALYTD([Profit Margin], Calender[Date])
```

## Previous year to date (PYTD)

```dax
PYTD Sales = CALCULATE(SUM('e_commerce_data ecommerce_data'[sales_per_order]), DATESYTD(SAMEPERIODLASTYEAR(Calender[Date])))

PYTD Profit = CALCULATE(SUM('e_commerce_data ecommerce_data'[profit_per_order]), DATESYTD(SAMEPERIODLASTYEAR(Calender[Date])))

PYTD QTY = CALCULATE(SUM('e_commerce_data ecommerce_data'[order_quantity]), DATESYTD(SAMEPERIODLASTYEAR(Calender[Date])))

PYTD Profit Margin = CALCULATE([Profit Margin], DATESYTD(SAMEPERIODLASTYEAR(Calender[Date])))
```

## Year on year (YoY) growth

Each YoY measure is (YTD - PYTD) / PYTD.

```dax
YoY Sales = ([YTD Sales] - [PYTD Sales]) / [PYTD Sales]

YoY Profit = ([YTD Profit] - [PYTD Profit]) / [PYTD Profit]

YoY Quantity = ([YTD Quantity] - [PYTD QTY]) / [PYTD QTY]

YoY Profit Margin = ([YTD Profit Margin] - [PYTD Profit Margin]) / [PYTD Profit Margin]
```

`YoY Profit Margin` is a relative change (for example 11.58% vs 10.99%), not a difference in percentage points.
