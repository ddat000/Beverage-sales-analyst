# Beverage Market Dynamics & Forecasting: B2B Channel Optimization

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

## 6) EDA Process

### Customer Type Analysis (Images 1-2 from original set)
![Customer_type_analysis](Charts/chart_1.png)
![Customer_type_analysis](Charts/chart_2.png)
- B2B customers generate 76.6% of total sales while B2C accounts for only 23.4%
- B2B average purchase value is 5-6x higher than B2C customers

**Recommendations:**
- Develop tiered B2B loyalty program with volume-based incentives
- Implement dedicated account management for high-value B2B customers
- Create specialized bulk pricing structures for different B2B segments
- Design separate marketing strategies targeting the unique needs of B2B vs B2C customers

### Product Performance Analysis (Images 3-5 from original set)
![Product_performance_analysis](Charts/chart_3.png)
![Product_performance_analysis](Charts/chart_4.png)
![Product_performance_analysis](Charts/chart_5.png)
**Key Insights:**
- Premium alcoholic beverages drive revenue despite lower quantities sold
- Veuve Clicquot and Moet & Chandon are top revenue generators
- Alcoholic beverages have the most product variety (19 unique products)

**Recommendations:**
- Expand premium alcohol selection, particularly champagnes and high-end spirits
- Create bundling opportunities between premium alcoholic products and complementary items
- Implement strategic pricing on highest-margin products
- Develop marketing campaigns highlighting quality and exclusivity of premium offerings

### Regional Sales Analysis (Image 6 from original set)
**Key Insights:**
- Hamburg leads in sales followed by Hessen and Saarland
- Similar quantity-to-sales ratios across regions indicates consistent pricing strategies

**Recommendations:**
- Leverage success factors from top-performing regions
- Implement targeted growth strategies for underperforming regions like Bremen
- Maintain consistent pricing across regions while optimizing distribution networks

### Time Series Analysis (Images 7-8 from original set, Image 1 from new set)
**Key Insights:**
- Consistent March sales dips across all three years (15-20% decrease)
- All beverage categories show identical seasonal patterns
- Recovery tends to occur quickly in April-May

**Recommendations:**
- Launch "March Madness" promotional campaigns to counter seasonal dips
- Coordinate inventory reduction across all categories during predictable low periods
- Implement integrated forecasting for all beverage types
- Consider company-wide rather than category-specific promotions during low periods

### Customer Concentration Analysis (Images 2-3 from new set)
**Key Insights:**
- Approximately 40-50 customers generate disproportionately high sales (200K-300K+ each)
- Sharp drop-off to long tail of smaller customers (50K or less)
- Similar Pareto distribution in both sales value and quantities

**Recommendations:**
- Develop comprehensive key account management for top 50 customers
- Create risk mitigation strategies for potential loss of high-value accounts
- Implement special retention programs for highest-value customers
- Design targeted growth strategies for mid-tier customers
- Analyze characteristics of top customers to identify potential look-alikes in smaller customer base

## Cross-Cutting Strategic Priorities

1. **Optimize B2B Focus**: Restructure sales organization to capitalize on high-value B2B opportunities
2. **Premium Product Leadership**: Position as premium beverage provider, especially in alcoholic category
3. **Seasonal Planning**: Implement coordinated inventory and marketing strategies for predictable seasonal patterns
4. **Customer Relationship Management**: Develop tiered approach based on clearly defined customer value
5. **Category-Specific Strategies**: Leverage unique patterns in each beverage category while maintaining coordinated overall approach
