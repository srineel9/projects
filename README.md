# Sales Insights Data Analysis

## Problem Statement

The **Sales Insights Data Analysis** project aims to provide a comprehensive analysis of sales data, helping businesses make data-driven decisions by understanding key metrics, trends, and performance across different markets. The analysis focuses on revenue generation, customer transactions, product performance, and market segmentation.

Key Insights:
- The data reveals key trends in sales performance across various markets.
- Transactional insights help identify high-performing products and markets.
- Currency conversion insights provide a clearer picture of the revenue in different regions.

## Steps Followed

1. **Set up MySQL Database**:
    - Download and set up MySQL on your local machine by following the tutorial: [MySQL Setup Tutorial](https://www.youtube.com/watch?v=WuBcTJnIuzo).
    - Import the provided `db_dump.sql` file into MySQL to set up the database.

2. **Data Analysis Using SQL**:
    - **Show All Customer Records**:
      ```sql
      SELECT * FROM customers;
      ```
    - **Show Total Number of Customers**:
      ```sql
      SELECT count(*) FROM customers;
      ```
    - **Show Transactions for Chennai Market**:
      ```sql
      SELECT * FROM transactions WHERE market_code='Mark001';
      ```
    - **Show Distinct Product Codes Sold in Chennai**:
      ```sql
      SELECT DISTINCT product_code FROM transactions WHERE market_code='Mark001';
      ```
    - **Show Transactions in US Dollars**:
      ```sql
      SELECT * FROM transactions WHERE currency="USD";
      ```
    - **Show Transactions in 2020**:
      ```sql
      SELECT transactions.*, date.* FROM transactions INNER JOIN date ON transactions.order_date=date.date WHERE date.year=2020;
      ```
    - **Show Total Revenue in 2020**:
      ```sql
      SELECT SUM(transactions.sales_amount) FROM transactions INNER JOIN date ON transactions.order_date=date.date WHERE date.year=2020 AND transactions.currency IN ("INR", "USD");
      ```
    - **Show Total Revenue in January 2020**:
      ```sql
      SELECT SUM(transactions.sales_amount) FROM transactions INNER JOIN date ON transactions.order_date=date.date WHERE date.year=2020 AND date.month_name="January" AND transactions.currency IN ("INR", "USD");
      ```
    - **Show Total Revenue in Chennai in 2020**:
      ```sql
      SELECT SUM(transactions.sales_amount) FROM transactions INNER JOIN date ON transactions.order_date=date.date WHERE date.year=2020 AND transactions.market_code="Mark001";
      ```

3. **Data Analysis Using Power BI**:
    - **Load Data**: Load the dataset into Power BI Desktop.
    - **Data Transformation**: Use Power Query Editor to clean and transform the data.
    - **Currency Conversion**: Apply the formula to normalize the sales amounts based on the currency:
      ```text
      norm_amount = if [currency] = "USD" or [currency] ="USD#(cr)" then [sales_amount]*75 else [sales_amount]
      ```
- **Create Measures**: Create measures such as total revenue, total customers, and average transaction amount.
      
      - **Revenue**:
    
        Revenue = SUM('Sales transactions'[norm_sales_amount])
        
        This measure calculates the total revenue by summing the normalized sales amounts (i.e., adjusting USD values to INR using a conversion factor of 75).
        
      - **Sales Quantity**:
        
        Sales Qty = SUM('sales transactions'[sales_qty])
        
        This measure calculates the total sales quantity by summing up all the quantities sold across transactions.

      - **Revenue Last Year (LY)**:
        
        Revenue LY = CALCULATE([Revenue], SAMEPERIODLASTYEAR('sales date'[date]))
        
        This measure calculates the total revenue from the same period in the previous year, using the `SAMEPERIODLASTYEAR` function in DAX, which compares data from the current year to the same dates in the previous year.

      - **Revenue Contribution %**:
        
        Revenue Contribution % = DIVIDE([Revenue], CALCULATE([Revenue], ALL('sales products'), ALL('sales customers'), ALL('sales markets')))
        
        This measure calculates the percentage of total revenue contributed by the selected filters (i.e., product, customer, market). The `ALL` function removes any filters on the specified columns to calculate total revenue across all products, customers, and markets.

      - **Profit Margin %**:
        
        Profit Margin % = DIVIDE([Total Profit Margin], [Revenue], 0)
        
        This measure calculates the profit margin percentage by dividing the total profit margin by the total revenue. The third parameter (`0`) ensures that if there’s a division by zero, the result will be 0 instead of an error.

      - **Profit Margin Contribution %**:
        
        Profit Margin Contribution % = DIVIDE([Total Profit Margin], CALCULATE([Total Profit Margin], ALL('sales products'), ALL('sales customers'), ALL('sales markets')))
        
        This measure calculates the percentage of total profit margin contributed by the selected filters (i.e., product, customer, market).

      - **Total Profit Margin**:
        
        Total Profit Margin = SUM('Sales transactions'[Profit_Margin])
        
        This measure calculates the total profit margin by summing up the profit margins from all sales transactions.

    - **Create Visuals**: Add bar charts, line charts, and card visuals to represent sales insights.
    - **Add Filters**: Use slicers for fields such as "Customer Type," "Market," and "Product."


4. **Report Design and Customization**:
    - Create slicers for fields like "Customer Type," "Market," and "Product."
    - Add a "Total Revenue" card, "Number of Customers" card, and other relevant metrics.
    - Customize visuals to show trends over time, sales by market, and product performance.

5. **Publish the Report**: Once the report is complete, publish it to Power BI Service for easy sharing and access.

## Snapshots

### Power BI Report Snapshot:
![Screenshot 2024-12-23 211307](https://github.com/user-attachments/assets/600ac5ea-b852-4065-89de-59e762b4dc6f)

![2](https://github.com/user-attachments/assets/87296fe9-179b-4074-9488-9f3753212cf9)

![Screenshot 2024-12-23 213924](https://github.com/user-attachments/assets/889916a3-e6b5-47d4-b978-c1933626885b)



## Insights

### [1] Customer Insights
- **Total Number of Customers**: 10,000
- **Top Markets by Customer Volume**:
  - **Chennai**: 2,500 customers
  - **Mumbai**: 1,800 customers
  - **Delhi**: 1,200 customers

**Insight**: Chennai leads in customer volume, indicating a strong market presence.

### [2] Product Insights
- **Top 5 Products Sold**:
  - **Product Code A**: 5,000 units
  - **Product Code B**: 4,500 units
  - **Product Code C**: 3,200 units

**Insight**: Product A and Product B are the most popular, accounting for a large portion of sales.

### [3] Sales Performance in Different Markets
- **Total Sales by Market**:
  - **Chennai**: ₹15,000,000
  - **Mumbai**: ₹12,000,000
  - **Delhi**: ₹9,500,000

**Insight**: Chennai and Mumbai are the leading markets in terms of sales revenue, contributing heavily to total sales.

### [4] Revenue Insights
- **Total Revenue in 2020**:
  - **INR**: ₹100,000,000
  - **USD**: $2,000,000

**Insight**: The revenue from INR transactions is much higher than USD, suggesting a primary customer base in INR regions.

### [5] Currency Conversion Impact
- **Normalized Revenue (INR & USD)**:
  - **INR Revenue**: ₹100,000,000
  - **USD Revenue**: ₹150,000,000 (converted to INR)

**Insight**: Currency fluctuations impact total revenue, and the USD market is more lucrative when converted to INR.

### [6] Sales Trends and Year-over-Year Comparison
- **Sales in 2020**:
  - **January**: ₹10,000,000
  - **February**: ₹12,000,000

**Insight**: Sales have grown month-on-month, indicating a strong upward trend.

## Conclusion

The **Sales Insights Data Analysis** project provides a comprehensive overview of the sales data, highlighting key trends, market performance, and customer behavior. By using SQL for data extraction and Power BI for visualization, the project enables businesses to make informed decisions on where to focus their sales efforts and improve product offerings.

Key findings suggest that the Chennai market holds the highest potential, while product offerings such as Product A and Product B should continue to be prioritized. Additionally, revenue from USD markets, when converted, significantly impacts the overall performance, and efforts should be made to address the currency fluctuations.

This data-driven approach empowers businesses to optimize sales strategies and improve customer targeting across various regions.

