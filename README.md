# 🍔 Online_Food_Ordering_System 

A MySQL database project for an Online Food Ordering System. It lets customers browse restaurant menus, place orders and make payments, and it generates sales and order reports.

## 📌 Problem Statement

Develop a database that manages restaurant information, menu items, customer orders and payment details, while generating sales and order reports.

## ✅ Expected Outcomes

- Maintain restaurant information
- Store menu details
- Manage customer orders
- Track payment information
- Generate restaurant sales reports
- Display customer order history
- Identify popular food items

## 🗂️ Database Design

**Database name:** `RESTAURANT_ONLINE_FOOD`

| Table | Columns |
|---|---|
| **Restaurants** | `restaurant_id` (PK), `restaurant_name`, `location`, `cuisine` |
| **Menu** | `menu_id` (PK), `restaurant_id` (FK), `item_name`, `price`, `availability` |
| **Customers** | `customer_id` (PK), `customer_name`, `phone_no`, `city` |
| **Orders** | `order_id` (PK), `customer_id` (FK), `menu_id` (FK), `quantity`, `order_date`, `order_status` |
| **Payments** | `payment_id` (PK), `order_id` (FK), `payment_method`, `amount`, `payment_status`, `payment_date` |

### Relationships

```
Restaurants (1) ──< Menu (1) ──< Orders >── (1) Customers
                                   │
                                   └──< Payments
```
🧩 ER Diagram

ER Diagram

📽️ Presentation & Demo Video
-📊 Presentation (PPT): SQL_PROJECT.pptx
-🎥 Demo video: Watch the project walkthrough

## 🛠️ Tech Stack

- MySQL Workbench

## 📁 Repository Structure

```
├── PROJECT.sql                                  # Tables, sample data, all 37 queries
├── Online_Food_Order_System_MySQL-Project.pdf   # Problem statement
├── SQL_PROJECT.pptx                             # Project presentation (PPT)
├── ER_Diagram.png                               # Entity-Relationship diagram
├── Project_Demo.mp4                             # Project walkthrough video
└── README.md
```

## 🚀 How to Run

1. Open **MySQL Workbench** and connect to your local server.
2. Open `PROJECT.sql`.
3. Run the script from top to bottom. It creates the database, tables and sample data first, then the queries.
4. If you hit an error on an `UPDATE` or `DELETE`, see [Troubleshooting](#-errors-faced--fixes).

## 📊 Sample Data

| Table | Records |
|---|---|
| Restaurants | 5 |
| Menu | 10 |
| Customers | 10 |
| Orders | 12 |
| Payments | 12 |

## 🧠 SQL Concepts Covered (37 Questions)

| Topic | Questions |
|---|---|
| Basic SELECT, UPDATE, DELETE | 1 – 4 |
| String functions (`UPPER`, `LOWER`, `LEFT`, `CONCAT_WS`, `REPLACE`) | 5 – 9 |
| Date functions (`DAYNAME`, `DATEDIFF`, `DATE_FORMAT`, `INTERVAL`) | 10 – 14 |
| Aggregate functions & `GROUP BY` | 15 – 19 |
| Joins (`INNER`, `LEFT`, multi-table) | 20 – 25 |
| Subqueries | 26 – 29 |
| Views | 30 – 32 |
| Transactions & `SAVEPOINT` | 33 |
| Stored procedures | 34 – 35 |
| Triggers | 36 – 37 |



## 🐞 Errors Faced & Fixes

### 1. Q2 – `Error Code: 1175` (Safe Update Mode)

**Query:**
```sql
UPDATE ORDERS SET ORDER_STATUS = 'Delivered' WHERE ORDER_STATUS = 'Preparing';
```

**Error:**
```
Error Code: 1175. You are using safe update mode and you tried to update a table
without a WHERE that uses a KEY column.
```

**Cause:** MySQL Workbench runs in *safe update mode* by default. It blocks `UPDATE`/`DELETE` statements whose `WHERE` clause doesn't use a primary key column. Here the filter is on `ORDER_STATUS`, which is not a key.

**Fix:** turn safe update mode off **before** running the update.
```sql
SET SQL_SAFE_UPDATES = 0;

UPDATE ORDERS SET ORDER_STATUS = 'Delivered' WHERE ORDER_STATUS = 'Preparing';

SET SQL_SAFE_UPDATES = 1;   -- optional: turn it back on
```

> ⚠️ The `SET` statement must come **before** the `UPDATE`. If it is placed after, the update has already failed.

---

### 2. Q4 / Q36 – Foreign key error when deleting orders

**Error:** `Cannot delete or update a parent row: a foreign key constraint fails`

**Cause:** `PAYMENTS.ORDER_ID` references `ORDERS.ORDER_ID`, so an order cannot be deleted while a payment row still points to it.

**Fix:** delete the child row (payment) first, then the order.
```sql
DELETE FROM PAYMENTS WHERE ORDER_ID = 405;
DELETE FROM ORDERS   WHERE ORDER_ID = 405;
```

---

### 3. Q37 – Trigger not logging a change

**Cause:** the trigger only logs when the status actually changes. Updating an order that is already `Delivered` to `'DELIVERED'` is not a change (MySQL string comparison is case-insensitive by default), so nothing is inserted.

**Fix:** test with a real change.
```sql
UPDATE ORDERS SET ORDER_STATUS = 'Cancelled' WHERE ORDER_ID = 402;
SELECT * FROM ORDER_STATUS_HISTORY;
```



## 💡 Key Learnings

- Safe update mode and how to handle it
- Joining 3–4 tables to build reports
- Foreign key constraints and the correct order for deleting data
- Views for reusable reports
- Transaction control with `COMMIT`, `ROLLBACK` and `SAVEPOINT`
- Automating tasks with stored procedures and triggers

## 👤 Author

**K V SIVA RAMA AJAY**
GitHub: [@github.com/RobinHood00786]

---
⭐ If you found this project helpful, give it a star!
