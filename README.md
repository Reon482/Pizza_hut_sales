# Pizza Hut Sales Analysis

## Overview:
The project involves analyzing Pizza Hut sales data using SQL to identify key trends and optimize sales strategies.
Focused on querying, aggregating, and interpreting data to support decision-making.

## Objective:
Drive sales performance and improve business strategies by uncovering insights from historical sales data.
Enhance understanding of customer preferences and optimize menu offerings.

## Key Insights:
Analyzed over 10 complex SQL queries to uncover:
Popular menu items (e.g., Margherita, Veggie Delight, Pepperoni Pizza).
Sales trends across different timeframes or locations.
Potential opportunities for upselling and menu optimization.
Contributed to strategies that enhance operational and marketing efficiency.

- Top-Selling Items: Identified signature pizzas (e.g., Margherita, Veggie Delight, and Pepperoni Pizza) as major revenue drivers.
- Sales Trends: Uncovered peak sales periods, helping to optimize staffing and inventory during busy times.
- Customer Preferences: Analyzed customer preferences, informing menu adjustments and promotional offers.
- Data-Driven Strategy: Provided a foundation for creating more targeted marketing campaigns and improving delivery or dine-in services.

The analysis supported effective business strategy development, resulting in improved decision-making and potentially increased sales performance.

## Project Structure:

1. **Retrieve the total number of orders place**:
   
       SELECT
           COUNT(order_id) AS Total_orders
       FROM
           orders;
   
2. **Calculate the total revenue generated from pizza sales**:
   
       SELECT 
           ROUND(SUM(order_details.quantity * pizzas.price),
               2) AS total_sales
       FROM
           order_details
              JOIN
           pizzas ON pizzas.pizza_id = order_details.pizza_id;
   
3. **Identify the highest-priced pizza**:

       SELECT 
          pizza_types.name, pizzas.price
       FROM
          pizza_types
             JOIN
          pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
       ORDER BY pizzas.price DESC
       LIMIT 1;
   
4. **Identify the most common pizza size ordered**:

       SELECT 
           pizzas.size,
           COUNT(order_details.order_details_id) AS order_count
       FROM
           pizzas
              JOIN
           order_details ON pizzas.pizza_id = order_details.pizza_id
       GROUP BY pizzas.size
       ORDER BY order_count DESC;
   
5. **List the top 5 most ordered pizza types along with their quantities**:

       SELECT 
           pizza_types.name, SUM(order_details.quantity) AS quantity
       FROM
           pizza_types
              JOIN
           pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
              JOIN
           order_details ON order_details.pizza_id = pizzas.pizza_id
       GROUP BY pizza_types.name
       ORDER BY quantity DESC
       LIMIT 5;

6. **Join the necessary tables to find the total quantity of each pizza category ordered**:

       SELECT 
           pizza_types.category,
           SUM(order_details.quantity) AS quantity
       FROM
           pizza_types
              JOIN
           pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
              JOIN
           order_details ON order_details.pizza_id = pizzas.pizza_id
       GROUP BY pizza_types.category
       ORDER BY quantity DESC;

7. **Determine the distribution of orders by hour of the day**:

       SELECT 
           HOUR(order_time) AS hour, COUNT(order_id) AS order_count
       FROM
           orders
       GROUP BY HOUR(order_time)
       ORDER BY order_count desc;

8. **Join relevent tables to find the category-wise distribution of pizzas**:

       SELECT 
           category, COUNT(name)
       FROM
           pizza_types
       GROUP BY category
       ORDER BY COUNT(name) DESC;

9. **Group the orders by date and calculate the averagenumber of pizzas ordered per day**:

       SELECT 
            ROUND(AVG(quantity), 0) AS avg_pizza_ordered_per_day
       FROM
           (SELECT 
                orders.order_date, SUM(order_details.quantity) AS quantity
           FROM
                orders
                   JOIN
                order_details ON orders.order_id = order_details.order_id
            GROUP BY orders.order_date) AS order_quantity;

11. **Determine the top 3 most ordered pizza types based on revenue**:

        SELECT 
            pizza_types.name,
            SUM(order_details.quantity * pizzas.price) AS revenue
        FROM
            pizza_types
                JOIN
            pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
                JOIN
            order_details ON order_details.pizza_id = pizzas.pizza_id
        GROUP BY pizza_types.name
        ORDER BY revenue DESC
        LIMIT 3;

12. **Calculate the percentage contribution of each pizza type to total revenue**:    

        SELECT 
            pizza_types.category,
            ROUND(SUM(order_details.quantity * pizzas.price) / (SELECT 
                    ROUND(SUM(order_details.quantity * pizzas.price),
                                2) AS total_sales
        FROM
            order_details
                JOIN
            pizzas ON pizzas.pizza_id = order_details.pizza_id) * 100,
            2) AS revenue_percent
        FROM
            pizza_types
                JOIN
            pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
                JOIN
            order_details ON order_details.pizza_id = pizzas.pizza_id
        GROUP BY pizza_types.category
        ORDER BY revenue_percent DESC;

13. **Analyze the cumulative revenue generated over time**:

        SELECT 
            order_date,
            SUM(revenue) over(order by order_date) as cum_revenue 
        from
            (select orders.order_date,
	          SUM(order_details.quantity * pizzas.price) AS revenue
	          FROM order_details
        JOIN pizzas 
	          ON pizzas.pizza_id = order_details.pizza_id
        JOIN orders
            ON orders.order_id = order_details.order_id
        GROUP BY orders.order_date) as sales;    

14. **Determine the top 3 most ordered pizza types based on revenue for each pizza category**:

        SELECT name, revenue from
             (SELECT category, name, revenue,
             rank() over (partition by category order by revenue desc) as rn
	           FROM
		            (select pizza_types.category,pizza_types.name,
                sum((order_details.quantity)*pizzas.price) as revenue
        FROM pizza_types JOIN pizzas 
            ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN order_details 
            ON order_details.pizza_id = pizzas.pizza_id
        GROUP BY pizza_types.category,pizza_types.name) as a) as b
        where rn <= 3;   











   
       
   














