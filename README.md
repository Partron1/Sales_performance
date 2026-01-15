## Sales Performance Overview
Sales experienced a steady decline in over the past three years, which has raised concerns. As a data analyst, I was tasked with analyzing three years of historical sales data to uncover insights and identify potential causes

### Objective
To design and develop a dynamic, visually engaging, and informative Sales Dashboard in Excel that empowers stakeholders to monitor performance, identify trends, and make data-driven decisions across products, regions, and timeframes.

### Business Questions
•	How has sales performed over the past years?

•	Which regions have the highest/lowest sales, and what factors contribute to these trends?

•	Can we predict future sales based on historical trends coupling strategic marketing implementations?
   
### Data Description
This is an e-commerce data  sourced from Kaggle and structured in a .csv file. 

### Plan 
•	Data Cleaning/Processing: Excel and Power Query

•	Analysis and Dashboard design: Excel 

### Cleaning Steps 
Power query;

```bigquery claining steps
# Define dataset
= Excel.CurrentWorkbook(){[Name="Sales"]}[Content]
```
```
# Transformed data into data type
= Table.TransformColumnTypes(Source,{{"ORDERNUMBER", Int64.Type}, {"QUANTITYORDERED", Int64.Type}, {"PRICEEACH", type number}, {"ORDERLINENUMBER", Int64.Type}, {"SALES", type number}, {"ORDERDATE", type text}, {"STATUS", type text}, {"QTR_ID", Int64.Type}, {"MONTH_ID", Int64.Type}, {"YEAR_ID", Int64.Type}, {"PRODUCTLINE", type text}, {"MSRP", Int64.Type}, {"PRODUCTCODE", type text}, {"CUSTOMERNAME", type text}, {"PHONE", type any}, {"ADDRESSLINE1", type text}, {"ADDRESSLINE2", type text}, {"CITY", type text}, {"STATE", type text}, {"POSTALCODE", type any}, {"COUNTRY", type text}, {"TERRITORY", type text}, {"CONTACTLASTNAME", type text}, {"CONTACTFIRSTNAME", type text}, {"DEALSIZE", type text}})
```
```
# Removed errors
= Table.RemoveRowsWithErrors(#"Changed Type")
```
```
# Added extra columns to calculate cost price and profit
= Table.AddColumn(#"Removed Errors", "cost_price", each [QUANTITYORDERED]*[PRICEEACH])
= Table.AddColumn(#"Added Custom", "profit", each [SALES]-[cost_price])
```
```
# Tranformed the quarter ID column by adding a text Q to make it meaningful
= Table.TransformColumns(#"Added Custom1", {{"QTR_ID", each "Q" & Text.From(_, "en-GH"), type text}})
```
```
# Changed the month ID into an actual month text
= Table.AddColumn(#"Added Prefix", "month", each Date.ToText(#date(2025, [MONTH_ID], 1), "MMMM"))
```
```
# Renamed columns
=Table.RenameColumns(#"Added Custom2",{{"ORDERNUMBER", "order_no"}, {"QUANTITYORDERED", "quantity_ordered"}, {"PRICEEACH", "unit_price"}, {"ORDERLINENUMBER", "order_line_no"}, {"SALES", "sales"}, {"ORDERDATE", "date"}, {"STATUS", "delivery_status"}, {"QTR_ID", "quarter_id"}, {"MONTH_ID", "month_id"}, {"YEAR_ID", "year"}, {"PRODUCTLINE", "product"}, {"MSRP", "mrsp"}, {"PRODUCTCODE", "product_code"}, {"CUSTOMERNAME", "customer"}, {"PHONE", "phone"}, {"ADDRESSLINE1", "address1"}, {"ADDRESSLINE2", "address2"}, {"CITY", "city"}, {"STATE", "state"}, {"POSTALCODE", "post_code"}, {"COUNTRY", "country"}, {"TERRITORY", "territory"}, {"CONTACTLASTNAME", "contact_last_name"}, {"CONTACTFIRSTNAME", "contact_first_name"}, {"DEALSIZE", "deal_size"}})
```
```
# Merged columns
=Table.AddColumn(#"Renamed Columns", "contact_name", each [contact_last_name] & " " & [contact_first_name])
```
```
# Removed columns
= Table.RemoveColumns(#"Added Custom3",{"contact_last_name", "contact_first_name"})
```
```
# Truncated month text to first 3 letters
= Table.TransformColumns(#"Removed Columns", {{"month", each Text.Start(_, 3), type text}})
```
```
# Reordered columns
= Table.ReorderColumns(#"Extracted First Characters",{"contact_name", "customer", "order_no", "order_line_no", "quantity_ordered", "unit_price", "cost_price", "profit", "sales", "date", "year", "delivery_status", "quarter_id", "month_id", "product", "mrsp", "product_code", "phone", "address1", "address2", "city", "state", "post_code", "country", "territory", "deal_size", "month"})
```
```
# Removed columns
= Table.RemoveColumns(#"Reordered Columns",{"date", "month_id"})
```
```
# Reordered columns
=Table.ReorderColumns(#"Removed Columns1",{"customer", "contact_name", "product", "order_no", "order_line_no", "quantity_ordered", "unit_price", "cost_price", "sales", "profit", "year", "month", "quarter_id", "mrsp", "product_code", "phone", "address1", "address2", "post_code", "country", "territory", "state", "city", "delivery_status", "deal_size"})
```
```
# Replaced values
= Table.ReplaceValue(#"Reordered Columns1","Motorcycles","Motorcycle",Replacer.ReplaceText,{"product"})
= Table.ReplaceValue(#"Replaced Value","Classic Cars","Classic Car",Replacer.ReplaceText,{"product"})
= Table.ReplaceValue(#"Replaced Value1","Planes","Plane",Replacer.ReplaceText,{"product"})
= Table.ReplaceValue(#"Replaced Value1","Planes","Plane",Replacer.ReplaceText,{"product"})
= Table.ReplaceValue(#"Replaced Value2","Ships","Ship",Replacer.ReplaceText,{"product"})
= Table.ReplaceValue(#"Replaced Value3","Trucks and Buses","Truck & Bus",Replacer.ReplaceText,{"product"})
= Table.ReplaceValue(#"Replaced Value4","Vintage Cars","Vintage Car",Replacer.ReplaceText,{"product"})
```
```
# Tranformed columns
= Table.TransformColumns(#"Replaced Value5",{{"customer", Text.Proper, type text}, {"contact_name", Text.Proper, type text}, {"product", Text.Proper, type text}, {"address1", Text.Proper, type text}, {"delivery_status", Text.Proper, type text}, {"deal_size", Text.Proper, type text}})
=Table.TransformColumns(#"Capitalized Each Word",{{"customer", Text.Trim, type text}, {"contact_name", Text.Trim, type text}, {"product", Text.Trim, type text}, {"address1", Text.Trim, type text}, {"delivery_status", Text.Trim, type text}, {"deal_size", Text.Trim, type text}})
= Table.TransformColumns(#"Trimmed Text",{{"customer", Text.Clean, type text}, {"contact_name", Text.Clean, type text}, {"product", Text.Clean, type text}, {"address1", Text.Clean, type text}, {"delivery_status", Text.Clean, type text}, {"deal_size", Text.Clean, type text}})
= Table.TransformColumnTypes(#"Cleaned Text",{{"unit_price", Currency.Type}, {"cost_price", Currency.Type}, {"sales", Currency.Type}, {"profit", Currency.Type}})
```
```
# Renamed columns
= Table.RenameColumns(#"Changed Type1",{{"mrsp", "msrp"}})
```
```
# Transformed column
= Table.TransformColumnTypes(#"Renamed Columns1",{{"msrp", Currency.Type}})
```
```
# Removed columns
= Table.RemoveColumns(#"Changed Type2",{"phone", "address2", "address1"})
```
**Conditional formating**

This is a great way to better understand data by highlightining partterns, outliers or thresholds

![Conditional_formatting](images/data_inspection.png)

### Exploratory Data Analysis (EDA)

- Key performance indicators (KPIs) including **total sales, total profit, total quantity and year-over-year(YoY%**) was established.
  
![EDA](images/eda.png)

### Dashboard  
A dynamic dashboard that lists and describe key plots: line chart,bar chart and histogram.
#### Target Audience
- *Sales Managers: To monitor team and individual performance.*
- *Head of Sales: To assess strategic sales performance.*
- *Regional Sales Reps: To understand their performance and areas of opportunity.*
- *Marketing Team: To identify campaign impact on sales.*

![Dashboard](images/Dashboard.png)


This dashboard provides a comprehensive view of overall sales performance through a structured layout that prioritizes key metrics for quick decision-making.

**KPIs (Top-Left Corner):**
- The most important metrics: **Sales ($10.0M), Profit ($1.7M), and Quantity ($99.1K)** are displayed prominently at the top-left. They are highlighted with bold text, distinct font colors, and year-over-year performance indicators to immediately show current performance and trends.

**Sales Trend (Center):**
- A line chart tracks **monthly sales performance**, helping to identify growth patterns, seasonality, and key peaks (e.g., November at $2.1M).

**Sales by Item (Left Section):**
- A horizontal bar chart breaks down sales by product category, showing that **Classic Cars ($3.9M)** are the top contributor, followed by Vintage Cars and Motorcycles.

**Top 5 Selling Cities (Center-Bottom):**
- A bar chart highlights leading markets, with **Madrid ($1.1M)** leading, followed by San Rafael and New York City.

**Delivery Status (Top-Right):**
- A progress breakdown of order fulfillment shows **93% of items shipped**, with minimal delays (2% cancelled, 2% on hold, 1% in process).

**Sales by Territory (Bottom-Right):**
- Regional sales performance is compared, where **EMEA (\$5.0M)** is the largest market, ahead of North America (\$3.9M), APAC, and Japan.

**Navigation Controls (Top-Right):**
- Buttons allow filtering by **company size (Large, Medium, Small)** and by **quarter (Q1–Q4)**, making the dashboard interactive and customizable for different business views.

**Summary:**
This dashboard is designed to provide executives with a **clear, top-level snapshot of sales performance**, enabling them to quickly evaluate overall KPIs, monitor trends, identify top products and markets, assess delivery performance, and compare regional sales. Its structured design ensures that the most critical insights are easy to spot at a glance, while supporting deeper exploration through filters and breakdowns.

### Key Findings

**Overall Sales & Profit Decline**

- Sales ($10.0M) and Profit ($1.7M) are both down by over 60% YoY, indicating a significant performance drop.

**Strong Product Category Dependence**

- Classic Cars ($3.9M) dominate sales, contributing far more than any other category, which signals high dependency on a single product line.

**Top Markets Identified**

- Madrid ($1.1M), San Rafael, and New York City are the strongest cities in terms of revenue, showing geographic concentration of sales.

**Delivery Performance is Strong**

- 93% of orders are shipped successfully, with very low levels of cancellations, delays, or issues.

**Regional Performance Gaps**

- EMEA ($5.0M) is the leading territory, while APAC and Japan significantly underperform (< $1M combined), indicating uneven regional distribution.

### Recommendations

**Address Sales & Profit Decline**

- Investigate the cause of the sharp YoY drop — possible pricing pressures, reduced demand, or competitive threats.

- Introduce promotional campaigns or bundling strategies to boost sales volumes.

**Diversify Product Portfolio**

- Reduce dependency on Classic Cars by promoting underperforming categories (e.g., Motorcycles, Planes, and Ships).

- Consider product innovation or expansion to balance revenue streams.

**Leverage Top-Performing Cities**

- Double down on successful markets like Madrid and New York City with targeted marketing campaigns.

- Explore what drives demand in these cities and replicate strategies in weaker markets.

**Expand Regional Opportunities**

- Strengthen presence in APAC and Japan, possibly through localized marketing, partnerships, or better distribution networks.

- Reassess pricing strategies and customer preferences in these regions.

**Maintain High Delivery Standards**

- With 93% success in shipping, logistics and supply chain are a strength. Continue investing in operational efficiency to maintain customer trust.

### Summary Insight:
- The business is experiencing a sharp decline in overall performance, despite strong logistics and a few high-performing markets. By diversifying the product mix, expanding regional coverage, and replicating city-level successes, the company can stabilize and return to growth.


