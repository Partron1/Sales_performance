## Sales Performance Overview
A fictional company has experienced a steady decline in sales over the past three years, which has raised concerns. As a data analyst, I was tasked with analyzing three years of historical sales data to uncover insights and identify potential causes

## Objective
To design and develop a dynamic, visually engaging, and informative Sales Dashboard in Excel that empowers stakeholders to monitor performance, identify trends, and make data-driven decisions across products, regions, and timeframes.

## Business Questions
•	How has sales performed over the past years?

•	Which regions have the highest/lowest sales, and what factors contribute to these trends?

•	Can we predict future sales based on historical trends coupling strategic marketing implementations?
   
## Data Description
This is an e-commerce data  sourced from Kaggle and structured in a .csv file. 

## Plan 
•	Use power query to clean and process data.

•	Use Excel to analyzed and design a compelling dashboard.

## Cleaning Steps 
## In excel;
•	*Rename some columns*

•	*Remove duplicates*

•	*Remove NA values in critical columns*

•	*Drop some columns*

•	*Create new columns (profit, Quarter etc.)*


## Loaded the dataset into power query;

```bigquery claining steps
= Excel.CurrentWorkbook(){[Name="Table1"]}[Content]

# Transformation
= Table.TransformColumnTypes(Source,{{"name", type text}, {"company", type text}, {"city", type text}, {"country", type text}, {"territory", type text}, {"product", type text}, {"deal_size", type text}, {"order_no", Int64.Type}, {"oder_line", Int64.Type}, {"quantity_ordered", Int64.Type}, {"unit_price", Currency.Type}, {"msrp", Currency.Type}, {"sales ", Currency.Type}, {"profit", Currency.Type}, {"margin", Currency.Type}, {"year", Int64.Type}, {"quarter", Int64.Type}, {"month", type text}, {"month_id", Int64.Type}, {"status", type text}})
= Table.TransformColumns(#"Changed Type", {{"quarter", each "Q" & Text.From(_,"en-GH"), type text}})
= Table.TransformColumns(#"Added Prefix",{{"month",each Text.Start(_, 3), type text}})

# Remove columns
= Table.RemoveColumns(#"Extracted First Characters",{"month_id", "order_no", "oder_line"})
= Table.RenameColumns(#"Removed Columns",{{"quantity_ordered", "quantity"}})
= Table.SelectRows(#"Renamed Columns", each ([year] <> null))

#Replace values
=Table.ReplaceValue(#"FilteredRows","VintageCars","Vintage Car",Replacer.ReplaceText,{"product"})
=Table.ReplaceValue(#"Replaced Value","Planes","Plane",Replacer.ReplaceText,{"product"})
=Table.ReplaceValue(#"ReplacedValue1","ClassicCars","ClassicCar”, Replacer.ReplaceText,{"product"})
=Table.ReplaceValue(#"ReplacedValue2","Motorcycles","Motorcycle",Replacer.ReplaceText,{"product"})
= Table.ReplaceValue(#"Replaced Value3","Ships","Ship",Replacer.ReplaceText,{"product"})
= Table.ReplaceValue(#"Replaced Value4","Trains","Train",Replacer.ReplaceText,{"product"})
=Table.ReplaceValue(#"Replaced Value5","Trucks And Buses","Truck & Bus",Replacer.ReplaceText,{"product"})
= Table.SelectRows(#"Replaced Value6", each true)
= Table.ReplaceValue(#"Filtered Rows1","Nyc","New York City",Replacer.ReplaceText,{"city"})
= Table.SelectRows(#"Replaced Value7", each true)

```

<img width="959" height="486" alt="data_inspection" src="https://github.com/user-attachments/assets/83918869-a153-4767-ae6f-c48a9b74882a" />

## Exploratory Data Analysis (EDA)
- *Histogram was employed during the EDA stage to visualize data spread, identify anomalies, and support data cleaning decisions.* 
- Key performance indicators (KPIs) including total sales, total profit, total quantity and year-over-year(YoY%) was established.
  
<img width="958" height="487" alt="eda" src="https://github.com/user-attachments/assets/2339d769-d0fe-485c-8c59-2ed925d08b53" />

## Dashboard  
A dynamic dashboard that lists and describe key plots: line chart,bar chart and histogram.
#### Target Audience
- *Sales Managers: To monitor team and individual performance.*
- *Head of Sales: To assess strategic sales performance and ROI.*
- *Regional Sales Reps: To understand their performance and areas of opportunity.*
- *Marketing Team: To identify campaign impact on sales.*

<img width="796" height="377" alt="Dashboard" src="https://github.com/user-attachments/assets/3975d5b4-9719-4123-b969-b10a6e685441" />

## Key Findings
- The analysis showed a consistent decline in performance throughout the years leaving the YoY% precentage declining significantly.
- The overall performance of sales have been poor even within my metrics.

## Business Impact  
- With this informations, stakeholders can have better insights into the business performance, save time, reduce cost and device trategies to reverse this decline.








