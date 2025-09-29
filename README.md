# plsql-window-functions-Arinatwe-Hope

# 1. Problem Definition
## Business Context:

**company type**: Arina Airlines is the East African airline that flies from big hubs in Nairobi, Dar es Salaam, and Kigali. They go to 15 different places with 12 planes and run more than 50 flights every day.
**Department:** Revenue Management & Customer Analytics
**Industry:** Air Transportation
## Data Challenge:

Arina airline cannot identify high-value customers for loyalty rewards, optimize pricing on competitive routes, or allocate aircraft efficiently based on seasonal demand patterns. Current data shows bookings but lacks customer segmentation, route profitability analysis, and temporal trend identification.
## Expected Outcome:

 At Arina Airlines, we want to identify the top 10% of customers by how often they fly and how much they spend. We also want to rank routes by how profitable they are and how many passengers use them, track monthly booking trends with growth rates, and put customers into loyalty groups for special promotions.


# 2. Success Criteria
1. **Ranking:** Identify most frequent flyers on Nairobi-Dar es Salaam route for priority boarding  
2. **Aggregate:** Track cumulative revenue per quarter to assess financial performance  
3. **Navigation:** Calculate monthly passenger growth to adjust flight schedules  
4. **Distribution:** Segment customers into Bronze/Silver/Gold/Platinum tiers for targeted marketing  
5. **Moving Average:** Smooth occupancy fluctuations to forecast aircraft allocation needs

# 3. Database Schema
Here I started downloading and creating database which i called SYS and password
<img width="1920" height="811" alt="ss_login" src="https://github.com/user-attachments/assets/a68526d2-ad29-4941-ba9a-9774ac34327b" />
 I created a user named SL1 and password so that I can work using it
<img width="1894" height="862" alt="user_created" src="https://github.com/user-attachments/assets/f9ab6d51-e655-4bd0-a609-5d5a57e420b2" />
This was followed by granting privileges on my database
<img width="1894" height="862" alt="grant_privile" src="https://github.com/user-attachments/assets/3e48fb60-80c4-49e2-b269-3d18f2582203" />


After this, I proceeded to create the tables, here comes first table called Customers which has different attributes like  customer id, names and contact details like email..
<img width="1894" height="862" alt="Table_customers" src="https://github.com/user-attachments/assets/9c57cc9c-00e2-4730-aa2d-d4ec805f225b" />
<img width="1894" height="879" alt="ccolumn" src="https://github.com/user-attachments/assets/9db23598-e21f-4fe0-a43b-dd05bd8d119e" />
<img width="1894" height="862" alt="Customer_insertion" src="https://github.com/user-attachments/assets/72db23f6-588d-46b3-b7dd-2dbe9f8b2727" />
<img width="1894" height="892" alt="Customers_data" src="https://github.com/user-attachments/assets/861dff43-8c63-400d-ba6f-815ddeca8cea" />

Next, I created the bookings table. It includes attributes like booking id, customer id, flight id, booking date, and fare paid to store all booking information.

<img width="1894" height="862" alt="Table_bookings" src="https://github.com/user-attachments/assets/e4892204-4c9a-4351-9da0-b78c21ceaf34" />


<img width="1894" height="879" alt="bcolumn" src="https://github.com/user-attachments/assets/1e557453-0737-40d1-9834-fc877d452b23" />


<img width="1894" height="862" alt="Booking_insertion" src="https://github.com/user-attachments/assets/6f95018b-0bc9-40ee-ae1f-6f66476ab884" />



<img width="1894" height="867" alt="Booking_data" src="https://github.com/user-attachments/assets/38c9a546-818e-472c-8e19-380ddc903918" />



I created the flights table. It has attributes like flight id, route id, departure time, arrival time, and flight status to keep details about each flight.

<img width="1894" height="862" alt="Table_flights" src="https://github.com/user-attachments/assets/6df2aaf0-687f-4023-abfb-2b0f005c1770" />


<img width="1894" height="879" alt="fcolumn" src="https://github.com/user-attachments/assets/265dc363-219b-45a7-a2a1-a24dbcc0c406" />


<img width="1894" height="862" alt="Flights_insertion" src="https://github.com/user-attachments/assets/a08badfe-c481-4984-ae59-d4322828faf7" />

<img width="1894" height="892" alt="Flights_data" src="https://github.com/user-attachments/assets/c2210845-0b52-48f2-8a8b-5ca6e1cf00c4" />



and then I created the routes table. It includes attributes like route id, origin, destination, and distance to store information about each route.

<img width="1894" height="862" alt="Table_routes" src="https://github.com/user-attachments/assets/6976891c-accd-46b9-a2dc-6474c0ade1a1" />


<img width="1894" height="879" alt="rcolumn" src="https://github.com/user-attachments/assets/28274b75-7c27-4032-9143-02dce58939d0" />


<img width="1894" height="862" alt="Routes_insertion" src="https://github.com/user-attachments/assets/e506ad3c-1665-4887-b1e7-3a6995e8f2f7" />


<img width="1894" height="867" alt="Routes_data" src="https://github.com/user-attachments/assets/69af5d8f-3346-4b41-97f0-22e5736e18ea" />



finally I created a sequences in order  to automatically generate unique values for the primary keys, so that each new record gets its own number without mistakes

<img width="1894" height="862" alt="Sequence_created" src="https://github.com/user-attachments/assets/e7eab86b-dd8a-49f6-b545-97b840303fc2" />



we cant forget that I also created an ER diagram to show how the tables are connected.

<img width="627" height="248" alt="ER_diag" src="https://github.com/user-attachments/assets/2c88b038-39b9-4b6f-a286-469d79192183" />




## Step 4: Window Functions Implementation 

**1. Ranking: Ranking: ROW_NUMBER(), RANK(), DENSE_RANK(), PERCENT_RANK() 
```SELECT 
    customer_id,
    customer_name,
    total_revenue,
    ROW_NUMBER() OVER (ORDER BY total_revenue DESC) AS row_number,
    RANK() OVER (ORDER BY total_revenue DESC) AS rank_position,
    DENSE_RANK() OVER (ORDER BY total_revenue DESC) AS dense_rank,
    ROUND(PERCENT_RANK() OVER (ORDER BY total_revenue) * 100, 2) AS percent_rank
FROM (
    SELECT 
        c.customer_id,
        c.first_name || ' ' || c.last_name AS customer_name,
        SUM(b.fare_paid) AS total_revenue
    FROM customers c
    JOIN bookings b ON c.customer_id = b.customer_id
    GROUP BY c.customer_id, c.first_name, c.last_name
)
ORDER BY total_revenue DESC;

```

<img width="1894" height="988" alt="Rank1" src="https://github.com/user-attachments/assets/68cee567-f710-4a16-ae13-10a04f0c2559" />



<img width="1894" height="988" alt="Rank2" src="https://github.com/user-attachments/assets/7a3d27f3-2711-4ad3-a6f5-64d91b3a4c6e" />


 I used ROW_NUMBER(), RANK(), DENSE_RANK(), and PERCENT_RANK() to rank customers and easily spot the top ones by revenue. Arina Airlines needed to find its highest-revenue customers for loyalty tiers, premium services, and retention plans.

**2. Aggregate: SUM(), AVG(), MIN(), MAX() with frame comparisons (ROWS vs RANGE) 


<img width="1894" height="862" alt="aggregate1" src="https://github.com/user-attachments/assets/2157bcf8-94e1-4499-81cb-6d220431e67b" />


<img width="1894" height="862" alt="aggreate2" src="https://github.com/user-attachments/assets/c361f402-85a0-410d-b2cc-8504b7384aad" />

<img width="1894" height="862" alt="aggreate3" src="https://github.com/user-attachments/assets/c5d0ef34-4efc-40dc-8434-26e34a3e41eb" />

In Arina Airline,ROWS helps show transaction patterns, while RANGE reveals trends over time. Using ROWS, we can see customers spending habits, and RANGE highlights seasonal revenue patterns for the airline.

**3. Navigation: LAG(), LEAD(), growth % calculations

```
SELECT 
    TO_CHAR(booking_date, 'YYYY-MM') AS month_year,
    COUNT(booking_id) AS current_month_bookings,
    SUM(fare_paid) AS current_month_revenue,
    LAG(COUNT(booking_id), 1) OVER (ORDER BY TO_CHAR(booking_date, 'YYYY-MM')) AS previous_month_bookings,
    LAG(SUM(fare_paid), 1) OVER (ORDER BY TO_CHAR(booking_date, 'YYYY-MM')) AS previous_month_revenue,
    LEAD(COUNT(booking_id), 1) OVER (ORDER BY TO_CHAR(booking_date, 'YYYY-MM')) AS next_month_bookings,
    LEAD(SUM(fare_paid), 1) OVER (ORDER BY TO_CHAR(booking_date, 'YYYY-MM')) AS next_month_revenue,
    ROUND(
        ((COUNT(booking_id) - LAG(COUNT(booking_id), 1) OVER (ORDER BY TO_CHAR(booking_date, 'YYYY-MM'))) / 
        LAG(COUNT(booking_id), 1) OVER (ORDER BY TO_CHAR(booking_date, 'YYYY-MM'))) * 100, 2
    ) AS booking_growth_pct,
    ROUND(
        ((SUM(fare_paid) - LAG(SUM(fare_paid), 1) OVER (ORDER BY TO_CHAR(booking_date, 'YYYY-MM'))) / 
        LAG(SUM(fare_paid), 1) OVER (ORDER BY TO_CHAR(booking_date, 'YYYY-MM'))) * 100, 2
    ) AS revenue_growth_pct
FROM bookings
GROUP BY TO_CHAR(booking_date, 'YYYY-MM')
ORDER BY TO_CHAR(booking_date, 'YYYY-MM');

```

<img width="1894" height="862" alt="Lag1" src="https://github.com/user-attachments/assets/a6372c43-76e1-4d3d-ace4-51451f53e141" />


<img width="1894" height="862" alt="Lag2" src="https://github.com/user-attachments/assets/e7d055b3-ce47-4291-906f-37f13d9fed69" />



In Arina Airlines, they used LAG() and LEAD() along with growth percentage calculations to analyze changes over time. This helped compare period-to-period performance and understand trends in bookings and revenue.

**4. Distribution: NTILE(4), CUME_DIST()
```

SELECT 
    customer_id,
    customer_name,
    total_revenue,
    total_flights,
    NTILE(4) OVER (ORDER BY total_revenue DESC) AS revenue_quartile,
    ROUND(CUME_DIST() OVER (ORDER BY total_revenue) * 100, 2) AS cumulative_distribution
FROM (
    SELECT 
        c.customer_id,
        c.first_name || ' ' || c.last_name AS customer_name,
        SUM(b.fare_paid) AS total_revenue,
        COUNT(b.booking_id) AS total_flights
    FROM customers c
    JOIN bookings b ON c.customer_id = b.customer_id
    GROUP BY c.customer_id, c.first_name, c.last_name
)
ORDER BY total_revenue DESC;
```

<img width="1894" height="988" alt="Distribution1" src="https://github.com/user-attachments/assets/9c082174-4324-4f9d-9e55-bccf16b6801b" />



<img width="1894" height="988" alt="Distibution2" src="https://github.com/user-attachments/assets/7f16ac6d-9b09-4a78-a928-93610687e99d" />


I used NTILE(4) and CUME_DIST() to divide customers into segments. This helped identify different customer groups and target them with tailored services and offers.


**Step 5: Results Analysis

**1. Descriptive – What happened?
- Our main customers are John Mwangi and Sarah Uwase, who travel the most on different routes.
- Some routes, like Nairobi to Dar es Salaam, make more money each month, while others stop growing after a few months.
- The number of passengers on flights changes with the season; some flights stay the same, others go up and down.
- 25% of our customers give more than half of the total money, so loyal passengers are very important for the airline
**2. Diagnostic – Why?
- The Nairobi–Dar es Salaam route makes a lot of money because it is popular with business travelers and corporate contracts.
- Revenue changes each month due to seasonal factors: January is busy after holidays, while February and March see fewer business travelers.
- The top 25% of customers fly three times more often than the bottom 25%, showing that frequent flyers bring in more revenue.
- In March, revenue dropped by 40% because fewer people booked flights, showing that demand changes, not ticket prices, caused the drop.
**3. Prescriptive – What next?

**Immediate Actions (Next 30 days)**
1. Start a loyalty program for top customers with special benefits.
2. Use dynamic pricing on Nairobi–Dar es Salaam during busy times.
3. Offer personalized upgrades to mid-tier customers to increase spending.
**Strategic Initiatives (Next Quarter)**  
4. Reactivate low-activity customers with special fares.  
5. Adjust aircraft capacity on busy routes during peak periods.  
6. Set up monthly dashboards to monitor performance.
**Long-term Planning (Next 6 months)**  
7. Expand routes to profitable, high-potential destinations.  
8. Implement a CRM system to track customer tiers and personalize communication.  
9. Create seasonal pricing strategies based on growth trends.


**Step 7: References 

1. The Analytics Edge: Unit 6 - Market Segmentation for Airlines from:https://rpubs.com/SulmanKhan/435961?utm
2. Airline Revenue Management: The Shift from Legacy from: https://www.fetcherr.io/blog/airline-revenue-management?
3.  Oracle Base. (2023). _Analytic Functions in Oracle_. Retrieved from [https://oracle-base.com/articles/misc/analytic-functions](https://oracle-base.com/articles/misc/analytic-functions) 
4. W3Schools. (2023). _SQL Window Functions_. Retrieved from [https://www.w3schools.com/sql/sql_window_functions.asp](https://www.w3schools.com/sql/sql_window_functions.asp)
5.  GeeksforGeeks. (2023). _Window Functions in SQL_. Retrieved from [https://www.geeksforgeeks.org/window-functions-in-sql/](https://www.geeksforgeeks.org/window-functions-in-sql/)
6. TutorialsPoint. (2023). _Oracle Analytic Functions_. Retrieved from [https://www.tutorialspoint.com/oracle/oracle_analytic_functions.htm](https://www.tutorialspoint.com/oracle/oracle_analytic_functions.htm)
7. Stack Overflow. (2023). _Oracle Window Functions Discussions_. Retrieved from [https://stackoverflow.com/questions/tagged/oracle+analytic-functions](https://stackoverflow.com/questions/tagged/oracle+analytic-functions)
8. Oracle FAQ. (2023). _Analytic Functions Examples_. Retrieved from [http://www.orafaq.com/wiki/Analytic_functions](http://www.orafaq.com/wiki/Analytic_functions)
9. DeepSeek. (2023). _SQL Window Functions Documentation_. Retrieved from [https://platform.deepseek.com/](https://platform.deepseek.com/)
10. ChatGPT. (2023). _Window Functions Implementation Guidance_. Retrieved from [https://chat.openai.com/](https://chat.openai.com/)
11. SQL Tutorial. (2023). _Oracle Analytic Functions Guide_. Retrieved from [https://www.sqltutorial.org/sql-window-functions/](https://www.sqltutorial.org/sql-window-functions/)
12. Oracle PL/SQL Documentation. (2023). _Database Development Concepts_. Retrieved from [https://docs.oracle.com/en/database/oracle/oracle-database/19/lnpls/](https://docs.oracle.com/en/database/oracle/oracle-database/19/lnpls/)
13. [DBATricks.com](https://DBATricks.com). (2023). _Practical Window Functions Examples_. Retrieved from [https://www.dbatricks.com/](https://www.dbatricks.com/)

All sources were properly cited. Implementations and analysis represent original work. No AI-
generated content was copied without attribution or adaptation.
