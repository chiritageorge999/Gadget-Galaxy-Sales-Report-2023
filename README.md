

# Gadget-Galaxy-Sales-Report-2023

<iframe title="SALES_2023" width="800" height="836" src="https://app.powerbi.com/view?r=eyJrIjoiZWJjMTBhYzYtNjk2Mi00YjM1LThkZGEtMjAwOTBlYzUzYjMxIiwidCI6ImViOGZiNTVjLTcyMDEtNDE0Yy05MDdlLWVhYTAwMmZlOThhMCIsImMiOjN9" frameborder="0" allowFullScreen="true"></iframe>

## DAX

```
 Bottom Products Revenue =

 // Calculate the selected value from the 'Top N Selection' table

    Var topNselected = SELECTEDVALUE('Top N Selection'[Value])

// Select the top N products based on the selected value

    var topProdTable = 
        TOPN(
            topNselected,
            ALLSELECTED('Pseudo prod table'),
            [Total Revenue],ASC
            )

// Calculate the revenue for the top products while keeping filters from 'topProdTable'

    var topProdSales = 
        CALCULATE(
            [Total Revenue], 
            KEEPFILTERS(topProdTable)
            )

// Calculate the total revenue for all products except the top N products

    var otherSales = 
        CALCULATE(
            [Total Revenue],
             ALLSELECTED('Pseudo prod table')
        )
             - 
             CALCULATE(
                [Total Revenue],
                topProdTable
                ) 

// Get the current product name

    var currentProd = 
    SELECTEDVALUE('Pseudo prod table'[Product])

// Return the result based on whether the current product is "Others"

    RETURN
    IF(
        currentProd <> "Others",
        topProdSales,
        otherSales
        )
```
```
Bottom Selected Value = 

// Calculate the selected value from the 'Top N Selection' table

VAR SelectedValue = SELECTEDVALUE('Top N Selection'[Value], 0)

// Calculate the maximum value from the top N rows in the 'Top N Selection' table

RETURN
CALCULATE(
    MAXX(
        TOPN(
            SelectedValue,
            'Top N Selection',
            'Top N Selection'[Value],
            ASC
        ),
        'Top N Selection'[Value]
    )
)

```
```
Top 3 Months 

// Calculate the top 3 months based on total revenue (descending order)

// CONCATENATEX combines the names of these top 3 months into a comma-separated string.

CONCATENATEX(

// TOPN selects the top 3 months with the highest revenue.

    TOPN(
        3,

// SUMMARIZE groups the data by month and calculates the total revenue for each month.

        SUMMARIZE(
            Append1,
            'Calendar'[Month Name],
            "Revenue", [Total Revenue]
        ),
        [Revenue], DESC
    ),
    'Calendar'[Month Name],
    ", ",
    [Revenue], DESC
)
```







