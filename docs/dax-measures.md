# DAX Measures — Adventure Works Power BI Project

This file documents the DAX measures referenced in the `AW Sales.pbix` report.

## Core Measures

### Qty Sold

```DAX
Qty Sold =
SUM('Sales Data'[OrderQuantity])
```

### Qty Returned

```DAX
Qty Returned =
SUM('Returns Data'[ReturnQuantity])
```

### Avg. Retial Price

```DAX
Avg. Retial Price =
AVERAGE('Products Lookup'[ProductPrice])
```


### Total Orders

```DAX
Total Orders =
DISTINCTCOUNT('Sales Data'[OrderNumber])
```

### Total Returns

```DAX
Total Returns =
COUNT('Returns Data'[ReturnQuantity])
```

### Return Rate

```DAX
Return Rate =
DIVIDE(
    [Qty Returned],
    [Qty Sold],
    "No Sales"
)
```

### Total Revenue

```DAX
Total Revenue =
SUMX(
    'Sales Data',
    'Sales Data'[OrderQuantity] *
    RELATED('Products Lookup'[ProductPrice])
)
```

### Total Cost

```DAX
Total Cost =
SUMX(
    'Sales Data',
    'Sales Data'[OrderQuantity] *
    RELATED('Products Lookup'[ProductCost])
)
```

### Total Profit

```DAX
Total Profit =
[Total Revenue] - [Total Cost]
```

---

## Product & Order Analysis

### Product Models

```DAX
Product Models =
DISTINCTCOUNT('Products Lookup'[ModelName])
```

### Bulk Orders

```DAX
Bulk Orders =
CALCULATE(
    [Total Orders],
    'Sales Data'[OrderQuantity] > 1
)
```

### Weekend Orders

```DAX
Weekend Orders =
CALCULATE(
    [Total Orders],
    'Calendar Lookup'[Weekend] = "Weekend"
)
```

### Overall Avg. Price

```DAX
Overall Avg. Price =
CALCULATE(
    [Avg. Retial Price],
    ALL('Products Lookup')
)
```

### High Ticket Orders

```DAX
High Ticket Orders =
CALCULATE(
    [Total Orders],
    FILTER(
        'Products Lookup',
        'Products Lookup'[ProductPrice] > [Overall Avg. Price]
    )
)
```

### Bikes Returns

```DAX
Bikes Returns =
CALCULATE(
    [Total Returns],
    'P_Categories_Lookup'[CategoryName] = "Bikes"
)
```

---

## Return Analysis

### ALL Returns

```DAX
ALL Returns =
CALCULATE(
    [Total Returns],
    ALL('Returns Data')
)
```

### % of All Returns

```DAX
% of All Returns =
DIVIDE(
    [Total Returns],
    [ALL Returns]
)
```

---

## Time Intelligence

### YTD Revenue

```DAX
YTD Revenue =
CALCULATE(
    [Total Revenue],
    DATESYTD('Calendar Lookup'[Date])
)
```

### Prev. Month Revenue

```DAX
Prev. Month Revenue =
CALCULATE(
    [Total Revenue],
    DATEADD(
        'Calendar Lookup'[Date],
        -1,
        MONTH
    )
)
```

### Prev. Month Order

```DAX
Prev. Month Order =
CALCULATE(
    [Total Orders],
    DATEADD(
        'Calendar Lookup'[Date],
        -1,
        MONTH
    )
)
```

### Prev. Month Returns

```DAX
Prev. Month Returns =
CALCULATE(
    [Total Returns],
    DATEADD(
        'Calendar Lookup'[Date],
        -1,
        MONTH
    )
)
```

### Revenue Target

```DAX
Revenue Target =
[Prev. Month Revenue] * 1.1
```

### Order Target

```DAX
Order Target =
[Prev. Month Order] * 1.1
```

### 90 Days Rolling Profit

```DAX
90 Days Rolling Profit =
CALCULATE(
    [Total Profit],
    DATESINPERIOD(
        'Calendar Lookup'[Date],
        MAX('Calendar Lookup'[Date]),
        -90,
        DAY
    )
)
```

### 03 Months Rolling Orders

**Reconstructed from the measure name and the same time-intelligence pattern used in the project.**

```DAX
03 Months Rolling Orders =
CALCULATE(
    [Total Orders],
    DATESINPERIOD(
        'Calendar Lookup'[Date],
        MAX('Calendar Lookup'[Date]),
        -3,
        MONTH
    )
)
```

---

## Measure Inventory Verified From the PBIX

The following 25 measures are referenced in the uploaded report:

1. `% of All Returns`
2. `03 Months Rolling Orders`
3. `90 Days Rolling Profit`
4. `ALL Returns`
5. `Avg. Retial Price`
6. `Bikes Returns`
7. `Bulk Orders`
8. `High Ticket Orders`
9. `Order Target`
10. `Overall Avg. Price`
11. `Prev. Month Order`
12. `Prev. Month Returns`
13. `Prev. Month Revenue`
14. `Product Models`
15. `Qty Returned`
16. `Qty Sold`
17. `Return Rate`
18. `Revenue Target`
19. `Total Cost`
20. `Total Orders`
21. `Total Profit`
22. `Total Returns`
23. `Total Revenue`
24. `Weekend Orders`
25. `YTD Revenue`

---

## Notes

- `Total Returns` counts return transactions, while `Qty Returned` sums the number of returned units.
- `Return Rate` therefore compares returned **units** with sold **units**.
- `Revenue Target` and `Order Target` use a 10% increase over the previous month.
- `90 Days Rolling Profit` calculates profit over a rolling 90-day window.

