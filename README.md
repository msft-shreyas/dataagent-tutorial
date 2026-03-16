# Data Agent Tutorial Fabcon ATL 2026
This repository contains all the content required for the Data Agent Tutorial at Fab Con ATL 2026. 

## Q&A During Tutorial
Please submit any and all questions during the tutorial to: [https://aka.ms/dataagent-qa](https://aka.ms/dataagent-qa)


## Updates for Lab 2 Failing example query: 
```
SELECT --Select customer city and alias as City
    c.[Customer City] AS City,
    -- Select customer state and alias as State
    c.[Customer State] AS State,
    -- Calculate the average Freight Value for each city-state group
    AVG(s.[Freight Value]) AS AvgFreightCost,
    -- Calculate the difference between the city-state average freight cost and the overall average freight cost
    AVG(s.[Freight Value]) - oa.OverallAvgFreight AS DifferenceFromOverallAvg,
    -- Calculate the percentage deviation from the overall average freight cost,
    -- handling division by zero by returning NULL when overall average freight is zero
    CASE
        WHEN oa.OverallAvgFreight = 0 THEN NULL
        ELSE (AVG(s.[Freight Value]) - oa.OverallAvgFreight) / oa.OverallAvgFreight * 100
    END AS DeviationPercentage
FROM
    -- Sales table alias as s
    [fda].[Sales] s
    -- Join with Customers table alias as c on Customer ID to get city and state info
    INNER JOIN [fda].[Customers] c ON s.[Customer ID] = c.[Customer ID]
    -- Cross join with a derived table calculating the overall average Freight Value from all sales where the freight value is not null
    CROSS JOIN (
        SELECT
            AVG([Freight Value]) AS OverallAvgFreight
        FROM [fda].[Sales]
        WHERE [Freight Value] IS NOT NULL
    ) oa
WHERE
    -- Filter out sales records where Freight Value is NULL
    s.[Freight Value] IS NOT NULL
GROUP BY
    -- Group the results by Customer City and State to compute aggregates per city-state combination
    c.[Customer City],
    c.[Customer State],
    -- Include the overall average freight in GROUP BY because it is referenced in SELECT (though it will be the same for all rows)
    oa.OverallAvgFreight
ORDER BY
    -- Order the final results alphabetically by State, then City
    c.[Customer State],
    c.[Customer City]
```
