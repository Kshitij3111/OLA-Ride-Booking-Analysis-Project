# 🛺 OLA Data Analyst 

## 📌 Project Objective

To monitor and evaluate the performance of **OLA ride bookings** in **Bengaluru**, this project aims to create a comprehensive **Ride Booking Analysis Report** based on one month of simulated ride data. The report will provide stakeholders with insights into:

- Customer behavior
- Vehicle performance
- Ride cancellations
- Payment preferences
- Satisfaction ratings

This project is designed to assist decision-makers in identifying patterns, optimizing fleet operations, improving customer satisfaction, and minimizing ride cancellations through data-driven insights.

---

## 📅 Project Scope

- **City**: Bengaluru
- **Duration**: 1 Month
- **Data Volume**: 100,000 rows of ride booking data
- **Special Focus**: Weekends & Match Days (increased ride volume on these days)

---

## 📊 Dashboards Overview

### 1. **📊 Overall Dashboard**

![Screenshot (22)](https://github.com/user-attachments/assets/483bb8e8-678d-4c1a-aa69-af34d20096d7)

**🎯 Purpose**: The Overall Dashboard offers a comprehensive view of the entire ride booking activity, providing insights into total bookings, booking value, and booking status breakdowns. It enables stakeholders to monitor trends and performance at a high level.

**🧾 Key Metrics & KPIs**:
- **Total Bookings**: Total number of ride bookings placed during the month.
- **Total Booking Value**: Total revenue generated from all ride bookings.
- **Booking Status Breakdown**:
  - Success: Percentage of completed rides.
  - Cancelled by Customers: Percentage of cancellations initiated by customers (must not exceed 7%).
  - Cancelled by Drivers: Percentage of cancellations initiated by drivers (must not exceed 18%).
  - Incomplete Rides: Percentage of rides that started but were not completed (must be less than 6%).
- **Booking Value Segmentation**:
  - Percentage of rides under ₹500.
  - Percentage of rides between ₹500 and ₹1000.
  - Percentage of rides over ₹1000.
- **Payment Method Distribution**: Breakdown of bookings by payment method (UPI, Cash, Credit Card, Wallet).
- **Ride Volume Over Time**: A time-series chart showing the daily or weekly booking volume.
- **Customer and Driver Ratings**: Average customer and driver ratings for all rides.

---

### 2. **📊 Vehicle Type Dashboard**

![Screenshot (23)](https://github.com/user-attachments/assets/46e8209b-02ec-4647-b9a3-cdbcb71af8a0)

**🎯 Purpose**: The Vehicle Type Dashboard provides insights into the performance of various vehicle types. It helps understand the contribution of each vehicle category to the overall ride booking activity, focusing on total booking value, successful bookings, and travel metrics.

**🧾 Key Metrics & KPIs**:
- **Vehicle Type**: Categorizes the data based on vehicle types (Auto, Prime Plus, Prime Sedan, Mini, Bike, eBike, Prime SUV).
- **Total Booking Value**: Total booking value for each vehicle type.
- **Successful Booking Value**: Total value of completed rides for each vehicle type.
- **Average Distance Travelled**: Average distance covered for each vehicle type.
- **Total Distance Travelled**: Overall distance covered by each vehicle type.

---

### 3. **📊 Revenue Dashboard**

![Screenshot (24)](https://github.com/user-attachments/assets/9d77b90a-9480-4af5-8687-da84e3430190)

**🎯 Purpose**: The Revenue Dashboard focuses on the financial aspect of ride bookings by analyzing revenue distribution across payment methods and highlighting the highest-value customers.

**🧾 Key Metrics & KPIs**:
- **Revenue by Payment Method (Bar Chart)**: Visualizes the total revenue broken down by payment methods (UPI, Cash, Credit Card, Wallet).
- **Top 5 Customers by Total Booking Value (Table)**: A table listing the top 5 customers based on total booking value, highlighting high-value customers.

---

### 4. **📊 Cancellation Dashboard**

![Screenshot (25)](https://github.com/user-attachments/assets/bde53e0a-ab82-4608-9a03-5d9b28f2807a)

**🎯 Purpose**: The Cancellation Dashboard provides insights into ride cancellations, focusing on customer-initiated and driver-initiated cancellations. It helps understand cancellation patterns and areas for improvement.

**🧾 Key Metrics & KPIs**:
- **Total Bookings**: Total number of rides booked in the dataset.
- **Successful Bookings**: Total number of completed rides.
- **Cancelled Bookings**: Total number of cancellations (customer or driver).
- **Cancellation Rate**: Percentage of cancellations (ratio of canceled bookings to total bookings), helping assess cancellation frequency.

---

### 5. **📊 Ratings Dashboard**

![Screenshot (26)](https://github.com/user-attachments/assets/5b114ff1-76ba-4cb2-a68e-db2cdf367986)

**🎯 Purpose**: The Ratings Dashboard evaluates driver and customer performance based on ratings. It offers insights into overall satisfaction levels for both parties.

**🧾 Key Metrics & KPIs**:
- **Average Driver Ratings**: The average rating given to drivers across all completed rides.
- **Average Customer Ratings**: The average rating given by customers across all completed rides.

---

## 🧮 SQL Analysis Questions

To complement the Power BI dashboards, the following **SQL queries** will be executed for deeper insights:

- **Retrieve all successful bookings**: Fetch all successfully completed bookings to analyze ride performance.

```sql
SELECT * FROM bookings 
WHERE Booking_Status = 'Success';
```
  
- **Find average ride distance per vehicle type**: Calculate the average distance covered by each vehicle type to analyze travel patterns.

```sql
SELECT 
    Vehicle_Type, AVG(Ride_Distance) AS avg_distance
FROM
    bookings
GROUP BY Vehicle_type;
```

- **Count total rides canceled by customers**: Count cancellations made by customers to understand cancellation trends.

```sql
SELECT 
        COUNT(*)
    FROM
        bookings
    WHERE
        Booking_Status = 'Canceled by Customer';
```

- **Identify top 5 customers by number of rides**: Identify top customers by the number of rides booked.

```sql
SELECT 
        Customer_ID, COUNT(Booking_Status) AS total_rides
    FROM
        bookings
    GROUP BY Customer_ID
    ORDER BY total_rides DESC
    LIMIT 5;
```

- **Rides canceled by drivers for personal reasons**: Count cancellations made by drivers due to personal reasons, aiding fleet management.

```sql
SELECT 
        COUNT(*) AS total_rides_cancelled
    FROM
        bookings
    WHERE
        Canceled_Rides_by_Driver = 'Personal & Car related issue';
```

- **Max/Min driver ratings for Prime Sedan**: Find the highest and lowest driver ratings for "Prime Sedan" to assess service consistency.

```sql
SELECT 
        MAX(Driver_Ratings) AS max_rating,
        MIN(Driver_Ratings) AS min_rating
    FROM
        bookings
    WHERE
        Vehicle_Type = 'Prime Sedan' and
        Driver_Ratings <> 'NA';
```

- **Rides where payment was made using UPI**: Identify rides paid via UPI, showing payment preferences.

```sql
SELECT 
        *
    FROM
        bookings
    WHERE
        Payment_Method = 'UPI';
```

- **Average customer rating per vehicle type**: Calculate the average customer rating for each vehicle type to gauge customer satisfaction.

```sql
SELECT 
        Vehicle_Type, AVG(Customer_Rating) AS avg_customer_rating
    FROM
        bookings
    GROUP BY Vehicle_Type;
```

- **Total booking value for successful rides**: Sum up the total fare from successful bookings to calculate overall revenue.

```sql
SELECT 
        SUM(Booking_Value) AS total_successful_ride_value
    FROM
        bookings
    WHERE
        Booking_Status = 'Success';
```

- **Incomplete rides with reason**: Identify incomplete rides and their reasons to understand issues causing ride failures.

```sql
SELECT 
        Booking_ID, Incomplete_Rides_Reason
    FROM
        bookings
    WHERE
        Incomplete_Rides = 'Yes';
```

---

## ⚒️ Data Generation Constraints

- **Booking ID**: Must start with "CNR" and be 10 digits long.
- **Locations**: Pickup and drop locations are based on 50 dummy areas in Bengaluru.
- **Match Days**: Mar 9, 16, 23, and 30 (higher volume expected).
- **Weekends**: Higher revenue and longer rides expected on weekends.
- **VTAT & CTAT**: Recorded only for successful bookings.
- **AC Complaints**: Valid only for 4-wheelers.
- **Vehicle Types**: Auto, Prime Plus, Prime Sedan, Mini, Bike, eBike, Prime SUV.
- **Payment Methods**: UPI, Cash, Credit Card, Wallet.

---

## 📁 Data Columns Summary

| **Column Name**               | **Description**                                      |
|-------------------------------|------------------------------------------------------|
| **Date, Time**                 | When the ride was booked                            |
| **Booking_ID**                 | Unique ride ID                                       |
| **Booking_Status**             | Success, Cancelled, Incomplete                      |
| **Customer_ID**                | Unique rider ID                                      |
| **Vehicle_Type**               | Auto, Bike, Prime Sedan, etc.                       |
| **Pickup_Location, Drop_Location** | Locations in Bengaluru                          |
| **V_TAT, C_TAT**               | Vehicle Time to Arrival, Customer Time to Arrival   |
| **Cancelled_Rides_by_Customer**| Reason for customer cancellations                    |
| **Cancelled_Rides_by_Driver** | Reason for driver cancellations                    |
| **Incomplete_Rides**           | Boolean (indicates if the ride was incomplete)      |
| **Incomplete_Rides_Reason**    | Reason for incomplete rides                         |
| **Booking_Value**              | Fare charged                                         |
| **Payment_Method**             | UPI, Cash, Credit Card, etc.                        |
| **Ride_Distance**              | Distance in km                                      |
| **Driver_Ratings**             | Rating out of 5                                     |
| **Customer_Rating**            | Rating out of 5                                     |

---

## 🎯 Outcomes

The following outcomes are to emerge from this project:

- **Increased Insight into Booking Patterns**: We expect to uncover key patterns in booking volumes, particularly during weekends and match days, which can help in optimizing operations during peak times.
  
- **Understanding Revenue Drivers**: The analysis will highlight which vehicle types and payment methods contribute most to revenue, allowing for targeted strategies to boost profitability.
  
- **Optimization of Fleet Performance**: The performance of different vehicle types (e.g., Auto, Prime Plus, Bike) will be assessed, leading to potential fleet reallocation and better resource utilization for high-demand areas.

- **Identification of Cancellation Trends**: We will gain a deeper understanding of both customer- and driver-initiated cancellations, potentially informing new policies or initiatives to reduce cancellations and improve service consistency.

- **Improved Customer and Driver Satisfaction**: Analyzing the average customer and driver ratings will help identify areas of improvement, such as service quality or vehicle conditions, ultimately enhancing satisfaction for both parties.

- **Informed Decision-Making**: The insights gathered from the dashboards and SQL analysis will provide data-driven recommendations for operational improvements, pricing strategies, and marketing tactics to foster business growth and enhance customer loyalty.

---
