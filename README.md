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
![](./Screenshot/ss_login.png) I created a user named SL1 and password so that I can work using it
![](./Screenshot/user_created.png)This was followed by granting privileges on my database
![](./Screenshot/grant_privile.png)

After this, I proceeded to create the tables, here comes first table called Customers which has different attributes like  customer id, names and contact details like email..
![](./Screenshot/Table_customers.png)![](./Screenshot/ccolumn.png)![](./Screenshot/Customer_insertion.png)![](./Screenshot/Customers_data.png)
Next, I created the bookings table. It includes attributes like booking id, customer id, flight id, booking date, and fare paid to store all booking information.

![](./Screenshot/Table_bookings.png)

![](./Screenshot/bcolumn.png)

![](./Screenshot/Booking_insertion.png)


![](./Screenshot/Booking_data.png)


I created the flights table. It has attributes like flight id, route id, departure time, arrival time, and flight status to keep details about each flight.

![](./Screenshot/Table_flights.png)

![](./Screenshot/fcolumn.png)

![](./Screenshot/Flights_insertion.png)
![](./Screenshot/Flights_data.png)


and then I created the routes table. It includes attributes like route id, origin, destination, and distance to store information about each route.

![](./Screenshot/Table_routes.png)

![](./Screenshot/rcolumn.png)

![](./Screenshot/Routes_insertion.png)

![](./Screenshot/Routes_data.png)


finally I created a sequences in order  to automatically generate unique values for the primary keys, so that each new record gets its own number without mistakes

![](./Screenshot/Sequence_created.png)


we cant forget that I also created an ER diagram to show how the tables are connected.

![](./Screenshot/ER_diag.png)



## Step 4: Window Functions Implementation 

**1. Ranking: Ranking: ROW_NUMBER(), RANK(), DENSE_RANK(), PERCENT_RANK() 

![](./Screenshot/Rank1.png)


![](./Screenshot/Rank2.png)

 I used ROW_NUMBER(), RANK(), DENSE_RANK(), and PERCENT_RANK() to rank customers and easily spot the top ones by revenue. Arina Airlines needed to find its highest-revenue customers for loyalty tiers, premium services, and retention plans.

**2. Aggregate: SUM(), AVG(), MIN(), MAX() with frame comparisons (ROWS vs RANGE) 

![](./Screenshot/aggregate1.png)

![](./Screenshot/aggreate2.png)
![](./Screenshot/aggreate3.png)
In Arina Airline,ROWS helps show transaction patterns, while RANGE reveals trends over time. Using ROWS, we can see customers spending habits, and RANGE highlights seasonal revenue patterns for the airline.

**3. Navigation: LAG(), LEAD(), growth % calculations

![](./Screenshot/lag1.png)

![](./Screenshot/lag2.png)


In Arina Airlines, they used LAG() and LEAD() along with growth percentage calculations to analyze changes over time. This helped compare period-to-period performance and understand trends in bookings and revenue.

**4. Distribution: NTILE(4), CUME_DIST()

![](./Screenshot/distribution1.png)


![](./Screenshot/Distibution2.png)

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
