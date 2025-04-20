# Beverage Market Dynamics & Forecasting: B2B Focus and Inventory Optimization

## 1) Purpose and Outcome
### **Purposes:**
- Understanding the distinct characteristics and purchasing behaviors of different customer types **B2B and B2C**.
- Evaluating the performance of **individual products and categories** to identify high-potential offerings and areas for improvement.
- **Sales Forecasting and Planning:** Identifying temporal sales patterns and trends to improve inventory management, promotional planning, and overall sales forecasting accuracy.
### **Outcomes:**
✅ Identified the significant revenue contribution of **B2B customers**.<br>
✅ Demonstrated the profitability of **premium alcoholic beverages**, supporting the expansion of this category and targeted marketing to capitalize on high-margin products.<br>
✅ Provide **Sales Forecasting and Seasonal Planning:**
   - Uncovered consistent seasonal dips in sales during **March**, enabling **promotions and coordinated inventory adjustments**.
   - Revealed uniform seasonal patterns across beverage categories, facilitating **demand forecasting**.

## 2) Technologies Used

### **Python:**
Used for data processing, analysis, and modeling:
- **Reading and cleaning** the dataset.
- **Performing exploratory data analysis (EDA).**
- **Building SARIMAX models** to forecast total sales and quantities of each products (monthly).

## 3) Data Sources
The dataset used in this analysis is sourced from the **Kaggle**. This dataset was created to simulate realistic sales patterns in the beverage industry, highlighting important factors like regional preferences, seasonal fluctuations, and customer segmentation. It features both Business-to-Business (B2B) and Business-to-Consumer (B2C) transactions, making it adaptable for a variety of analytical purposes.

**Data sources:** https://www.kaggle.com/datasets/sebastianwillmann/beverage-sales/data

## 4) Data Overview
| Order_ID   | Customer_ID   | Customer_Type   | Product            | Category            |   Unit_Price |   Quantity |   Discount |   Total_Price | Region             | Order_Date   |
|:-----------|:--------------|:----------------|:-------------------|:--------------------|-------------:|-----------:|-----------:|--------------:|:-------------------|:-------------|
| ORD1       | CUS1496       | B2B             | Vio Wasser         | Water               |         1.66 |         53 |       0.1  |         79.18 | Baden-Württemberg  | 2023-08-23   |
| ORD1       | CUS1496       | B2B             | Evian              | Water               |         1.56 |         90 |       0.1  |        126.36 | Baden-Württemberg  | 2023-08-23   |
| ORD1       | CUS1496       | B2B             | Sprite             | Soft Drinks         |         1.17 |         73 |       0.05 |         81.14 | Baden-Württemberg  | 2023-08-23   |
| ORD1       | CUS1496       | B2B             | Rauch Multivitamin | Juices              |         3.22 |         59 |       0.1  |        170.98 | Baden-Württemberg  | 2023-08-23   |
| ORD1       | CUS1496       | B2B             | Gerolsteiner       | Water               |         0.87 |         35 |       0.1  |         27.4  | Baden-Württemberg  | 2023-08-23   |
| ORD2       | CUS2847       | B2C             | Sauvignon Blanc    | Alcoholic Beverages |         9.09 |          2 |       0    |         18.18 | Schleswig-Holstein | 2023-03-16   |
| ORD3       | CUS1806       | B2B             | Tomato Juice       | Juices              |         2.14 |         44 |       0.1  |         84.74 | Hamburg            | 2022-11-20   |
| ORD3       | CUS1806       | B2B             | Vittel             | Water               |         0.43 |         13 |       0.05 |          5.31 | Hamburg            | 2022-11-20   |
| ORD3       | CUS1806       | B2B             | San Pellegrino     | Water               |         1.21 |         92 |       0.1  |        100.19 | Hamburg            | 2022-11-20   |
| ORD3       | CUS1806       | B2B             | Evian              | Water               |         1.38 |          3 |       0.05 |          3.93 | Hamburg            | 2022-11-20   |

- **Order_ID:** Unique identifier for each order. Groups multiple products within the same order.
- **Customer_ID:** Unique identifier for each customer, distinguishing individual buyers.
- **Customer_Type:** Indicates whether the customer is B2B (business-to-business) or B2C (business-to-consumer).
- **Product:** The name of the product purchased, such as "Coca-Cola" or "Erdinger Weißbier".
- **Category:** The product category, such as "Soft Drinks" or "Alcoholic Beverages".
- **Unit_Price:** The price per unit of the product.
- **Quantity:** The number of units purchased for the specified product in the order.
- **Discount:** The discount applied to the product (e.g., 0.1 for 10%). Discounts are only given to B2B customers.
- **Total_Price:** The total price for the product after applying discounts.
- **Region:** The region of the customer, such as "Bayern" or "Berlin".
- **Order_Date:** The date when the order was placed.

## 5) Data Cleaning

```python
df.isnull().sum()
```
|               |   0 |
|:--------------|----:|
| Order_ID      |   0 |
| Customer_ID   |   0 |
| Customer_Type |   0 |
| Product       |   0 |
| Category      |   0 |
| Unit_Price    |   0 |
| Quantity      |   0 |
| Discount      |   0 |
| Total_Price   |   0 |
| Region        |   0 |
| Order_Date    |   0 |
```python
df.duplicated().sum()
```
0
```python
df['Order_ID'] = df['Order_ID'].str[3:].astype('int')
df['Customer_ID'] = df['Customer_ID'].str[3:].astype('int')
df['Customer_Type'] = df['Customer_Type'].astype('category')
df['Category'] = df['Category'].astype('category')
df['Order_Date'] = pd.to_datetime(df['Order_Date'], format='%Y-%m-%d')
df.info()
```

## 6) EDA

### Customer Type Analysis
![Customer_type_analysis](Charts/chart_1.png)
![Customer_type_analysis](Charts/chart_2.png)
![Customer_type_analysis](Charts/chart_15.png)
![Customer_type_analysis](Charts/chart_16.png)

- B2B customers dominate both sales (76.6%) and quantities (77.7%), making them your primary revenue source.
- B2B customers have significantly higher average sales ($280) and quantities (50 units) compared to B2C customers ($45 and 8 units).
- Focus retention efforts on top B2B customers while developing strategies to move mid-tier B2C customers to higher value segments.

### Product Performance Analysis
![Product_performance_analysis](Charts/chart_3.png)
![Product_performance_analysis](Charts/chart_4.png)
![Product_performance_analysis](Charts/chart_5.png)
![Product_performance_analysis](Charts/chart_6.png)

- Premium alcoholic beverages drive revenue: Veuve Clicquot, Moët & Chandon, and Johnnie Walker top sales despite lower quantities.
- Sales-quantity mismatch: While alcoholic beverages dominate sales (~$900M), all categories (juices, water, soft drinks) sell in similar quantities (~50M units each), revealing dramatic price differences.
- Customer preference: Both B2B and B2C customers appear to order equally across categories (25% each)
- Juice opportunity: Hohes C Orange and other juices have the highest quantities sold, but don't appear in top revenue products

### Regional Sales Analysis
![Regional_sales_analysis](Charts/chart_7.png)
**Key Insights:**
- Hamburg leads in sales followed by Hessen and Saarland
- Similar quantity-to-sales ratios across regions indicates consistent pricing strategies

### Time Series Analysis 
![Time_seris_analysis](Charts/chart_8.png)
![Time_seris_analysis](Charts/chart_9.png)
![Time_seris_analysis](Charts/chart_10.png)
![Time_seris_analysis](Charts/chart_11.png)
![Time_seris_analysis](Charts/chart_12.png)
![Time_seris_analysis](Charts/chart_13.png)
![Time_seris_analysis](Charts/chart_14.png)

- Consistent March sales dips across all three years (15-20% decrease)
- All beverage categories show identical seasonal patterns in sales and quantities
- Recovery tends to occur quickly in April-May.
- Launch a promotional campaigns in March to counter seasonal dips.
- Coordinate inventory reduction across all categories during predictable low periods
- Implement forecasting for all beverage types

### Key Findings

1. **Optimize B2B Focus**: Restructure sales organization to capitalize on high-value B2B opportunities
2. **Premium Product Leadership**: Position as premium beverage provider, especially in alcoholic category
3. **Seasonal Planning**: Implement coordinated inventory and marketing strategies for predictable seasonal patterns

## 7) Forecasting

### Total Sales Forecasting

![Total_sales_forecasting](Total_sales_forecast_charts/EDA_chart.png)
![Total_sales_forecasting](Total_sales_forecast_charts/Monthly_sales_before_transf.png)
![Total_sales_forecasting](Total_sales_forecast_charts/Monthly_sales_after_transf.png)
![Total_sales_forecasting](Total_sales_forecast_charts/ACF_and_PACF.png)
![Total_sales_forecasting](Total_sales_forecast_charts/LN_forecast.png)
![Total_sales_forecasting](Total_sales_forecast_charts/XGB_forecast.png)
![Total_sales_forecasting](Total_sales_forecast_charts/SARIMAX_forecast.png)
![Total_sales_forecasting](Total_sales_forecast_charts/Diagnostics_plot.png)
![Total_sales_forecasting](Total_sales_forecast_charts/Model_comparison.png)

### Quantities Of Each Products Forecasting

![Quantities_of_each_products_forecasting](Product_forecast_charts/Product_forecast_charts.png)
![Quantities_of_each_products_forecasting](Product_forecast_charts/Total_quantities_forecsat_chart.png)

## 8) Outcome

1. **B2B Customer Dominance:**
   - B2B customers contribute 76.6% of total sales and 77.7% of quantities
   - They spend significantly more per transaction ($280 average) compared to B2C customers ($45)
   - B2B customers purchase larger quantities (average 50 units vs. 8 units for B2C)
   - The analysis recommends focusing retention efforts on top B2B customers as they represent the most valuable market segment

2. **Premium Alcoholic Beverage Profitability:**
   - Luxury brands (Veuve Clicquot, Moët & Chandon, Johnnie Walker) generate the highest revenue despite lower quantities sold
   - Alcoholic beverages dominate sales (~$900M) while having similar unit sales to other categories (~50M units each) 
   - The analysis supports expanding this product category and implementing targeted marketing strategies to capitalize on these high-margin products

3. **Seasonal Sales Patterns and Forecasting:**
   - Consistent March sales dips of 15-20% were identified across all three years analyzed
   - All beverage categories display identical seasonal patterns in both sales and quantities
   - Sales typically recover quickly in April-May following the March dip
   - The analysis recommends launching promotional campaigns in March to counter seasonal declines
   - Coordinated inventory reduction strategies should be implemented across all categories during predictable low periods
   - SARIMAX models were built to forecast total sales and quantities of each product monthly
