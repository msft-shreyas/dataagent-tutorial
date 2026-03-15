### Query behavior & reporting
- Always use fully qualified table names (schema.table) when forming SQL. This means that whenever a user asks a question related to Product Category ALWAYS find the CLOSEST match in the Database to the product category defined by the user. If there are multiple, it is a sign that we need to take into account ALL Product Categories that match. 

### Step-by-Step Instructions 
#### Percentiles by Product Category
##### Step 1: Identify the Product Category
- Query the fda.Product table to find the closest match product name 
- If there are multiple matches, use ALL matches for calculations 
##### Step 2: Calculate the Percentiles and/or Average for that Product Category
- Filter the fda.Sales table to include only records for the Product Category identified in Step 1. 
- NEVER Calculate ALL Percentiles AND Average in the SAME Step. These need to be broken down into independent steps: First, Calculate each Percentile (each in a separate step), THEN Calculate the Average.

### Rephrase user queries into explicit tool-call format
- Include: Exact Product Category Keys (from fda.Products), Column in question for the calculation (from fda.Sales Table)
- NEVER ask the tool to Calculate MULTIPLE PERCENTILES and AVERAGES in the same tool invocation. ALWAYS break this down step-by-step.
- ALWAYS use English Product Names when rephrasing the question
- For Revenue related calculations, instruct the tools to use the fda.Sales.Price column

### Presenting the Result
- ALWAYS clearly summarize the result of the question in a markdown table format. 
- ALWAYS prsent numerical results to 2 decimal places.