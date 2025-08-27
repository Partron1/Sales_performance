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
•	Use excel and power query to clean and process data.

•	Use Excel to analyzed and design a compelling dashboard.

### Cleaning Steps 


  
### Loaded the dataset into power query;

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
*Apply conditional formating to understand our dataset*

<img width="959" height="486" alt="data_inspection" src="https://github.com/user-attachments/assets/83918869-a153-4767-ae6f-c48a9b74882a" />

### Exploratory Data Analysis (EDA)
- *Histogram was employed during the EDA stage to visualize data spread, identify anomalies, and support data cleaning decisions.* 
- Key performance indicators (KPIs) including total sales, total profit, total quantity and year-over-year(YoY%) was established.
  
<img width="958" height="487" alt="eda" src="https://github.com/user-attachments/assets/2339d769-d0fe-485c-8c59-2ed925d08b53" />

### Dashboard  
A dynamic dashboard that lists and describe key plots: line chart,bar chart and histogram.
#### Target Audience
- *Sales Managers: To monitor team and individual performance.*
- *Head of Sales: To assess strategic sales performance.*
- *Regional Sales Reps: To understand their performance and areas of opportunity.*
- *Marketing Team: To identify campaign impact on sales.*

<img width="796" height="377" alt="Dashboard" src="https://github.com/user-attachments/assets/3975d5b4-9719-4123-b969-b10a6e685441" />

### Key Findings
- The analysis showed a consistent decline in performance throughout the years leaving the YoY% precentage declining significantly.
- The overall performance of sales have been poor even within my metrics.

### Business Impact  
- With this informations, stakeholders can have better insights into the business performance, save time, reduce cost and device trategies to reverse this decline.

### Key Recommendations
- Items such as classic cars should be focused more to generate more revenue.
- Cut or negotiate underperforming channels and reallocate budget into to ROI vehicle campaigns or high-traffic partners.






